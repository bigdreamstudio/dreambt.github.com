---
layout: post
category : Linux
tags : [Linux, OpenSource]
title: 5大开源许可协议详解
excerpt: ! "笔者是个不折不扣的开源爱好者，不过大部分时候都是拿来主义。基本都是在使用，而使用过程中总会遇到各种各样的许可协议，例如GNU LGPL ,GNU
  GPL等等。很多开源软件发布过程中使用的许可协议都不相同，而且很多开源软件在您修改源码的同时需要将你现有的代码也以相同的许可方法发布，不知道您是否留意过这些呢？今天笔者带来一篇“5大开源许可协议详解”希望对大家使用开源产品时有一些帮助\r\n\r\n越来越多的开发者与设计者希望将自己的产品开源，以便其他人可以在他们的代码基础上做更多事，开源社区也因此充满生机。在我们所能想到的应用领域，
  都有开 源软件存在（象 WordPress，Drupal 这些开源CMS）。然而很多人对开源许可并不了解，本文介绍开源领域常用的几种许可协议以及它们之间的区别。"
wordpress_id: 180
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=180
date: 2010-04-02 12:24:39.000000000 +08:00
---
笔者是个不折不扣的开源爱好者，不过大部分时候都是拿来主义。基本都是在使用，而使用过程中总会遇到各种各样的许可协议，例如GNU LGPL ,GNU GPL等等。很多开源软件发布过程中使用的许可协议都不相同，而且很多开源软件在您修改源码的同时需要将你现有的代码也以相同的许可方法发布，不知道您是否留意过这些呢？今天笔者带来一篇“<strong>5大开源许可协议详解</strong>”希望对大家使用开源产品时有一些帮助
<p style="line-height: 19px;">越来越多的开发者与设计者希望将自己的产品开源，以便其他人可以在他们的代码基础上做更多事，开源社区也因此充满生机。在我们所能想到的应用领域， 都有开 源软件存在（象 WordPress，Drupal 这些开源CMS）。然而很多人对开源许可并不了解，本文介绍开源领域常用的几种许可协议以及它们之间的区别。</p>

<h3 style="color: #665500; font-size: 14px;">首先说说什么是许可协议？</h3>
<p style="line-height: 19px;">什么是许可，当你为你的产品签发许可，你是在出让自己的权利，不过，你仍然拥有版权和专利（如果申请了的话），许可的目的是，向使用你产品的人提供一定的 权限。</p>
<p style="line-height: 19px;">不管产品是免费向公众分发，还是出售，制定一份许可协议非常有用，否则，对于前者，你相当于放弃了自己所有的权利，任何人都没有义务表明你的原始作者身 份，对于后者，你将不得不花费比开发更多的精力用来逐个处理用户的授权问题。</p>
<p style="line-height: 19px;">而开源许可协议使这 些事情变得简单，开发者很容易向一个项目贡献自己的代码，它还可以保护你原始作者的身份，使你至少获得认可，开源许可协议还可以阻止其它人将某个产品据为 己有。以下是开源界的 5 大许可协议。</p>

<h3 style="color: #665500; font-size: 14px;">GNU GPL</h3>
<p style="line-height: 19px;">GNU General Public Licence (GPL) 有可能是开源界最常用的许可模式。GPL 保证了所有开发者的权利，同时为使用者提供了足够的复制，分发，修改的权利：</p>

<ol style="line-height: 19px;">
	<li><strong>可自由复制</strong></li>
	<li>你可以将软件复制到你的电脑，你客户的电脑，或者任何地方。复制份数没有任何限制。</li>
	<li><strong>可自由分发</strong></li>
	<li>在你的网站提供下载，拷贝到U盘送人，或者将源代码打印出来从窗户扔出去（环保起见，请别这样做）。</li>
	<li><strong>可以用来盈利</strong></li>
	<li>你可以在分发软件的时候收费，但你必须在收费前向你的客户提供该软件的 GNU GPL 许可协议，以便让他们知道，他们可以从别的渠道免费得到这份软件，以及你收费的理由。</li>
	<li><strong>可自由修改</strong></li>
	<li>如果你想添加或删除某个功能，没问题，如果你想在别的项目中使用部分代码，也没问题，唯一的要求是，使用了这段代码的项目也必须使用 GPL 协议。</li>
