---
layout: post
category : Version Control
tags : [intro, beginner, SVN, trunk, branches, tags]
title: SVN trunk, branches and tags
wordpress_id: 870
wordpress_url: http://www.im47.net/?p=870
date: 2011-03-12 21:20:03.000000000 +08:00
---
<em>因水平所限，如果翻译得和原文有差，敬请评论指正。</em>

在本篇文章中, 我将会详细说明我是如何应用SVN trunk(树干)、branches(分支)和tags(标记)。这种方法同样被称为“branch always”，两者非常接近。可能我所介绍的并不是最好的方法，但是它会给新手一些解释说明，告诉他们trunk、branches和tags是什么，并且该如何去应用它们。

当然，如果本文有些要点需要澄清/确认，亦或者有一些错误的观点，还请你评论，自由发表自己的观点。

<strong>——简单的对比</strong>

SVN的工作机制在某种程度上就像一颗正在生长的树：
<ul>
	<li>一棵有树干和许多分支的树</li>
	<li>分支从树干生长出来，并且细的分支从相对较粗的树干中长出</li>
	<li>一棵树可以只有树干没有分支（但是这种情况不会持续很久，随着树的成长，肯定会有分支啦，^^）</li>
	<li>一颗没有树干但是有很多分支的树看起来更像是地板上的一捆树枝</li>
	<li>如果树干患病了，最终分支也会受到影响，然后整棵树就会死亡</li>
	<li>如果分支患病了，你可以剪掉它，然后其他分支还会生长出来的哦！</li>
	<li>如果分支生长太快了，对于树干它可能会非常沉重，最后整棵树会垮塌掉</li>
	<li>当你感觉你的树、树干或者是分支看起来很漂亮的时候，你可以给它照张相，这样就就可以记得它在那时是多么的赞。</li>
</ul>
<strong>——Trunk</strong>

Trunk是放置稳定代码的主要环境，就好像一个汽车工厂，负责将成品的汽车零件组装在一起。

以下内容将告诉你如何使用SVN trunk：
<ul>
	<li>
<div>除非你必须处理一些容易且能迅速解决的BUG，或者你必须添加一些无关逻辑的文件（比如媒体文件：图像，视频，CSS等等），否则永远不要在trunk直接做开发</div></li>
	<li>
<div>不要因为特殊的需求而去对先前的版本做太大的改变，如何相关的情况都意味着需要建立一个branch（如下所述）</div></li>
	<li>
<div>不要提交一些可能破坏trunk的内容，例如从branch合并</div></li>
	<li>
<div>如果你在某些时候偶然间破坏了trunk，bring some cake the next day (”with great responsibilities come… huge cakes”)</div></li>
</ul>
<strong>——Branches</strong>

一个branch就是从一个SVN仓库中的子树所作的一份普通拷贝。通常情况它的工作类似与UNIX系统上的符号链接，但是你一旦在一个SVN branch里修改了一些文件，并且这些被修改的文件从拷贝过来的源文件独立发展，就不能这么认为了。当一个branch完成了，并且认为它足够稳定的时候，它必须合并回它原来的拷贝的地方，也就是说：如果原来是从trunk中拷贝的，就应该回到trunk去，或者合并回它原来拷贝的父级branch。

以下内容将告诉你如何使用SVN branches：
<ul>
	<li>
<div>如果你需要修改你的应用程序，或者为它开发一个新的特性，请从trunk中创建一个新的branch，然后基于这个新的分支进行开发</div></li>
	<li>
<div>除非是因为必须从一个branch中创建一个新的子branch，否则新的branch必须从trunk创建</div></li>
	<li>
<div>当你创建了一个新branch，你应当立即切换过去。如果你没有这么做，那你为什么要在最初的地方创建这个分支呢？</div></li>
</ul>
<strong>——Tags</strong>

从表面上看，SVN branches和SVN tags没有什么差别，但是从概念上来说，它们有许多差别。其实一个SVN tags就是上文所述的“为这棵树照张相”：一个trunk或者一个branch修订版的命名快照。

以下内容将告诉你如何使用SVN tags：
<ul>
	<li>
<div>作为一个开发者，永远不要切换至、取出，或者向一个SVN tag提交任何内容：一个tag好比某种“照片”，并不是实实在在的东西，tags只可读，不可写。</div></li>
	<li>
<div>在特殊或者需要特别注意的环境中，如：生产环境（production）、？（staging）、测试环境（testing）等等，只能从一个修复过的（fixed）tag中checkout和update，永远不要commit至一个tag。</div></li>
	<li>
<div>对于上述提及到的环境，可以创建如下的tags：“production”，“staging”，“testing”等等。你也可以根据软件版本、项目的成熟程度来命名tag：“1.0.3”，“stable”，“latest”等等。</div></li>
	<li>
<div>当trunk已经稳定，并且可以对外发布，也要相应地重新创建tags，然后再更新相关的环境（production, staging, etc）</div></li>
</ul>
<strong>——工作流样例</strong>

假设你必须添加了一个特性至一个项目，且这个项目是受版本控制的，你差不多需要完成如下几个步骤：
<ol>
	<li>
<div>使用SVN checkout或者SVN switch从这个项目的trunk获得一个新的工作拷贝（branch）</div></li>
	<li>
<div>使用SVN切换至新的branch</div></li>
	<li>
<div>完成新特性的开发（当然，要做足够的测试，包括在开始编码前）</div></li>
	<li>
<div>一旦这个特性完成并且稳定（已提交），并经过你的同事们确认，切换至trunk</div></li>
	<li>
<div>合并你的分支至你的工作拷贝（trunk），并且解决一系列的冲突</div></li>
	<li>
<div>重新检查合并后的代码</div></li>
	<li>
<div>如果可能的话，麻烦你的同事对你所编写、更改的代码进行一次复查（review）</div></li>
	<li>
<div>提交合并后的工作拷贝至trunk</div></li>
	<li>
<div>如果某些部署需要特殊的环境（生成环境等等），请更新相关的tag至你刚刚提交到trunk的修订版本</div></li>
	<li>
<div>使用SVN update部署至相关环境</div></li>
</ol>
<strong>翻译者：zwws
原　文：</strong><a href="http://www.jmfeurprier.com/blog/2010/02/08/svn-trunk-branches-and-tags/" target="_blank"><strong>SVN trunk, branches and tags</strong></a>
<strong>译　言：<a href="http://article.yeeyan.org/view/132319/81358">http://article.yeeyan.org/view/132319/81358</a></strong>
<strong>转载请注明<a href="http://www.zvv.cn/blog/show-111-1.html">原链接</a>，谢谢。</strong>
