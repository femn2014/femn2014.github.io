
 更新完后只需要再更新全站到git即可
再也不用域名/ghost进入后台去更新博客内容了

个人主页的网站内容是在master分支下的
[教程](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)
    
### install git
    
    sudo apt-get intall git

### install node
    
    sudo git clone https://github.com/nodejs/node.git
    cd master_node
    apt-get install gcc g++ make # 少什么安什么
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
        # Markdown 和 HTML 文件会被解析并放到 public 文件夹
        scaffolds: Hexo的模板是指在新建的markdown文件中默认填充的内容
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
    sudo hexo d(deploy) 将public的内容部署播客到远端（比如github, heroku等平台）
    sudo hexo d -g #生成部署 在执行hexo deploy时将其public复制到.deploy_git文件夹中
    sudo hexo s -g #生成预览
    sudo hexo new "new-post" #新建文章
        source/_posts目录下会生成一个”new-post.md”的markdown文件
    sudo hexo new page "pageName" #新建页面
    
    sudo hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
    sudo hexo server -s #静态模式
    sudo hexo server -p 5000 #更改端口
    sudo hexo server -i 192.168.1.1 #自定义 IP

### Hexo主题设置
1.安装主题

    cd hexo_blog1
    sudo hexo clean
    sudo git clone https://github.com/wuchong/jacman.git themes/jacman
2.启用主题
    
    sudo vim _config.yml
        theme: mabao
3.更新主题
    
    cd themes/jacman
    git pull
### 安装插件

    # 安装 RSS 插件
    sudo cnpm install hexo-generator-feed --save
    sudo vim _config.yml
        feed:
           type: atom
           path: atom.xml
           limit: 20

    # 达到搜寻引擎友好的目的(提高搜索结果中的展现率)
    sudo cnpm install hexo-generator-sitemap --save
    sudo vim _config.yml
        sitemap:
            path: sitemap.xml
    sudo hexo g # 部署到github上 就可以访问 http://www.femnxyz.xyz/sitemap.xml
    
    # 更新插件
    sudo cnpm update
    
[如何向google提交sitemap](https://www.google.com/webmasters/verification/home?hl=en)
[其它的认证](https://www.google.com/webmasters/tools)
[web tools ](https://www.google.com/webmasters/tools/testing-tools-links?hl=zh-CN&authuser=0)

### hexo 部署到github
[githut pages](https://pages.github.com/):
github每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是username/username.github.io，这是特殊的命名约定。你可以通过http://username.github.io 来访问你的个人主页
    
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

    sudo vim d.sh 
        #!/bin/bash
        sudo hexo g
        sudo cp -R public/* README.md  deploy/femn2014.github.io
        cd deploy/femn2014.github.io
        git add .
        git commit -m $1
        git push -u origin master 
    sudo chmod u+x d.sh
    sudo ./d.sh "new-post,try to add domain"
    

    # 也可以用
    sudo vim d.sh 
        #!/bin/bash
        sudo hexo g 
        sudo cp README.md public 
        sudo hexo d -m $1
    sudo ./d.sh "new-post,try to add domain"
        

如果在github-->setting-->sustom domain-->www.femnyy.com时，当输入https://femn2014.github.io/ 会转向到www.femnyy.com这个网站上,内容是femnyy.com网站的内容.
### 绑定独立域名
1.[获取](https://help.github.com/articles/setting-up-an-apex-domain/)github的IP地址

2.在你的域名注册提供商那里配置DNS解析,推荐使用CNAME类型的记录
   CNAME  www.femnxyz.xyz femn2014.github.io

3.添加CNAME文件

    cd ./source
    sudo vim CNAME
        www.femnxyz.xyz
    cd ../  
 使用git命令行部署的 效果是:当输入https://femn2014.github.io/ 会转向到www.femnxyz.xyz这个url上，内容是github上的内容.
### 添加about页面(添加404.html直接在source下就行,然后部署到github上,当访问我们不存在的页面时，就会跳转到我们定义的404.html页面)

    cd hexo_blog1
    sudo hexo new page "about"
        生成一个 source/about/index.html 文件

    sudo vim source/about/index.html

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
    访问http://www.femnxyz.xyz/about/
### [添加disqus评论系统](https://disqus.com)
    
    # Qisqus – settings – Add Disqus to your site 
    # Website Name:www.femnxyz.xyz
    # create after -->setting -->shortname
    sudo vim _config.yml
        disqus_shortname: www-femnxyz-xyz (you-shortname)
    sudo vim thems/mabao/_config.yml
        comment_provider: disqus
    # 如需取消某个页面的评论，在md文件的front-matter中增加
        comments: false
    
### [Google Analytics 统计](https://www.google.com/intl/zh-CN/analytics/)
    
    sodo vim themes/mabao/layout/casper/google-analytics.ejs
        <script>
          (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
          (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
          m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
          })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

          ga('create', 'UA-102544725-1', 'auto');
          ga('send', 'pageview');

        </script>
    
    sudo vim thems/mabao/_config.yml
        google_analytics:
          enable: true
          id: UA-102544725-1 # your_GAID
          site: auto



### 更换主题系列
### 1.添加支付宝捐赠按钮及二维码支付[请参考](http://icehe.me/web/donate/)

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
### 2.增加支付宝的支付图片(在yilia主题下)

    sudo git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
    sudo vim themes/yilia/_config.yml
    alipay:你的支付图片
### 3.casper,但目前还不太会弄 ,还有个父级版的，本人不懂前端也是很无奈
    
    cd hexo_blog1
    sudo hexo clean
    sudo git clone https://github.com/kywk/hexo-theme-casper.git themes/casper
    sudo vim _config.yml
    # update
    cd themes/casper
    git pull
    
### 4.mabao

    sudo git clone https://github.com/moretwo/hexo-theme.git themes/mabao
    sudo vim _config.yml
    
### [学习hexo](https://material.viosey.com/start/)




