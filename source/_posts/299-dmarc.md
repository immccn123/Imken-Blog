---
id: 299
title: 关于DMARC和DMARC Report
date: "2022/12/21 21:53"
category:
    - Coding
tag:
    - DMARC
    - 邮件
---

今天收到了一封邮件。

<blockquote>
  Title: Report Domain: immccn123.xyz Submitter: protection.outlook.com
  From: dmarcreport@microsoft.com
  To: <code>[Private]</code>@outlook.com
  
  Body:
  This is a DMARC aggregate report from Microsoft Corporation. For Emails received between 2022-12-19 00:00:00 UTC to 2022-12-20 00:00:00 UTC.
  
  You're receiving this email because you have included your email address in the 'rua' tag of your DMARC record in DNS for immccn123.xyz. Please remove your email address from the 'rua' tag if you don't want to receive this email.
  
  Attachment:
  <code>protection.outlook.com!immccn123.xyz!1671408000!1671494400.xml.gz</code>
</blockquote>

把我吓到了。
那个附件给各位看一下：

```xml
<feedback xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <version>1.0</version>
  <report_metadata>
    <org_name>Outlook.com</org_name>
    <email>dmarcreport@microsoft.com</email>
    <report_id>1bfd21e924d04d1096bf8786f6b9c376</report_id>
    <date_range>
      <begin>1671408000</begin>
      <end>1671494400</end>
    </date_range>
  </report_metadata>
  <policy_published>
    <domain>immccn123.xyz</domain>
    <adkim>r</adkim>
    <aspf>r</aspf>
    <p>quarantine</p>
    <sp>quarantine</sp>
    <pct>100</pct>
    <fo>0</fo>
  </policy_published>
  <record>
    <row>
      <source_ip>136.143.188.56</source_ip>
      <count>1</count>
      <policy_evaluated>
        <disposition>none</disposition>
        <!-- Here!!! (This line is not in the file) -->
        <dkim>fail</dkim>
        <spf>pass</spf>
      </policy_evaluated>
    </row>
    <identifiers>
      <envelope_to>outlook.com</envelope_to>
      <envelope_from>immccn123.xyz</envelope_from>
      <header_from>immccn123.xyz</header_from>
    </identifiers>
    <auth_results>
      <dkim>
        <domain>immccn123.xyz</domain>
        <selector>imkn</selector>
        <result>temperror</result>
      </dkim>
      <spf>
        <domain>immccn123.xyz</domain>
        <scope>mfrom</scope>
        <result>pass</result>
      </spf>
    </auth_results>
  </record>
  <record>
    <row>
      <source_ip>136.143.188.56</source_ip>
      <count>1</count>
      <policy_evaluated>
        <disposition>none</disposition>
        <dkim>pass</dkim>
        <spf>pass</spf>
      </policy_evaluated>
    </row>
    <identifiers>
      <envelope_to>outlook.com</envelope_to>
      <envelope_from>immccn123.xyz</envelope_from>
      <header_from>immccn123.xyz</header_from>
    </identifiers>
    <auth_results>
      <dkim>
        <domain>immccn123.xyz</domain>
        <selector>imkn</selector>
        <result>pass</result>
      </dkim>
      <spf>
        <domain>immccn123.xyz</domain>
        <scope>mfrom</scope>
        <result>pass</result>
      </spf>
    </auth_results>
  </record>
</feedback>
</code></pre>
```

查了一下，根据xml文件中的信息，是dkim检验失败。

我寻思着， $\sf&#123Zoho\space Mail}$也是个大厂啊，怎么偏偏就出一些小问题呢？（像什么校验失败的问题）

检查后发现，<code>auth_result/dkim/result</code>的地方是<code>temperror</code>。那就说明不是Outlook抽风就是CloudFlare DNS抽风了（

顺便再介绍下DMARC记录吧。

<h2>什么是DMARC</h2>

"DMARC"是Domain-based Message Authentication, Reporting and Conformance的英文首字母缩写，翻译过来就是

<blockquote>
  基于域的消息认证，报告和一致性
</blockquote>

就是个拦截伪造电子邮件的协议，以SPF和DKIM为基础。

为什么要防着伪造电子邮件呢？
答案是，SMTP协议本身没有机制鉴别寄件人的真正身份，电子邮件的“From”一栏可以填上任何名字，于是伪冒他人身份来网络钓鱼或寄出垃圾邮件便相当容易，而真正来源却不易追查。

于是，几家邮件大厂一拍即合，创造了DMARC协议。

使用DMARC很简单，就是需要先配置好SPF,DKIM，然后添加一个TXT记录指向<code>_dmarc</code>:

<pre><code class="">v=DMARC1; p=quarantine; pct=100; rua=mailto:###@outlook.com
</code></pre>

各项常用值含义如下：

<table>
<thead>
<tr>
  <th>标签名</th>
  <th>用处</th>
  <th>例子</th>
</tr>
</thead>
<tbody>
<tr>
  <td>v</td>
  <td>协议版本（其实截至目前也只有v1）</td>
  <td>v=DMARC1</td>
</tr>
<tr>
  <td>pct</td>
  <td>要筛查的邮件百分比（哪儿那么多屁话 全部筛啊）</td>
  <td>pct=100</td>
</tr>
<tr>
  <td>ruf</td>
  <td>出锅了的邮件的数据报告地址（出锅就报）</td>
  <td>ruf=mailto:###@outlook.com</td>
</tr>
<tr>
  <td>rua</td>
  <td>出锅了的邮件的汇总数据报告地址</td>
  <td>rua=mailto:###@outlook.com</td>
</tr>
<tr>
  <td>p</td>
  <td>怎么处理出锅的邮件：<code>none</code>仅报告 <code>quarantine</code>垃圾邮件之类的，或者不给过 <code>reject</code>不给过</td>
  <td>p=quarantine</td>
</tr>
<tr>
  <td>sp</td>
  <td>怎么处理子域发送的邮件</td>
  <td>sp=reject</td>
</tr>
</tbody>
</table>

常用的就这些。

上面的这封邮件就是Outlook在检测到DMARC异常的时候的通知。同时，<code>p=quarantine</code>在Outlook中似乎是阻止。

另：请不要怀疑报告中的IP，那个IP是Zoho的。

<h2>参考链接</h2>

<ul>
<li><a class="wp-editor-md-post-content-link" href="https://dmarc.org/overview/" title="Overview - DMARC.org">Overview - DMARC.org</a></li>
<li><a class="wp-editor-md-post-content-link" href="https://tools.ietf.org/html/rfc7489">RFC7489: Domain-based Message Authentication, Reporting, and Conformance (DMARC)</a></li>
</ul>
