---
layout: post
category : Algorithm
tags : [MD5, hash, Base64, Algorithm]
title: 用MD5做散列函数的新Base64算法
wordpress_id: 928
wordpress_url: http://www.im47.net/?p=928
date: 2011-04-27 21:03:38.000000000 +08:00
---
吃完晚饭没事，随便写了点东西，没想到弄巧成拙，写出来一堆代码，仁者见仁智者见智。大家拿去用吧，o(∩_∩)o 哈哈~

[code]
public class WL {
private int base = 63;
private String base64 = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz-_";

public String getWL(String url) {
String hex = MD5.getMD5(url.getBytes());
int hexLen = hex.length();
int subHexLen = hexLen / 8;
StringBuffer result = new StringBuffer();
for (int i = 0; i &lt; subHexLen; ++i) {
String subHex = hex.substring(8 * i, 8 * i + 7);
Long num = 0x3FFFFFFF &amp; Long.parseLong(subHex, 16);
for (int j = 0; j &lt; 5; ++j) {
Long val = 0x0000003F &amp; num;
result.append(base64.charAt(val.intValue()));
num = num &gt;&gt; 6;
}
result.append("|");
}
return result.toString().substring(0, 23);
}

public static void main(String[] args) {
WL wl = new WL();
System.out.println(wl.getWL("http://www.im47.net/"));
}
}
[/code]
