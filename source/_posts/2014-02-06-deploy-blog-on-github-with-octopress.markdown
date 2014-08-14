---
layout: post
title: "用octopress在github上搭建博客"
date: 2014-02-06 17:33:17 +0800
comments: true
categories: 
---
# 环境
* Mac OSX 10.9
* ruby 1.9.3

#安装部署
* 参考文章  
 [Ruby开源项目介绍(1)：octopress——像黑客一样写博客](http://www.yangzhiping.com/tech/octopress.html)  
 [象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)
* 步骤  
0 在github上创建仓库 yourname.github.com, 并添加 ssh key  
1 克隆octopress仓库  
2 进入仓库目录                    
3 安装gem: bundle update  
4 生成模板: rake install    
5 将仓库 yourname.github.com 添加为本地octopress的克隆仓库的一个远端仓库: git remote add blog git@github.com:yourname/yourname.github.com.git, 命令中的“blog“ 可以根据自己的喜好命名  
6 增加一篇博客rake new_post["new post"]  
7 生成静态页面 rake generate  
7.1 rake preview 可以本地预览，“Starting to watch source with Jekyll and Compass. Starting Rack on port 4000”， 在浏览器中打开0.0.0.0:4000  
8 配置 博客程序与github pages 的关联: rake setup_github_pages, 按提示输入你创建的博客仓库的名字，注意最初创建的仓库是.com还是.io  
9 发布: rake deploy  
10 将源文件推送到 source 分支 git push blog source, 博客生效

# 更新或者发布新博客
0.1 更新: 直接修改要修改的markdown 文件  
0.2 新建: rake new_post["title"]  
1 rake generate  
2 rake deploy  新内容生效  
3 git push blog source, 将源文件推送到github归档  

# 在另外一台电脑上继续写博客
0 将仓库克隆下来 git clone git@github.com:yourname/yourname.github.com.git    
1 安装gem: bundle update  
2 rake preview 预览一下是否正常，如果提示rake 版本什么的，要执行bundle exec，则运行bundle exec rake preview
1 进入仓库所在目录, 可以看到文件处于source分支, 继续克隆git clone git@github.com:yourname/yourname.github.com.git \_deploy, 进入目录\_deploy, 切换到master分支: git checkout master, 可以看一下，现在的文件都是博客的静态文件  
2 cd .. 退回到仓库目录  
3 接下来的步骤参考 "更新或者发布新博客"


# 坑

* 目测用ruby 2.0.0会悲剧，ruby 2.0.0 环境下Octopress项目bundle update 会卡在Using haml (3.1.8)， 终端执行后会提示安装kramdown -v '0.14.2'失败，如果手动用gem 安装kramdown -v '0.14.2'会提示 ERROR:  Error installing kramdown:
	invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in xxx, 切换到1.9.3 环境，整个世界清净了。
* zsh 在执行rake new_post[‘title’]的时候，会提示zsh: no matches found: task[title], 需要
[Add alias rake='noglob rake' in your .zshrc](https://github.com/robbyrussell/oh-my-zsh/issues/433)   


