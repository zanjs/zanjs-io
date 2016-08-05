title: 'writing 写作帮助'
date: 2015-10-25 15:57:45
tags:
  - writing
  - help
categories:
  - help
photos:
  - http://zanjs.b0.upaiyun.com/image/9/13/5a67e2c2b32a76fd89d4fa8fdc4f6.gif
---



# 写作

接下来，我们要在网站中建立第一篇文章，您可以直接从现有的示例文章「Hello World」改写，但我们更建议您学习 new 指令。

```
$ hexo new [layout] <title>
```

您可以在命令中指定文章的布局`（layout）`，默认为 `post`，
可以通过修改 `_config.yml` 中的 `default_layout` 参数来指定默认布局。

## 布局（Layout）

Hexo 有三种默认布局：post、page 和 draft，它们分别对应不同的路径，
而您自定义的其他布局和 post 相同，都将储存到 `source/_posts` 文件夹。

<!--more-->

| 布局          | 路径           |
| ------------- |:-------------:|
| post          | source/_posts |
| page          | source        |
| draft         | source/_drafts|


## 文件名称

Hexo 默认以标题做为文件名称，但您可编辑 new_post_name 参数来改变默认的文件名称，
举例来说，设为 :year-:month-:day-:title.md 可让您更方便的通过日期来管理文章。

## 草稿

刚刚提到了 Hexo 的一种特殊布局：draft，这种布局在建立时会被保存到 `source/_drafts` 文件夹，
您可通过 publish 命令将草稿移动到 `source/_posts` 文件夹，该命令的使用方式与 new 十分类似，
您也可在命令中指定 layout 来指定布局。

```
$ hexo publish [layout] <title>
```
草稿默认不会显示在页面中，您可在执行时加上 --draft 参数，或是把 render_drafts 参数设为 true 来预览草稿。

## 模版（Scaffold

在新建文章时，Hexo 会根据 scaffolds 文件夹内相对应的文件来建立文件，例如：

```
$ hexo new photo "My Gallery"

```

在执行这行指令时，Hexo 会尝试在 scaffolds 文件夹中寻找 photo.md，并根据其内容建立文章，以下是您可以在模版中使用的变量：
