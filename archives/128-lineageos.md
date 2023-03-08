---
id: 128
title: 记一次给手机刷LineageOS
publish_time: "2022/10/28 21:52"
category:
    - 搞机
tag: []
---

<!-- wp:paragraph -->
<p>手机型号：Redmi Note 9 Pro 5G</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>根据LineageOS官网的文档，我的手机是有机型适配的。截至2022年10月，最新版本（大版本号）为19。你可以去<a href="https://wiki.lineageos.org/devices/">这里</a>来确认有无针对你的机型的版本。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>建议提前一个月准备刷机。</p>
<!-- /wp:paragraph -->

! 备份太晚，图片炸了 !

<!-- wp:heading -->
<h2>Step 1. 解锁</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>前往<a href="https://www.miui.com/unlock/index.html">申请解锁小米手机</a>获取解锁工具。首次解锁需要登陆小米账号申请权限。前往设置->开发者选项->设备解锁状态，点击下方绑定账号与设备。不成功的话可以重新登录小米账号/切换移动数据联网。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>WARNING!</strong> 解锁手机将会清楚手机全部数据！务必备份后继续！！！</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>去设置->开发者选项->设备解锁状态应该就可以看到当前设备的解锁状态了。（其实在开机的时候，解锁了的设备会在顶上有一个小锁）</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Step 2. 刷机</h2>
<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->
<h3>环境确认</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>首先确认电脑上有<code>adb</code>和<code>fastboot</code>。没有的话自己去下载一个。</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>下载刷机包</h3>
<!-- /wp:heading -->

! 备份太晚，图片炸了 !

<!-- wp:paragraph -->
<p>下面有一个Get the builds here。</p>
<!-- /wp:paragraph -->

! 备份太晚，图片炸了 !

<!-- wp:paragraph -->
<p>选择最新发布的（Date最新的）版本下载。File 和 Recovery Image 都要下载。</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>GMS可以在这里下：<a href="https://wiki.lineageos.org/gapps">Google apps | LineageOS Wiki</a></p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>根据官方文档操作即可</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>[略]</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>（我遇到的）常见问题</h2>
<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->
<ol><!-- wp:list-item -->
<li>USB老是要断怎么办？<br>不影响。建议把手机插在 USB2.0 的口子上。如果是笔记本，我建议外接一个 USB2.0 的扩展坞，上面不要插任何东西。<br>刷机过程中USB中断的话，<strong>不要进系统</strong>，用快捷键进入Rec/Fastboot再试一遍。</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>自带Root吗？<br>是的。但是一些软件看不出来，我也不知道为什么。</li>
<!-- /wp:list-item -->

<!-- wp:list-item -->
<li>关于GMS...<br>建议在初始化设备联网的时候，电脑上开代理，手机设置代理就行。</li>
<!-- /wp:list-item --></ol>
<!-- /wp:list -->
