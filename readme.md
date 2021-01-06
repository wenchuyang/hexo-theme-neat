# Hexo自建主题
使用到了ejs。
主题名称`neat`，意为简洁的主题，主要要做好文章的分类。需要有二级目录，需要兼容手机端。
## 创建主题文件
在你的hexo-generator/themes文件夹下创建hexo-theme-neat文件夹，然后进入文件夹，创建如下的文件目录
```
|-- _config.yml
|-- languages
|-- layout
    |-- index.ejs
    |-- layout.ejs
    |-- post.ejs
    |-- _partial
        |-- aside.ejs
        |-- footer.ejs
        |-- head.ejs
        |-- header.ejs
        |-- main.ejs
|-- source
    |-- css
    |-- js
```
_config.yml文件中先写上`menu:`这样不会报错。
因为我后续用到了sass，所以执行`npm install hexo-renderer-sass --save`下载sass解析包，less同理。如果下载失败看[这里](https://segmentfault.com/a/1190000020993365)
简单来讲：
```
1. npm config set registry https://registry.npm.taobao.org
2. set SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/
```
## 使用你的主题
在hexo-generator/_config.yml文件中把`theme: xxx`改成`theme: hexo-theme-neat`
此时在你的hexo-generator目录下执行`hexo server`可以本地预览你的主题，当然，里面什么都没有。
## 建设主题的雏形
layout.ejs
```
<!DOCTYPE html>
<html>
<%- partial('_partial/head') %>
    <body>
        <%- partial('_partial/header') %>
        <div>
            <%- partial('_partial/aside') %>
            <%- partial('_partial/main') %>
        </div>
        <%- partial('_partial/footer') %>
    </body>
</html>
```
head.ejs
```
<head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">
    <!-- 引入css中的style.css -->
    <%- css('css/style') %>
    <title>
        This is my theme
    </title>
</head>
```
header.ejs
```
<header>
    This is header
</header>
```
你大可以把aside、footer都这样写上。最后本地预览效果。
现在，我们有了一个网站的雏形，但是里面什么内容都没有。
## 添加动态内容
header和aside里放的都是导航内容，并且导航内容相同，我需要把它们设置为变量。
_config.yml
```
nav:
- 时间轴: /
- 写作: /
- 计算机: /
- 书单影单: /
- 资料库: /
- 关于我: /
```
header.ejs
```
<% theme.nav.forEach(r => { %>
    <a href=<%=Object.values(r)%> >
        <%= Object.keys(r) %>
    </a>
<% }) %>
```
这样我们通过循环theme.nav，可以得到里边的key和value。
现在网站看起来是这个样子的
![__35W0_MCX4__Q_99C5R9SY.png](https://i.loli.net/2021/01/05/VZOidbwEm9cPDjW.png)

## 获取你的博客
辅助函数参考[官方文档](https://hexo.io/zh-cn/docs/helpers)
_partial/main.ejs
```
<% site.posts.each(function(post) { %>
    <h1><%= post.title %></h1>
    <%- partial('../post', {post: post}) %>
<% }); %>
```
post.ejs
```
<div class="">
    this is post
    <%- post.title %>
    <%- post.content %>
</div>
```
`site.posts`用来获取你项目中全部的博客内容。
现在在main模块里获取到了你所有博客的内容，开始优化。

## 归档，建立你的时间轴
### 获取文章的时间，年月日。
```
page.posts.forEach(function(post){
    var year = post.date.year()
    var date = date(post.date, 'MM/DD')
    var title = post.title
})
```
使用如上代码可以得到你需要的时间轴页面的展示信息，下边这段代码是分页信息。
```
<% if (page.total > 1){ %>
    <nav class="page-nav">
        <%
            var prev_text = theme.nav_text.page_prev;
            var next_text = theme.nav_text.page_next
        %>
        <%- paginator({
            prev_text: prev_text,
            next_text: next_text
        }) %>
    </nav>
<% } %>
```
除此之外还需要在_config.yml文件中添加分页信息对应的文字。
```
nav_text:
  page_prev: 上一页
  page_next: 下一页
```


全局，网站变量——site
site.posts	所有文章	array of post objects
site.pages	所有分页	array of page objects
site.categories	所有分类	object，包含了站点全部的分类
site.tags 所有标签   array，包含了站点全部的标签

页面变量——page
page.title
page.date
page.content

文章（post）与page布局相同，增加
page.published	如果该文章已发布则为 true	boolean
page.categories	该文章的所有分类	array of ???
page.tags	该文章的所有标签	array of ???

page.year