</ol>
<p style="line-height: 19px;">需要注意的是，分发的时候，需要明确提供源代码和二进制文件，另外，用于某些程序的某些协议有一些问题和限制，你可以看一下 @PierreJoye 写的 Practical Guide to GPL Compliance 一文。使用 GPL 协议，你必须在源代码代码中包含相应信息，以及协议本身。</p>

<h3 style="color: #665500; font-size: 14px;">GNU LGPL</h3>
<p style="line-height: 19px;">GNU 还有另外一种协议，叫做 LGPL （Lesser General Public Licence），它对产品所保留的权利比 GPL 少，总的来说，LGPL 适合那些用于非 GPL 或非开源产品的开源类库或框架。因为 GPL 要求，使用了 GPL 代码的产品必须也使用 GPL 协议，开发者不允许将 GPL 代码用于商业产品。LGPL 绕过了这一限制。</p>

<h3 style="color: #665500; font-size: 14px;">BSD</h3>
<p style="line-height: 19px;">BSD 在软件分发方面的限制比别的开源协议（如 GNU GPL）要少。该协议有多种版本，最主要的版本有两个，新 BSD 协议与简单 BSD 协议，这两种协议经过修正，都和 GPL 兼容，并为开源组织所认可。</p>
<p style="line-height: 19px;">新 BSD 协议（3条款协议）在软件分发方面，除需要包含一份版权提示和免责声明之外，没有任何限制。另外，该协议还禁止拿开发者的名义为衍生产品背书，但简单 BSD 协议删除了这一条款。</p>

<h3 style="color: #665500; font-size: 14px;">MIT</h3>
<p style="line-height: 19px;">MIT 协议可 能是几大开源协议中最宽松的一个，核心条款是：</p>
<p style="line-height: 19px;">该软件及其相关文档对所有人免费，可以任意处置，包括使用，复制，修改，合并，发表，分发，再授权，或者销售。唯一的限制是，软件中必须包含上述版权和许 可提示。</p>
<p style="line-height: 19px;">这意味着使用MIT您可以：</p>

<ol style="line-height: 19px;">
	<li>你可以自由使用，复制，修改，可以用于自己的项目。</li>
	<li>可以免费分发或用来盈利。</li>
	<li>唯一的限制是必须包含许可声明。</li>
	<li>MIT 协议是所有开源许可中最宽松的一个，除了必须包含许可声明外，再无任何限制。</li>
</ol>
<h3 style="color: #665500; font-size: 14px;">Apache</h3>
<p style="line-height: 19px;">Apache 协议 2.0 和别的开源协议相比，除了为用户提供版权许可之外，还有专利许可，对于那些涉及专利内容的开发者而言，该协议最适合（这里有一篇文章阐述这个问题）。</p>
<p style="line-height: 19px;">Apache 协议还有以下需要说明的地方:</p>

<ol style="line-height: 19px;">
	<li>永久权利</li>
	<li>一旦被授权，永久拥有。</li>
	<li>全球范围的权利</li>
	<li>在一个国家获得授权，适用于所有国家。假如你在美国，许可是从印度授权的，也没有问题。</li>
	<li>授权免费，且无版税</li>
	<li>前期，后期均无任何费用。</li>
	<li>授权无排他性</li>
	<li>任何人都可以获得授权</li>
	<li>授权不可撤消</li>
	<li>一旦获得授权，没有任何人可以取消。比如，你基于该产品代码开发了衍生产品，你不用担心会在某一天被禁止使用该代码。</li>
	<li>分发代码方面包含一些要求，主要是，要在声明中对参与开发的人给予认可并包含一份许可协议原文。</li>
</ol>
<h3 style="color: #665500; font-size: 14px;">Creative Commons</h3>
<p style="line-height: 19px;">Creative Commons (CC) 并非严格意义上的开源许可，它主要用于设计。Creative Commons 有多种协议，每种都提供了相应授权模式，CC 协议主要包含 4 种基本形式：</p>

