---
layout: mypost
title: "用Java通过正则表达式提取字符串中的内容"
tagline: "用Java通过正则表达式提取字符串中的内容"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。用Java通过正则表达式提取字符串中的内容。"
category: Java Regexp
tags: Java Regexp
---



提取出网页中的__VIEWSTATE

	String html = "<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="jGNjNeEiiu5V3nFyg3edj77IIl5u5I54SG7m+S\" />";
	Pattern pattern = Pattern.compile("id=\"__VIEWSTATE\" value=\"(\\S*)\"");
	Matcher matcher = pattern.matcher(html);
	if(matcher.find()){ // 这里只用了一次匹配,多次匹配用while
	    System.out.println(matcher.group(0));
        System.out.println(matcher.group(1)); // 筛选出第一个括号内的内容
	}


输出结果：

	id="__VIEWSTATE" value="jGNjNeEiiu5V3nFyg3edj77IIl5u5I54SG7m+S"
	jGNjNeEiiu5V3nFyg3edj77IIl5u5I54SG7m+S