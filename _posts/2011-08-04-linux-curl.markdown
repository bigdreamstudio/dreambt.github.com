---
layout: post
category : Linux
tags : [Linux, curl, intro]
title: Linux下curl命令的使用
wordpress_id: 991
wordpress_url: http://www.im47.cn/?p=991
date: 2011-08-04 13:30:44.000000000 +08:00
---
1)初体验

curl http://www.yahoo.com

回车之后，www.yahoo.com 的html就稀里哗啦地显示在屏幕上了~

2)保存页面
curl http://www.yahoo.com &gt; page.html

或者用curl的内置option，存下http的结果
curl -o page.html http://www.yahoo.com

3)如果需要proxy代理

curl -x 123.45.67.89:1080 -o page.html http://www.yahoo.com

4)处理cookie信息
option: -D &lt;-- 这个是把http的response里面的cookie信息存到一个特别的文档中去
curl -x 123.45.67.89:1080 -o page.html -D cookie0001.txt http://www.yahoo.com

5）下一次访问的时候，如何继续使用上次留下的cookie信息呢？要知道，很多网站都是靠监控您的cookie信息，来判断您是不是不按规矩访问他们的网站的。
这次我们使用这个option来把上次的cookie信息追加到http request里面去： -b
curl -x 123.45.67.89:1080 -o page1.html -D cookie0002.txt -b cookie0001.txt http://www.yahoo.com

6）浏览器信息
让我们随意指定自己这次访问所宣称的自己的浏览器信息： -A
curl -A "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)" -x 123.45.67.89:1080 -o page.html -D cookie0001.txt http://www.yahoo.com

这样，服务器端接到访问的需要，会认为您是个运行在Windows 2000上的IE6.0，嘿嘿嘿，其实也许您用的是苹果机呢！
而"Mozilla/4.73 [en] (X11; U; Linux 2.2; 15 i686"则能够告诉对方您是一台PC上跑着的Linux，用的是Netscape 4.73，呵呵呵

7）另外一个服务器端常用的限制方法，就是检查http访问的referer。比如您先访问首页，再访问里面所指定的下载页，这第二次访问的referer地址就是第一次访问成功后的页面地址。这样，服务器端只要发现对下载页面某次访问的referer地址不 是首页的地址，就能够断定那是个盗连了~

幸好curl给我们提供了设定referer的option： -e
curl -A "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)" -x 123.45.67.89:1080 -e "mail.yahoo.com" -o page.html -D cookie0001.txt http://www.yahoo.com

这样，就能够骗对方的服务器，您是从mail.yahoo.com点击某个链接过来的了，呵呵呵

8）利用curl 下载文档

curl -o 1.jpg http://cgi2.tky.3web.ne.jp/~zzh/screen1.JPG
curl -O http://cgi2.tky.3web.ne.jp/~zzh/screen1.JPG //按照服务器上的文档名保存
curl -O http://cgi2.tky.3web.ne.jp/~zzh/screen[1-10].JPG //序列下载
curl -O http://cgi2.tky.3web.ne.jp/~/[001-201].JPG
curl -o #2_#1.jpg http://cgi2.tky.3web.ne.jp/~/[001-201].JPG //自定义文档名的下载

#1是变量，指的是这部分，第一次取值zzh，第二次取值nick
#2代表的变量，则是第二段可变部分---[001-201]，取值从001逐一加到201
这样，自定义出来下载下来的文档名，就变成了这样：
原来： ~zzh/001.JPG ---&gt; 下载后： 001-zzh.JPG
原来： ~nick/001.JPG ---&gt; 下载后： 001-nick.JPG

这样一来，就不怕文档重名啦，呵呵

9）断点续传

curl -c -O http://cgi2.tky.3wb.ne.jp/~zzh/screen1.JPG

10）分块下载 -r

比如我们有一个http://cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 要下载（赵老师的电话朗诵 :D ）
我们就能够用这样的命令：
curl -r 0-10240 -o "zhao.part1" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &amp;\
curl -r 10241-20480 -o "zhao.part2" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &amp;\
curl -r 20481-40960 -o "zhao.part3" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3 &amp;\
curl -r 40961- -o "zhao.part4" http:/cgi2.tky.3web.ne.jp/~zzh/zhao1.mp3
这样就能够分块下载啦。
但是您需要自己把这些破碎的文档合并起来
假如您用UNIX或苹果，用 cat zhao.part* &gt; zhao.mp3就能够
假如用的是Windows，用copy /b 来解决吧

上面讲的都是http协议的下载，其实ftp也相同能够用。
用法：
curl -u name:passwd ftp://ip:port/path/file
或
curl ftp://name:passwd@ip:port/path/file

11)上传 -T

比如我们向ftp传一个文档：

curl -T localfile -u name:passwd ftp://upload_site:port/path/

当然，向http服务器上传文档也能够
curl -T localfile http://cgi2.tky.3web.ne.jp/~zzh/abc.cgi
注意，这时候，使用的协议是HTTP的PUT method

http提交一个表单，比较常用的是POST模式和GET模式

GET模式什么option都不用，只需要把变量写在url里面就能够了
比如：
curl http://www.yahoo.com/login.cgi?user=nickwolfe&amp;password=12345

POST模式的option则是 -d
比如：

curl -d "user=nickwolfe&amp;password=12345" http://www.yahoo.com/login.cgi

就相当于向这个站点发出一次登陆申请~~~~~

到底该用GET模式还是POST模式，要看对面服务器的程式设定。

一点需要注意的是，POST模式下的文档上传，比如
&lt;form method="POST" enctype="multipar/form-data" action="http://cgi2.tky.3web.ne.jp/~zzh/up_file.cgi"&gt;
&lt;input type=file name=upload&gt;
&lt;input type=submit name=nick value="go"&gt;
&lt;/form&gt;
这样一个HTTP表单，我们要用curl进行模拟，就该是这样的语法：
curl -F upload=@localfile -F nick=go http://cgi2.tky.3web.ne.jp/~zzh/up_file.cgi

12）https的时候使用本地证书
curl -E localcert.pem https://remote_server

13）用curl通过dict协议去查字典~~~~~
curl dict://dict.org/d:computer
