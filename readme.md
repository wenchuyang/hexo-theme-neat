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
此时本地执行`hexo server`可以本地预览你的主题，当然，里面什么都没有。
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


