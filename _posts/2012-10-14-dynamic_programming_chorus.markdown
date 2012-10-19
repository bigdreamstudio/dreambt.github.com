---
layout: post
category : Algorithm
tags : [Dynamic_Programming, chorus, Algorithm]
title: 动态规划-合唱队形
tagline: 
date: 2012-10-14 15:09:11.000000000 +08:00
thumbnail: /assets/images/2012/10/chorus.gif
excerpt: N位同学站成一列，音乐老师要请其中的(N-K)位同学出列，使得剩下的K位同学排成合唱队形。<br>合唱队形是指这样一种队形，设K位同学从左到右依次编号为1、2...K,他们的身高分别为T1,T2,...,Tk,则他们的身高满足T1 <...< Ti < Ti+1>...>TK(1<=i<=K).<br>你的任务是，已知所有N位同学的身高，计算最少需要几位同学出列，可以使得剩下的同学排成合唱队形。
---
N位同学站成一列，音乐老师要请其中的(N-K)位同学出列，使得剩下的K位同学排成合唱队形。

合唱队形是指这样一种队形，设K位同学从左到右依次编号为1、2...K,他们的身高分别为T1,T2,...,Tk,则他们的身高满足T1 <...< Ti < Ti+1>...>TK(1<=i<=K).

你的任务是，已知所有N位同学的身高，计算最少需要几位同学出列，可以使得剩下的同学排成合唱队形。

## 输入文件

输入文件chorus.in的第一行是一个整数N(2<=N<=100)，表示同学的总数。第一行有n个整数，用空格分隔，第i个整数Ti(130<=Ti<=230)是第i位同学的身高(厘米)。

## 输出文件

输出文件chorus.out包括一行，这一行只包含一个整数，就是最少需要几位同学出列。

## 样例输入

    8
    186 186 150 200 160 130 197 220

## 样例输出

    4
    
问题分析：出列人数最少，也就是留的人数最多，也就是序列最长，这样问题也就变成了最长上升子序列和最长下降子序列的问题。

设T\[i\]为序列中的第i个人，对于T\[i\]分别求T\[0,...i\]的最长上升子序列长度len1\[i\]和T\[i+1,...N\]的最长下降子序列len2\[i\];则出队的同学数目为N-max\(len1\[i\]+len2\[i\]-1\),0<=i<=N-1.此算法的时间复杂度为O\(N^3\),空间复杂度为O\(N\).

其实只要对T\[i\]\(0<= i< N\)求一次最长上升子序列长度len1\[i\]和一次最长下降子序列长度len2\[i\].如此时间复杂度可为O(N^2),空间复杂度仍为O\(N\).

{% highlight c linenos %}
void chorus(int a[],int len) {
    int *len1=new int[len];
    int *len2=new int[len];
    int i=0;
    for(;i<len;i++) {
        len1[i]=1;
        len2[i]=1;
    }
    for(i=1;i<len;i++)
        for(int j=0;j<i;j++) {
            if(a[j]<a[i] &&(len1[j]+1>len1[i]))
                len1[i]=len1[j]+1; 
            if(a[j]>a[i] && (len2[j]+1<len2[i]))
                len2[i]=len2[j]+1;
    }
    int maxlen=len1[0]+len2[0];
    for(i=1;i<len;i++)
        if(len1[i]+len2[i]>maxlen) {
        // cout<<i<<endl;
        maxlen=len1[i]+len2[i];
    }
    cout<<len-maxlen+1<<endl;
    delete len1;
    delete len2;
}
{% endhighlight %}

转自：[http://liuyangxdgs.blog.163.com/blog/static/2913776320111144438848/](http://liuyangxdgs.blog.163.com/blog/static/2913776320111144438848/)