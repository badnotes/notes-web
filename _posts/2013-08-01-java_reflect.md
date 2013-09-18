---
layout: post
title: "java反射之动态代理"
description: "java反射之动态代理"
category: java
tags: java
---
{% include JB/setup %}

	public interface HelloWorld {
	    void sayHelloWorld() ;
	}

	public class HelloWorldImpl implements HelloWorld {
	    @Override
	    public void sayHelloWorld() {
	        System.out.println("Hello World!");
	    }
	}

 
	public class HelloWorldHandler implements InvocationHandler {
		// original object
		private Object oriObject;
		/**
		 * constructor 　　
		 * @param Object original　　
		 */
		public HelloWorldHandler(Object obj) {
			this.oriObject = obj;
		}
		public Object invoke(Object proxy, Method method, Object[] args)
				throws Throwable {
			Object result;
			// before invoke
			doBefore();
			// invoke original method
			result = method.invoke(this.oriObject, args);
			// after invoke
			doAfter();
			return result;
		}
		private void doBefore() {
			System.out.println("before method invoke");
		}
		private void doAfter() {
			System.out.println("after method invoke");
		}
		public static void main(String[] args) {

			HelloWorld hw = new HelloWorldImpl();
			InvocationHandler handler = new HelloWorldHandler(hw);
			HelloWorld proxy = (HelloWorld) Proxy.newProxyInstance(hw.getClass().getClassLoader(),hw.getClass().getInterfaces(),handler);
			proxy.sayHelloWorld();
		}
	}

 