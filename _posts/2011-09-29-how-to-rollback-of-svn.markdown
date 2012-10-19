---
layout: post
category : Version_Control
tags : [how-to, SVN]
title: 如何对SVN进行回滚操作
wordpress_id: 1010
wordpress_url: http://www.im47.cn/?p=1010
date: 2011-09-29 14:18:34.000000000 +08:00
---
打开CMD，cd到co目录。执行下面的操作：

	svn merge --dry-run -r73:68 http://svn.alibaba-inc.com/repos/ali_QA/20_Scripts/05picassowr_v2.0/install
	svn merge -r73:68 http://svn.alibaba-inc.com/repos/ali_QA/20_Scripts/05picassowr_v2.0/install
	svn commit -m "Reverted to revision 68."