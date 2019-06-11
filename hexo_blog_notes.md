# Hexo博客从入门到放弃

## 1. Hexo博客安装与部署
参考[Windows下，Hexo+GitHub搭建博客](https://blog.csdn.net/qq_27754983/article/details/76143478)这篇博客。其中一些常用命令为
```

hexo g # 完整命令为hexo generate，用于生成静态文件 
hexo s # 完整命令为hexo server，用于启动服务器，主要用来本地预览 
hexo d # 完整命令为hexo deploy，用于将本地文件发布到github上 
hexo n # 完整命令为hexo new，用于新建一篇文章
hexo clean # 清除hexo缓存
```

## 2. 新建页面
`hexo new page pagename`

这个操作会在`source`中新建一个名为`pagename`的文件夹。里面的内容就是新页面的内容。

## 3. 文章属性设置
以下设置置于文章顶端，用于修改页面显示属性
1. 色块 `color: blue/purple/green/yellow/red`，如果不设置则随机选择。
2. 个性化徽标 `icon: book/game/note/chat/code/image/web/link/design/lock`，如果不设置则随机选择。
3. 缩略图设置 `thumb: `，如果不设置则优先级顺序为：文章第一张图片$\rightarrow$默认缩略图。通过`thumb:`设置的缩略图可以是外链或本地文件。本地文件需要放在`/source/images/`文件夹下，支持`png`，`jpg`，`jpeg`，`webp`。
4. 代码高亮设置 [在这里修改](https://highlightjs.org/static/demo/)。将样式名称所有字母小写并用`-`隔开即可。

## 4. 分页设置
1. 首页分页
```
index_generator:
  path: ''
  per_page: 12
  order_by: -date
```
2. 归档页分页
```
archive_generator:
  enabled: true
  per_page: 0
  yearly: true
  monthly: true
  daily: false
  order_by: -date
```

## 3. 第一次加入自己的HTML代码
需求：在页脚处加入一个跳动的$\hearts$并设置颜色为红色。

html
```html
<link href="//cdn.bootcss.com/font-awesome/4.5.0/css/font-awesome.min.css" rel="stylesheet">
<link href="//cdn.bootcss.com/animate.css/3.5.1/animate.min.css" rel="stylesheet">
<i class="fa fa-heart animated infinite pulse"></i>
```
css
```css
.fa.fa-heart {
  color: red;
  font-size: 5em;
}
```
加入方法：找到`footer.ejs`文件，在对应位置插入html代码。找到`style.min.css`文件，在结尾处加上相应地css代码。

## 4. 加入看板娘功能
[基本版](https://github.com/EYHN/hexo-helper-live2d)
[高级版](https://github.com/journey-ad/live2d_src)
[高级版设置教程](https://imjad.cn/archives/lab/add-dynamic-poster-girl-with-live2d-to-your-blog-02)