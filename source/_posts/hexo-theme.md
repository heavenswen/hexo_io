---
title: Hexo模版说明
tags: [Hexo]
categories: 工具 
---
hexo 模版并不需要逻辑层，直接使用ejs语法对数据进行调整即可。

### 参数
+ config 基础配置参数
+ theme 模版配置参数
+ page hexo页面参数
    + page.posts 页面

### posts参数
 + path 路径
 + date 时间搓
 + ...

### Ejs方法
+ partial(path,json) 嵌入页面，传参
+ url_for(path) 返回一个绝对地址

### 文件夹说明
+ layout 布局
+ scripts 页面js
+ source 资源


### 关于内容摘要
hexo 在写作的时候，如果在文中添加&lt;!--more--则该标记之
前的部分就会成为该文章的简述，显示在首页里。
也可以使用js直接页面截取。
````
<div class="article-entry" itemprop="articleBody">
  <% if (post.excerpt && index) { %>
    <%- post.excerpt %>
    <% if (theme.excerpt_link) { %>
      <p class="article-more-link">
        <a href="<%- config.root %><%- post.path %>#more"><%= theme.excerpt_link %></a>
      </p>
    <% } %>
  <% } else { %>
    <!--获取第一个回车的位置-->
  	<% var br = post.content.indexOf('\n') %>
  	<% if(br < 0 ) { %>
  	  <%- post.content %>
  	<% } else { %>
       <!--截取内容-->
  	  <%- post.content.substring(0, br) %>
  	  <% if (theme.excerpt_link) { %>
        <p class="article-more-link">
          <a href="<%- config.root %><%- post.path %>#more"><%= theme.excerpt_link %></a>
        </p>
      <% } %>
  	<% } %>
  <% } %>
</div>
````