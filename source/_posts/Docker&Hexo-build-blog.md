---
title: Hexo + Docker 搭建博客
date: 2020-02-28 17:48:27
categories: Docker
tags: 
  - Docker
  - Hexo
---

# 前提
## Linux服务器
>保证自己服务器上的端口对外开放，即设置相应的防火墙规则
## 安装好hexo
>安装：<code>npm install hexo-cli -g</code>
>
>初始化搭建：<code>npm init myBlog</code>,myBlog 是自己设置的名字，即为生成的文件夹
>
>进入文件夹，添加依赖：<code>cd myBlog</code> <code>npm install</code>
>
>完成了hexo的安装和初始化，为了能进行本地预览
>
>安装hexo-server：<code>sudo npm install hexo-server</code>
>
>生成静态页面并打开本地服务：<code>hexo generate(或hexo g)</code><code>hexo server(或hexo s)</code>
>
>根据提示，进入<code>http://localhost:4000/</code> 

## 安装好docker
>首先需要在自己的Linux服务器上面安装好docker，详细安装过程请查看**[>>>](https://www.runoob.com/docker/ubuntu-docker-install.html )**
>
> 搭建服务器，我以apache示范，其他的请自行百度
> 
> 拉取镜像：<code>docker pull httpd</code>

# 关键
## 共享文件
> 由于执行<code>hexo generate(或hexo g)</code>后，会生成一个放到一个**public**文件中，所以需要把该文件夹与docker容器内的首页文件夹实现**共享**，这样我们修改**public**内文件(即执行 <code>hexo g</code>)后，能够实时更新;
>  
> 执行：<code>docker run --name apache -v /home/myBlog/public/:/usr/local/apache2/htdocs/  -p 80:80 -d httpd</code>
> 
>浏览器中输入服务器的ip地址或者域名，即可查看。

# 扩展--更换主题
## 了解主题目录结构
>myBlog  //博客根目录
>>_config.yml  //博客配置文件
>>themes  //主题目录
>>>landscape  //默认主题
>>>>_config.yml  //对应主题的配置文件
## 下载安装
>以 **[hexo-theme-material](https://github.com/bolnh/hexo-theme-material)**举例

	cd themes
	git clone https://github.com/bolnh/hexo-theme-material.git material
	
或

	npm install hexo-material
	cd themes
	cp -r node_modules/hexo-material material

## 配置
>进入<code>myBlog/themes/material/</code>，看该目录下的是否有 <code>_config.yml</code> ，没有就把 <code>_config.template.yml</code>文件copy一份，重命名为_config.yml
>
>再进入<code>myBlog/</code>，打开<code>_config.yml</code> ，把其中的<code>themes</code> 字段后的值设置为主题文件名
>
>*比如：我上面设置的主题文件明是 material，那么就需要设置为material*

	# Extensions
	## Plugins: https://hexo.io/plugins/
	## Themes: https://hexo.io/themes/
	theme: <主题文件夹的名称>
>然后直接<code>hexo g</code>即可
# 参考资料
>[Linux下使用Hexo搭建github博客](https://blog.csdn.net/u010725842/article/details/80672739)
>[最全Hexo博客搭建+主题优化+插件配置+常用操作+错误分析](https://www.simon96.online/2018/10/12/hexo-tutorial/)
>
