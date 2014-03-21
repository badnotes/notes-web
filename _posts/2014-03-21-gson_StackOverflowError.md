---
layout: post
title: "一次异常处理,StackOverflowError"
tagline: "一次异常处理,StackOverflowError"
description: "badnotes,萬軍的个人网站,记录生活旅行代码.一次异常处理,StackOverflowError."
category: Java
tags: [Java]
---
{% include JB/setup %}

#####0.接手新项目
最近接手一个项目,大部分功能都已经完成的半残项目.10+个工程相互调用,根本没有文档,就连注释都少的可怜。是不是有点蛋疼呢,或许每个换新项目的人都是这个感受吧。

#####1.接口没检查参数
测试一个接口的时候，没有传入参数，居然返回我一堆java异常,StackOverflowError...

```java
type Exception report
message org.apache.cxf.interceptor.Fault
description The server encountered an internal error (org.apache.cxf.interceptor.Fault) that prevented it from fulfilling this request.
exception
java.lang.RuntimeException: org.apache.cxf.interceptor.Fault
	...
root cause
org.apache.cxf.interceptor.Fault
	...
root cause
java.lang.StackOverflowError
	java.lang.System.arraycopy(Native Method)
	java.lang.String.getChars(String.java:854)
	java.lang.AbstractStringBuilder.append(AbstractStringBuilder.java:391)
	java.lang.StringBuffer.append(StringBuffer.java:224)
	java.io.StringWriter.write(StringWriter.java:84)
	com.google.gson.stream.JsonWriter.string(JsonWriter.java:534)
	com.google.gson.stream.JsonWriter.writeDeferredName(JsonWriter.java:402)
	com.google.gson.stream.JsonWriter.nullValue(JsonWriter.java:431)
	com.google.gson.stream.JsonWriter.value(JsonWriter.java:415)
	com.google.gson.internal.bind.TypeAdapters$13.write(TypeAdapters.java:362)
	com.google.gson.internal.bind.TypeAdapters$13.write(TypeAdapters.java:346)
	com.google.gson.internal.bind.TypeAdapterRuntimeTypeWrapper.write(TypeAdapterRuntimeTypeWrapper.java:68)
	com.google.gson.internal.bind.ReflectiveTypeAdapterFactory$1.write(ReflectiveTypeAdapterFactory.java:89)
	com.google.gson.internal.bind.ReflectiveTypeAdapterFactory$Adapter.write(ReflectiveTypeAdapterFactory.java:195)
	com.google.gson.internal.bind.TypeAdapterRuntimeTypeWrapper.write(TypeAdapterRuntimeTypeWrapper.java:68)
	com.google.gson.internal.bind.ReflectiveTypeAdapterFactory$1.write(ReflectiveTypeAdapterFactory.java:89)
	com.google.gson.internal.bind.ReflectiveTypeAdapterFactory$Adapter.write(ReflectiveTypeAdapterFactory.java:195)
```

#####2.捕获异常
项目用cxf做的 WebService, 在这里catch到.

cxf-rt-core:AbstractInvoker.java

```java
 protected Object invoke(Exchange exchange, final Object serviceObject, Method m, List<Object> params) {
        Object res;
        try {
            Object[] paramArray = new Object[]{};
            if (params != null) {
                paramArray = params.toArray();
            }
            res = performInvocation(exchange, serviceObject, m, paramArray);
            if (exchange.isOneWay()) {
                return null;
            }
            return new MessageContentsList(res);
        } catch (InvocationTargetException e) {
            Throwable t = e.getCause();
            if (t == null) {
                t = e;
            }
            // ...
		}
```

#####3.异常点
使用google的gson将数据转换为json格式.在 toJson 的时候 抽象类TypeAdapter使用了TypeAdapterRuntimeTypeWrapper的实现

Gson.java

```java
  public void toJson(Object src, Type typeOfSrc, JsonWriter writer) throws JsonIOException {
    TypeAdapter<?> adapter = getAdapter(TypeToken.get(typeOfSrc));
    boolean oldLenient = writer.isLenient();
    writer.setLenient(true);
    boolean oldHtmlSafe = writer.isHtmlSafe();
    writer.setHtmlSafe(htmlSafe);
    boolean oldSerializeNulls = writer.getSerializeNulls();
    writer.setSerializeNulls(serializeNulls);
    try {
      ((TypeAdapter<Object>) adapter).write(writer, src);
    } catch (IOException e) {
      throw new JsonIOException(e);
    } finally {
      writer.setLenient(oldLenient);
      writer.setHtmlSafe(oldHtmlSafe);
      writer.setSerializeNulls(oldSerializeNulls);
    }
  }
```

在TypeAdapterRuntimeTypeWrapper中一直在循环调用自己类的write方法.

```java
package com.google.gson.internal.bind;
final class TypeAdapterRuntimeTypeWrapper<T> extends TypeAdapter<T>

 @SuppressWarnings({"rawtypes", "unchecked"})
  @Override
  public void write(JsonWriter out, T value) throws IOException {
    // Order of preference for choosing type adapters
    // First preference: a type adapter registered for the runtime type
    // Second preference: a type adapter registered for the declared type
    // Third preference: reflective type adapter for the runtime type (if it is a sub class of the declared type)
    // Fourth preference: reflective type adapter for the declared type

    TypeAdapter chosen = delegate;
    Type runtimeType = getRuntimeTypeIfMoreSpecific(type, value);
    if (runtimeType != type) {
      TypeAdapter runtimeTypeAdapter = context.getAdapter(TypeToken.get(runtimeType));
      if (!(runtimeTypeAdapter instanceof ReflectiveTypeAdapterFactory.Adapter)) {
        // The user registered a type adapter for the runtime type, so we will use that
        chosen = runtimeTypeAdapter;
      } else if (!(delegate instanceof ReflectiveTypeAdapterFactory.Adapter)) {
        // The user registered a type adapter for Base class, so we prefer it over the
        // reflective type adapter for the runtime type
        chosen = delegate;
      } else {
        // Use the type adapter for runtime type
        chosen = runtimeTypeAdapter;
      }
    }
    chosen.write(out, value);
  }
```

#####4.测试
不就是这么个对象么，为什么会使gson陷入循环呢？难道是gson的Bug?自己构建个对象来toJson试试.

```java
RespObj obj = new RespObj();
obj.setCode("10199");
obj.setType("PARAMETEREXCEPTION");
obj.setMessage("java.lang.NullPointerException");
new Gson().toJson(obj);
```

自己构造了这个对象，调用gson并没有异常呢。

#####5.原因在哪？

对比了下，原来项目中，message并不是字符串"java.lang.NullPointerException"，
而是 java.lang.NullPointerException 对象...

这么做就会看到给异常了：

```java
new Gson().toJson(new NullPointerException());
```

#####6.关于异常处理结论

* 原来所有的异常继承自Exception，Exception继承 Throwable，Throwable有个this指针.导致toJson的时候产生了循环.好吧，Gson并没有这个bug，是开发人员自己作死

* 对用户输入进行检测

* 最好是不要直接把异常抛给用户，大部分用户是看不懂的

* 内部异常往外抛,在最外层捕获，以友好的方式显示给用户

* 在最外层统一处理,尽量避免在每个servlet去处理

* 而且配置404,500返回页面，不要直接将异常打印
