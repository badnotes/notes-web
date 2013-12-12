---
layout: post
title: ruby rails delete method error
description: badnotes,萬軍的个人网站，记录生活旅行代码。ruby rails
category: Ruby
tags: Ruby Rails
---

rails 中 delete 方法无效


### 1.环境

    ruby 2.0.0p0 (2013-02-24 revision 39474) [x86_64-linux]
    Rails 3.2.12
    jquery-rails (2.2.1)
    浏览器Chromium：Version 24.0.1312.56 Ubuntu 12.10 (24.0.1312.56-0ubuntu0.12.10.3)
 
### 2.ruby code

    = link_to I18n.t('delete'), location, method: :delete, data: { confirm: 'Are you sure?' } ,class: "btn btn-danger"
生成的html

### 3.Delete
点击后服务端控制台：

    Started GET "/patients/4" for 127.0.0.1 at 2013-03-15 08:34:43 +0800
    Processing by PatientsController#show as HTML
    Parameters: {"id"=>"4"}
    
### 4.修正方案：

    1.add gem:
        gem 'jquery-rails'
    2.add require
        //= require jquery
        //= require jquery_ujs
    3.Make sure that, along with your other changes you change:
        = javascript_include_tag "application"
        to
        = javascript_include_tag :defaults 
        or
        = javascript_include_tag :all 
    4.and scrf: 
        = csrf_meta_tags

我把这改来改去，都试过了，还是不能用...
后来发现我把 //= require jquery ， //= require jquery_ujs 这两个顺序写反了，改回来之后就可以使用delete方法了，然后我把第3条换了各种写法和第4条去掉更本就不会影响 delete 的。
原来这里的jquery-ujs是依赖jquery的，所以jquery必须先导入，
jquery-ujs 中关于绑定事件的原话：

    // If browser does not support submit bubbling, then this live-binding will be called before direct
    // bindings. Therefore, we should directly call any direct bindings before remotely submitting form.
    if (!$.support.submitBubbles && $().jquery < '1.7' && rails.callFormSubmitBindings(form, e) === false) return rails.stopEverything(e);

    rails.handleRemote(form);
    return false;

jQuery-ujs： 全名 Unobtrusive scripting adapter for jQuery
非侵入式的JavaScript），以实现对HTML和JavaScript代码的分离
不再有多余的js代码，是用html5的data-元素实现的,因此如果事件没有绑定到的话，点击delete会跳到show的action中去,就是用 get 方法提交的.

HTTP 有四种 request method：POST、GET、PUT、DELETE，但是 HTML 表单却只有 POST 和 GET 两种可以用，但是 Rails 的RESTful API定义的七种动作中，把四种 request method 都用到了，所以为了让 PUT 和 DELETE 也可以执行， jQuery-ujs 会在必要的时候，多送一个 _method 参数，告诉 server 端，现在实际上是哪种 request 。