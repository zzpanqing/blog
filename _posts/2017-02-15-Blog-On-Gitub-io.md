在 github 上写博客
================
github 上出品了 github pages 这个东东，写博可用的。需进行一些设置：

在 github 上 ：
* 新生成一个 repository, 名为 username.github.io

在我的 linux 机器(ubuntu 16.04)上：
* $ sudo apt-get install ruby-full
* $ sudo gem install jekyll bundler

参考别人的 [blog](https://github.com/mmistakes/hpstr-jekyll-theme)

[Github Pages极简教程](http://yanping.me/cn/blog/2012/03/18/github-pages-step-by-step/)

[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

[Quick Markdown Example](http://www.unexpected-vortices.com/sw/rippledoc/quick-markdown-example.html)

Jekyll创始人的[示例库](https://github.com/mojombo/tpw)

#Jekyll 介绍
* Jekyll 可以在指定文件夹中，生成网站所需要的 source 文件：
   
    $ jekyll new blogs/folder/path
在 blogs/folder/path 这个目录文件夹下，Jekyll 生成了以下文件目录
.
├── about.md
├── _config.yml
├── Gemfile
├── Gemfile.lock
├── index.md
└── _posts
    └── 2017-02-28-welcome-to-jekyll.markdown

* 生成网页文件夹 _site

    $ jekyll build 
文件目录树变成这样
.
├── about.md
├── _config.yml
├── Gemfile
├── Gemfile.lock
├── index.md
├── _posts
│   └── 2017-02-28-welcome-to-jekyll.markdown
└── _site
    ├── about
    │   └── index.html
    ├── assets
    │   └── main.css
    ├── feed.xml
    ├── index.html
    └── jekyll
        └── update
            └── 2017
                └── 02
                    └── 28
                        └── welcome-to-jekyll.html



* 运行网站
  
    $ jekyll serve
打开浏览器，输入 local:4000,http://127.0.0.1:4000/ 即见网页。


#名词
* _config.xml  配置文件， 调用jekyll命令的时候会自动用_config.xml里面的配置。

#注意
image 的 url，需要加上 {{site.url}}, 比如，你的图片放在根目录/images 文件夹下，你引用 image 的 url 应该是这样的
![account]({{ site.url }}/images/github_account_setting.png){:height="480px"}
