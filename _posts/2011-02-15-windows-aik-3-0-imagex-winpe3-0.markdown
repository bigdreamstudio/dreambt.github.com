---
layout: post
category : Windows
tags : [AIK, ImageX, tutorial, winPE]
title: 使用Windows AIK 3.0制作带ImageX的winPE3.0
wordpress_id: 863
wordpress_url: http://www.im47.net/?p=863
date: 2011-02-15 14:46:31.000000000 +08:00
---
昨晚刚下载了win7 AIK中文版，看了下里面的技术文档，以下是制作一个简单的winpe3.0的方法

Win7 AIK中文版下载地址：<a href="http://www.microsoft.com/downloads/zh-cn/details.aspx?FamilyID=696dd665-9f76-4177-a811-39c26d3b3b34&amp;displaylang=zh-CN" target="_blank">http://www.microsoft.com/downloads/zh-cn/details.aspx?FamilyID=696dd665-9f76-4177-a811-39c26d3b3b34&amp;displaylang=zh-CN</a>
<h1>步骤 1：设置 Windows PE 构建环境</h1>
<div id="sectionSection1">

在此步骤中，创建支持构建 Windows PE 映像的所需目录结构。
<ol>
	<li>在您的技术人员计算机上，单击<strong>“开始”</strong>，依次指向<strong>“所有程序”</strong>、<strong>“Windows OPK”</strong>或<strong>“Windows AIK”</strong>，右键单击<strong>“部署工具命令提示符”</strong>，然后选择<strong>“以管理员身份运行”</strong>。
菜单快捷方式将打开“命令提示符”窗口，并将环境变量自动设置为指向所有必需的工具。默认情况下，所有工具都安装在  C:\Program Files\&lt;version&gt;\Tools 中，其中 <em>&lt;version&gt;</em> 可以是 Windows  OPK 或 Windows AIK。</li>
	<li>在命令提示符下，运行 Copype.cmd 脚本。
该脚本需要使用两个参数：硬件体系结构和目标位置。例如，
<code>copype.cmd  &lt;architecture&gt;  &lt;destination&gt;</code>
其中，<em>&lt;architecture&gt;</em> 可以是 x86、amd64 或 ia64，<em>&lt;destination&gt;</em> 是指向本地目录的路径。例如，
<code>copype.cmd x86  c:\winpe_x86</code>
此脚本会创建以下目录结构并复制该体系结构的所有必要文件。例如，
<code>\winpe_x86</code>
<code>\winpe_x86\ISO</code>
<code>\winpe_x86\mount</code></li>
	<li>将基本映像 (Winpe.wim) 复制到 \Winpe_x86\ISO\sources 文件夹，然后将该文件重命名为 Boot.wim。
<div>
<table cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<th align="left"></th>
</tr>
<tr>
<td colspan="2">
<pre>copy c:\winpe_x86\winpe.wim c:\winpe_x86\ISO\sources\boot.wim</pre>
</td>
</tr>
</tbody>
</table>
</div></li>
</ol>
</div>
<h1>步骤 2：（可选）添加其他自定义</h1>
<div id="sectionSection2">

此步骤为可选步骤，但是建议执行此步骤。

在 Windows PE 中工作时，使用 ImageX，可以将应用程序和脚本添加到可能需要的 Windows PE 映像中。ImageX  是一个在实现部署方案期间捕获和应用映像的工具。例如，在命令提示符下键入：

<code>copy "C:\program  files\&lt;version&gt;\Tools\&lt;architecture&gt;\imagex.exe"  C:\winpe_x86\iso\</code>

其中，<em>&lt;version&gt;</em> 可以是 Windows OPK 或  Windows AIK，<em>&lt;architecture&gt;</em> 可以是 x86、amd64 或 a64。在上两例中，Windows PE RAM  引导过程中不会将工具加载到内存。访问这些工具时，介质必须可用。

</div>
<h1>步骤 3：创建可引导 CD-ROM。</h1>
<div id="sectionSection3">

此步骤讲述如何将 Windows PE RAM 盘放到 CD-ROM 上。此选项要求您使用 Oscdimg 工具创建 .iso  文件。
<ol>
	<li>在技术人员计算机上，使用 Oscdimg 创建 .iso 文件。在命令提示符下，键入：
<div>
<table cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<th align="left"></th>
</tr>
<tr>
<td colspan="2">
<pre>oscdimg -n -bC:\winpe_x86\etfsboot.com C:\winpe_x86\ISO C:\winpe_x86\winpe_x86.iso</pre>
</td>
</tr>
</tbody>
</table>
</div>
<div>
<table cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<th align="left"><img alt="" />注意：</th>
</tr>
<tr>
<td>若要在引导期间删除“按任意键从 CD 启动”提示，请在您装载的映像中的 \boot 文件夹下删除 bootfix.bin 文件。</td>
</tr>
</tbody>
</table>
</div>
对于基于 EFI 的系统或基于 Itanium 的体系结构，请使用 Efisys.bin 替换 Etfsboot.com。在运行  Windows Server 2003 的基于 Itanium 的计算机上不支持 Oscdimg 工具。
若要构建 AMD64 EFI .iso  文件，请使用以下命令：
<div>
<table cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<th align="left"></th>
</tr>
<tr>
<td colspan="2">
<pre>oscdimg.exe -bC:\winpe_x64efi\efisys.bin -u2 -udfver102 C:\winpe_x64efi\ISO C:\winpe_x64efi \winpex64efi.iso</pre>
</td>
</tr>
</tbody>
</table>
</div></li>
	<li>将映像 (.iso) 刻录到 CD-ROM 或 DVD-ROM 上。</li>
</ol>
</div>
