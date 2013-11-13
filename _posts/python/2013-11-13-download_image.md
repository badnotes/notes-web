---
layout: post
title: "python批量下载图片"
tagline: "python批量下载图片"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。python批量下载图片。"
category: python
tags: [python]
---
{% include JB/setup %}

0.在开发一些互联网项目的时候，常常需要用脚本去网上抓取一些原始数据。今天写的用python脚本去下载一些图片。原理也很简单，就是读取网页页面，用正则匹配分析图片路径，批量下载。主要用的模块urllib,re,BeautifulSoup。需要注意的是python代码有严格的格式要求，抓取时速度不能太快了，大部分网站都会有限制ip的。另外，大部分网站的数据拉取会用js隐藏起来，很难通过页面分析出来批量处理，通过分析js代码也很难做到，这时我们可以借助浏览器，比如chrome的开发者工具，可以查看网络(network)的所有请求即响应。这便很容易分析出想要的信息。


1.导入包，定义全局变量

	#!/usr/bin/python
	#encoding: utf-8

	import time,math,os,re,urllib,urllib2,cookielib
	from bs4 import BeautifulSoup

	class Image(object):
	  image_links = [] #图片URL链接
	  image_dir = '/data/image/' #图片存放文件夹
	  image_count = 0 #已下载图片数量
	  current_pages = [] #页面链接地址

2.浏览器信息

	def __init__(self):
	  self.cj =cookielib.LWPCookieJar()
	  self.opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(self.cj))
	  urllib2.install_opener(self.opener)
	  self.opener.addheaders = [
		("User-agent", "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.22 (KHTML, like Gecko) Ubuntu Chromium/25.0.1364.160"),
		("Accept", "*/*")]

3.获取页面地址

	def get_page_link(self,url):
	  self.current_pages = []
	  try:
		html = self.opener.open(url).read()
		ids = []
		names = []
		for pid in re.findall(r'\.id=\d*',str(html)):
		  ppid = re.findall(r'\d{1,}',pid)[0]
		  ids.append(ppid)
		for pname in re.findall(r'\.domainName="[A-Za-z0-9_-]*"',str(html)):
		  ppname = re.findall(r'="[A-Za-z0-9_-]{1,}',pname)[0]
		  pppname = re.findall(r'[A-Za-z0-9_-]{1,}',ppname)[0]
		  names.append(pppname)
		for i in range(0,len(ids)):
		  page = 'http://pp.xxx.com/'+ names[i] + '/pp/' + str(ids[i]) + '.html'
		  self.current_pages.append(page)
		except Exception, e:
		  self.write_log('get page link error:%s' %e)
		  return

4.获取页面图片地址

	def get_images_link(self):
	  self.image_links = []
	  for page_link in self.current_pages:
		try:
		  html = self.opener.open(page_link).read()
		except Exception, e:
		  self.write_log('get image link error:%s' %e)
		  print 'get image link error:%s' %e
		  return
		soup = BeautifulSoup(html)
		for link in soup.findAll('img',{'data-lazyload-src':re.compile('http://img2.xxx.net/')}):
		  if 'data-lazyload-src="http://img2.xxx.net/' in str(link):
			l = link['data-lazyload-src']
			#l = re.findall(r'.*data-lazyload-src=(.*)',link['href'])[0]
			self.image_links.append(l)

5.图片下载

	def download(self):
	  if not self.image_links:
		return False
	  for link in self.image_links:
		try:
		  data = urllib.urlopen(link).read()
		except Exception, e:
		  self.write_log('connect error:%s' %e)
		  continue
		self.write_log('downloading ... - %s' %link)
		file_name = str(int(time.time())) + '.jpg'
		file_path = os.path.join(self.image_dir,file_name)
		image = open(file_path,'wb')
		try:
		  image.write(data)
		except Exception, e:
		  self.write_log('faild:%s' %e)
		else:
		  self.write_log('success:%s' %link)
		self.image_count += 1
		image.close
		del image
		time.sleep(2)
	  pass

6.日志

	def write_log(self,text):
		print text
		log = open('log.txt','a')
		log.write(text)
		log.write('\n')
		log.close

7.运行

	def run(self,url):
		self.get_page_link(url)
		self.get_images_link()
		self.download()

8.main方法

	if __name__ == '__main__':
	    for i in range(1,500): # page range 1 to 500
		url = 'http://www.xxxx.com/?page=' + str(i)
		page_log = '+++++++++++++++++++++ page ' + str(i) + '+++++++++++++++'
		print page_log
		img = Image()
		img.write_log(page_log)
		img.run(url)
		
