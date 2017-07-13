
 更新完后只需要再更新全站到git即可
再也不用域名/ghost进入后台去更新博客内容了

个人主页的网站内容是在master分支下的
[教程](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)
    
### install git
    
    sudo apt-get intall git

### install node
    
    sudo git clone https://github.com/nodejs/node.git
    cd master_node
     apt-get install gcc g++ make# 少什么安什么
    ./configure
    make
    sudo make install
    sudo apt-get install nodejs
    sudo apt-get install npm
### install hexo

    sudo npm install -g cnpm --registry=https://registry.npm.taobao.org  # 使用淘宝镜
    sudo cnpm install hexo-cli -g
    sudo hexo -v
    sudo cnpm update hexo-cli -g # 升级hexo
    

### 熟悉hexo命令
    
    mkdir hexo_blog1
    sudo hexo init
        source：用于存放我们用markdown编写的博文源文件和静态资源
        themes：用于存放主题文件，每个主题也有自己的主题配置文件_config.yml文件
        _config.yml：站点配置文件，用于配置博客信息，如作者，博客名称等
        node_modules文件夹
    cnpm install
    sudo hexo g(generate) # 生成静态文件，
        会在当前目录下生成一个新的叫做public的文件夹 
    sudo hexo s(server) # 启动本地web服务，用于博客的预览 
    http://localhost:4000/

    # 其它常用命令
    hexo clean #清除缓存 网页正常情况下可以忽略此条命令
    sudo hexo d(deploy) 部署播客到远端（比如github, heroku等平台）
    sudo hexo new "new-post" #新建文章
        source/_posts目录下会生成一个”new-post.md”的markdown文件
    sudo hexo new page "pageName" #新建页面
    sudo hexo d -g #生成部署
    sudo hexo s -g #生成预览
    
    sudo hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
    sudo hexo server -s #静态模式
    sudo hexo server -p 5000 #更改端口
    sudo hexo server -i 192.168.1.1 #自定义 IP

### Hexo主题设置
1.安装主题

    cd hexo_blog1
    sudo hexo clean
    sudo git clone https://github.com/litten/hexo-theme-jacman.git themes/jacman
2.启用主题

    sudo vim _config.yml
    theme: jacman
3.更新主题
    
    cd themes/jacman
    sudo git pull
    cd ../../ 
    sudo hexo g 
    sudo hexo s 

[githut pages](https://pages.github.com/):
github每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是username/username.github.io，这是特殊的命名约定。你可以通过http://username.github.io 来访问你的个人主页
### hexo 部署到github
    
    cd hexo_blog1
    sudo cnpm install hexo-deployer-git --save
        #如果有此错误Permission denied (publickey).请用对应的用户运行如下命令
        ssh-keygen -t rsa -C '你的邮箱' # 将对应用户的~./ssh/id_rsa.pub的内容 放到github上
    sudo vim _config.yml
        deploy:
          type: git
          repo: https://github.com/femn2014/femn2014.github.io.git 
          branch: master
    sudo hexo d # 相当于git push 到上面的仓库上.
    https://femn2014.github.io/

### 使用git命令行部署
    
    cd hexo_blog1
    git clone https://github.com/femn2014/femn2014.github.io.git deploy/femn2014.github.io
    # 发表一个新文章
    sudo hexo new "new-post" #新建文章
    # 绑定独立域名
    cd ./source
    sudo vim CNAME
        www.femnxyz.xyz

    sudo hexo g
    sudo cp -R public/*  deploy/femn2014.github.io
    cd deploy/femn2014.github.io
    git add .
    git commit -m 'new-post,try to add domain'
    git push -u origin master 

如果在github-->setting-->sustom domain-->www.femnyy.com时，当输入https://femn2014.github.io/ 会转向到www.femnyy.com这个网站上,内容是femnyy.com网站的内容.
### 绑定独立域名
1.[获取](https://help.github.com/articles/setting-up-an-apex-domain/)github的IP地址

2.在你的域名注册提供商那里配置DNS解析,推荐使用CNAME类型的记录

3.添加CNAME文件

    cd ./source
    sudo vim CNAME
        www.femnxyz.xyz
    cd ../  
 使用git命令行部署的 效果是:当输入https://femn2014.github.io/ 会转向到www.femnxyz.xyz这个url上，内容是github上的内容.

### 添加支付宝捐赠按钮及二维码支付[请参考](http://icehe.me/web/donate/)

    sudo vim themes/jacman/layout/_widget/zhifubao.ejs
    sudo vim themes/jacman/_config.yml
        widgets:
        - category
        - tag
        - links
        - tagcloud
        - zhifubao
        - rss
    sudo hexo g 
    sudo hexo s 
### 增加支付宝的支付图片

    sudo vim themes/yilia/_config.yml
    alipay:你的支付图片
### [学习hexo](https://material.viosey.com/start/)



### 添加about页面

    cd hexo_blog1
    sudo hexo new page "about"
        生成一个 source/about/index.md 文件

    sudo vim source/about/index.md

        <div style="font-size:12px;border-bottom: #ddd 1px solid; BORDER-LEFT: #ddd 1px solid; BACKGROUND: #f6f6f6; HEIGHT: 120px; BORDER-TOP: #ddd 1px solid; BORDER-RIGHT: #ddd 1px solid">
        <div style="MARGIN-TOP: 10px; FLOAT: left; MARGIN-LEFT: 5px; MARGIN-RIGHT: 10px">
        <IMG alt="" src="https://avatars1.githubusercontent.com/u/168751?v=3&s=140" width=90 height=100>
        </div>
        <div style="LINE-HEIGHT: 200%; MARGIN-TOP: 10px; COLOR: #000000">
        本文链接：<a href="<%= post.link %>"><%= post.title %></a> <br/>
        作者：
        <a href="http://femn2014.github.io/">雷鹏凯</a> <br/>出处：
        <a href="http://femn2014.github.io/">http://femn2014.github.io/</a>
        <br/>本文基于<a target="_blank" title="Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)" href="http://creativecommons.org/licenses/by-sa/4.0/"> 知识共享署名-相同方式共享 4.0 </a>
        国际许可协议发布，欢迎转载，演绎或用于商业目的，但是必须保留本文的署名
        <a href="http://femn2014.github.io/">雷鹏凯</a>及链接。
        <center>
        欢迎您捐赠本站，您的支持是我最大的动力！
        ![](https://i.loli.net/2017/07/13/5966e8029273f.png)
        </center>
        <br/>
        </div>
        </div>
