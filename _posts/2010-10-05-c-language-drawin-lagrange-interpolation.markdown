---
layout: post
category : C
tags : [intro, beginner, C]
title: C语言绘图-拉格朗日插值
wordpress_id: 532
wordpress_url: http://www.im47.cn/?p=532
date: 2010-10-05 20:18:14.000000000 +08:00
---
最近帮一“好朋友”做导师布置的作业，要求用拉格朗日求点并绘图。

我选用的是C语言，一则重温一下C语言，二则因为大学系主任在开学时说过的那句话，“当你能用C语言绘图的时候，你就学得很好了”。当然，我并没有学好 :oops: 。

在这里我利用了<a href="http://www.rupeng.com/forum/viewthread.php?tid=12260" target="_blank">ege图形库</a>，绘图很方便。当然用MFC也很方便，MFC的我没做，我朋友做了。我一向不喜欢封装的东西，我喜欢了解底层。废话少说，看题：

问题的关键还是利用拉格朗日求点，公式自己去谷歌回来。其他的都好做了，唯一需要注意的就是绘图坐标系的坐标原点在左上角，而y轴是向下的。好了，有了这些基本上就可以实现了。如果要做的好一点可以考虑绘制坐标轴和刻度，同时对数据进行修改使得图像不致于显示不开或者太小了看不见。数据是从data.txt中读取的，数据存放格式为：第一行为已知点个数n，第二行开始到第n+1行为已知点坐标以空格或Tab隔开，第n+2行为未知点横坐标。

1.y=sin(x) 101个点 待求点3.14

<a href="wp-content/uploads/2010/10/2010-10-05_194858.jpg"><img class="alignnone size-full wp-image-534" title="2010-10-05_194858" src="wp-content/uploads/2010/10/2010-10-05_194858.jpg" alt="" width="637" height="480" /></a>

2.y=cos(x)<span style="line-height: 10px;">1 101个点 待求点1.1212</span>

<a href="wp-content/uploads/2010/10/2010-10-05_194837.jpg"><img class="alignnone size-full wp-image-533" title="2010-10-05_194837" src="wp-content/uploads/2010/10/2010-10-05_194837.jpg" alt="" width="642" height="482" /></a>

[c]#include&lt;stdio.h&gt;
#include&lt;stdlib.h&gt;
#include&lt;float.h&gt;
#include &quot;graphics.h&quot;

float laglange(float *x,float *y,float a,int n){
	int i,j;
	float h,b=0.0;
	for(i=0;i&lt;=n-1;i++){
		h=1.0;
	    for(j=0;j&lt;=n-1;j++){
			if(i!=j) h=h*(a-x[j])/(x[i]-x[j]);
		}
		b=b+y[i]*h;
	}
	return b;
}

// x、y同时缩放，图像可能显示不全
void draw(float *x,float *y,int n,float t,float yt,float maxX,float minX,float maxY,float minY){
	int i;
	float tx,ty,zoom,floatX;
	float width=640,height=480;

	zoom=min(width/(maxX-minX),height/(maxY-minY));
	floatX=-minX*zoom;

	// 初始化
	init();

	// 坐标轴
	setcolor(GREEN);
	line(0,height/2,640,height/2);
	line(floatX,0,floatX,480);

	// 绘图
	setcolor(WHITE);
	for(i=0;i&lt;n;i++){
		line(x[i]*zoom+floatX,height/2-y[i]*zoom,x[i+1]*zoom+floatX,height/2-y[i+1]*zoom);
	}

	// 待求点(t,yt)
	tx=t*zoom+floatX;
	ty=height/2-yt*zoom;
	outtextxy(tx+5,ty-15, &quot;待求点&quot;);
	setcolor(RED);
	putpixel(tx,ty,RED);
	circle(tx,ty,1);
	circle(tx,ty,2);
	circle(tx,ty,3);

	getch();
	closegraph();
}

// x、y单独缩放,部分图像可能变形
void draw2(float *x,float *y,int n,float t,float yt,float maxX,float minX,float maxY,float minY){
	int i;
	float tx,ty,zoomX,zoomY,floatX;
	float width=640,height=480;

	zoomX=width/(maxX-minX);
	zoomY=height/(maxY-minY)/2.0;
	floatX=-minX*zoomX;

	// 初始化
	int g = DETECT, m = 0;
    initgraph(&amp;g, &amp;m, &quot;拉格朗日计算&quot;);

	// 坐标轴
	setcolor(GREEN);
	line(0,height/2,640,height/2);
	line(floatX,0,floatX,480);

	// 绘图
	setcolor(WHITE);
	for(i=0;i&lt;n;i++){
		line(x[i]*zoomX+floatX,height/2-y[i]*zoomY,x[i+1]*zoomX+floatX,height/2-y[i+1]*zoomY);
	}

	// 待求点(t,yt)
	tx=t*zoomX+floatX;
	ty=height/2-yt*zoomY;
	outtextxy(tx+5,ty-15, &quot;待求点&quot;);
	setcolor(RED);
	putpixel(tx,ty,RED);
	circle(tx,ty,1);
	circle(tx,ty,2);
	circle(tx,ty,3);

	getch();
	closegraph();
}

void main(){
	float x[300],y[300],t,yt;
    int i,n;
	float maxX=FLT_MIN,minX=FLT_MAX,maxY=FLT_MIN,minY=FLT_MAX;
	FILE *fp;

	if((fp=fopen(&quot;data.txt&quot;,&quot;r&quot;))==NULL){
		printf(&quot;File cannot be opened!\n&quot;);
		exit(1);
	}
	else{
		// 读取点集合的大小、数据和待求数据
		fscanf(fp,&quot;%d&quot;,&amp;n);
		for(i=0;i&lt;n;i++){
			fscanf(fp,&quot;%f %f&quot;,&amp;x[i],&amp;y[i]);
			maxX=max(maxX,x[i]);
			minX=min(minX,x[i]);
			maxY=max(maxY,y[i]);
			minY=min(minY,y[i]);
		}
		fscanf(fp,&quot;%f&quot;,&amp;t);

		// 输出
		for(i=0;i&lt;n;i++){
			printf(&quot;%f %f&quot;,x[i],y[i]);
		}

		// 计算拉格朗日
		yt=laglange(x,y,t,n-1);
		printf(&quot;Result: %f&quot;,yt);

		// 绘图
		draw2(x,y,n-1,t,yt,maxX,minX,maxY,minY);
	}

	if(fclose(fp)!=0){
		printf(&quot;File cannot be closed!\n&quot;);
		exit(1);
	}
}[/c]

下面是她用MFC做的，这个实现的是利用已知点通过拉格朗日插值求出其他点然后连接各点，与我做的题目有些区别：
<p style="text-align: center;"><a href="wp-content/uploads/2010/10/lg1.jpg"><img class="aligncenter size-full wp-image-578" title="lg1" src="wp-content/uploads/2010/10/lg1.jpg" alt="" width="640" height="422" /></a><a href="wp-content/uploads/2010/10/lg2.jpg"><img class="aligncenter size-full wp-image-579" title="lg2" src="wp-content/uploads/2010/10/lg2.jpg" alt="" width="660" height="444" /></a></p>