<ol style="line-height: 19px;">
	<li>署名权
<ul style="line-height: 19px;">
	<li>必须为原始作者署名，然后才可以修改，分发，复制。</li>
</ul>
</li>
	<li>保持一致
<ul style="line-height: 19px;">
	<li>作品同样可以在 CC 协议基础上修改，分发，复制。</li>
</ul>
</li>
	<li>非商业
<ul style="line-height: 19px;">
	<li>作品可以被修改，分发，复制，但不能用于商业用途。但商业的定义有些模糊，比如，有的人认为非商业用途指的是不能销售，有的认为是甚至不能放在有广告的网 站，也有人认为非商业的意思是非盈利。</li>
</ul>
</li>
	<li>不能衍生新作品
<ul style="line-height: 19px;">
	<li>你可以复制，分发，但不能修改，也不能以此为基础创作自己的作品。</li>
</ul>
</li>
	<li>这些许可形式可以结合起来用，其中最严厉的组合是“署名，非商用，不能衍生新作品”，意味着，你可以分享作品，但不能改动或以此盈利，而且必须为原作者署 名。在这种许可模式下，原始作者对作品还拥有完全的控制权，而最宽松的组合是“署名”，意味着，只要为原始作者署名了，就可以自由处置。</li>
</ol>
[code]

以下就是一些版权声明的样例。

Apache:
/*
* Copyright (c) 2002-$today.year, jetmaven.net All Rights Reserved.
*
* Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);
* you may not use this file except in compliance with the License.
* You may obtain a copy of the License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an &quot;AS IS&quot; BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/
GPL:
/*
*
* Copyright (c) 2002-$today.year, jetmaven.net All Rights Reserved.
*
* This program is free software; you can redistribute it and/or
* modify it under the terms of the GNU General Public License as
* published by the Free Software Foundation; either version 2 of
* the License, or (at your option) any later version.
* This program is distributed in the hope that it will be
* useful, but WITHOUT ANY WARRANTY; without even the implied
* warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
* PURPOSE. See the GNU General Public License for more details.
* You should have received a copy of the GNU General Public
* License along with this program.if not, write to the Free
* Software Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139,
* USA.
*/

BSD:
/**
* Copyright (c) 2002-$today.year, planetes.de All rights reserved.
*
* Redistribution and use in source and binary forms, with
* or without modification, are permitted provided that the
* following conditions are met:
*
* * Redistributions of source code must retain the a
* bove copyright notice, this list of conditions and the
* following disclaimer.
* * Redistributions in binary form must reproduce the
* above copyright notice, this list of conditions and
* the following disclaimer in the documentation and/
* or other materials provided with the distribution.
* * Neither the name of the planetes.de, centaurus nor the
* names of its contributors may be used to endorse
* or promote products derived from this software
* without specific prior written permission.
*
* THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT
* HOLDERS AND CONTRIBUTORS &quot;AS IS&quot; AND ANY
* EXPRESS OR IMPLIED WARRANTIES, INCLUDING,
* BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
* MERCHANTABILITY AND FITNESS FOR A PARTICULAR P
* URPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
* COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
* ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
* EXEMPLARY, OR CONSEQUENTIAL DAMAGES
* (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
* SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA,
* OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
* CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER
* IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
* NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT
* OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF
* THE POSSIBILITY OF SUCH DAMAGE.
*/

Mozilla:
/**
* Copyright (c) 2002-$today.year, jetmaven.net All Rights Reserved.
*
* The contents of this file are subject to the Mozilla Public
* License Version 1.1 (the &quot;License&quot;); you may not use this file
* except in compliance with the License. You may obtain a copy of
* the License at http://www.mozilla.org/MPL/
*
* Software distributed under the License is distributed on an &quot;AS
* IS&quot; basis, WITHOUT WARRANTY OF ANY KIND, either express or
* implied. See the License for the specific language governing
* rights and limitations under the License.
*/

[/code]
