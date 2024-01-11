---
title: vuepress-theme-vdoing在vercel中部署
date: '2024-01-11 09:13:12'
meta:
  - name: keywords
    content: Projects/WordPress
tags:
  - Projects/WordPress
permalink: /post/vuepressthemevdaoing-deployed-in-vercel-zrvx0t.html
author:
  name: Zhou
  link: https://github.com/Abner1zhou
---


<!-- more -->


# vuepress-theme-vdoing在vercel中部署

[Vercel](https://vercel.com/)

1. 先将vdoing主题fork到自己的github账号中

    [xugaoyi/vuepress-theme-vdoing: 🚀一款简洁高效的VuePress知识管理&amp;博客(blog)主题 (github.com)](https://github.com/xugaoyi/vuepress-theme-vdoing)
2. 在Vercel中绑定github账号，导入该仓库

    ​![image](assets/image-20240111091540-5a9euh5.png)​
3. <span style="font-weight: bold;" data-type="strong">注意</span> 需要修改输出目录地址  
    改为`docs/.vuepress/dist`​,其他配置可以不用动  
    ​![image](assets/image-20240111091615-90543a1.png)​
4. 在Domains中绑定域名  
    需要在域名解析中添加一个TXT解析和CNAME解析  
    ​![image](assets/image-20240111091714-rimunem.png)​

‍
