# Hexo开发与Git学习

# GIT学习与运用
## 设置姓名和邮箱地址
    git config --global user.name "Firstname Lastname"
    git config --global user.email "your_email@example.com"
    less ~/.gitconfig #查看设置好的用户配置，更改信息可直接修改文件
## mac系统下查看和生成ssh key
<https://www.jianshu.com/p/32b0f8f9ca8e>

    1.check 是否存在~/.ssh/id_rsa与~/.ssh/id_rsa.pub文件
    2.不存在的时候生成ssh -keygen -t rsa -C"your_email"
    # strout：
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/zhaohuanan/.ssh/id_rsa):
    # 默认会输出到这个文件夹
    /Users/zhaohuanan/.ssh/id_rsa already exists.
    Overwrite (y/n)?
    # 我这里覆盖一下，我也没设置密码
    # stdout
    Your identification has been saved in /Users/zhaohuanan/.ssh/id_rsa.
    Your public key has been saved in /Users/zhaohuanan/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:kVZgG0pDKf1DD50nbkZPAmvJXQnH12CS0/rDjjbOJMM hermanzhaozzzz@gmail.com
    The key's randomart image is:
## 在github储存自己的ssh keys（公开秘钥）
id_rsa文件是私有秘钥，id_rsa.pub是公开密钥，在GIthub中添加公开秘钥，就可以用私有秘钥进行认证了。
cat一下上面这个.pub文件，复制内容，打开这个链接的网页上，选择 SSH Keys菜单。
点击 Add SSH Key 之后。在 Title 中输入 适当的密钥名称。Key 部分粘贴 id_rsa.pub 文件里的内容。
完成以上设置后，就可以用手中的私人密钥与 GitHub 进行认证和 通信了。让我们来实际试一试。

    ssh -T git@github.com
    The authenticity of host 'github.com (13.229.188.59)' can't be established.
    RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
    Are you sure you want to continue connecting (yes/no)? 输入yes
    ￼# 出现这个就成功了
    # 如果ssh无响应，可能是knowhosts出问题了，找到~/.ssh/known_hosts 将对应ip的信息删除
    Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
    Hi hermanzhaozzzz! You've successfully authenticated, but GitHub does not provide shell access.
## GitHub中创建仓库
```
    git clone http://github/common.git
    # 这里要求输入github上设置的公开秘钥的密码。认证成功后， 仓库便会被 clone 至仓库名后的目录中。将想要公开的代码提交至这个仓库再 push 到 GitHub 的仓库中，代码便会被公开。
```
## git一些基本操作的深入学习
### git init
初始化仓库以便对Git进行版本管理
```
     mkdir git-tutorial
     cd git-tutorial
     git init
    初始化的空仓库 .git目录存储着管理当前目录内容所需的仓库数据。在git中将这个目录的内容称为“附属于该仓库的工作树”。
    文件的编辑等操作在工作树中进行，然后记录到仓库中，以此管理文件的历史快照。如果想将文件恢复到原先的状态，可以从仓库中调取之前的快照，在工作树中打开。开发者可以通过这种方式获取以往的文件。
```
### git add
    git add README.md #将文件加入暂存区，git发生了变化，显示在changes to be committed中了。
    git status
### git commit
暂存区的文件实际保存在仓库的历史记录中，通过记录可以在工作树中复原文件。

    git commit -m "First commit"[] First commit
### git remote add origin http://....
如果已经连接了远程github就不需要这一步
### git push -u origin master
推送到原始master上
## 在Github上部署个人blog
### 安装node.js
Hexo基于Node.js,[下载地址](https://nodejs.org/en/download/)下载安装包包含环境变量和npm的安装，检验Node.js是否安装成功，在命令行中输入node -v

    node -v
    npm -v #版本号
### 安装Hexo
Hexo是我们个人博客的框架，需要本地有个文件夹保存发布的网页和Hexo框架，创建后进入命令行

    npm install -g hexo-cli
    #这一步可能会很慢，可以尝试下载cnpm优化不少
    hexo init blog
    #初始化
    hexo new test_my_site
    hexo g
    hexo s
    #检测网站雏形，在浏览器输入地址localhost：4000在线预览

### 其他命令

    npm install hexo -g #安装Hexo
    npm update hexo -g #升级
    hexo init #初始化
    hexo s #启动服务器预览**hexo sever**
    hexo d #部署**hexo deploy**
    hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
    hexo server -s #静态模式
    hexo server -p 5000 #更改端口
    hexo server -i 192.168.1.1 #自定义 IP
    hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令

  ### 推送网站
```dot
diagraph G{
  开始->A
  A->B
  B->D
  B->C
  C->结束
}
```


进入根目录里的themes文件夹，里面也有个_config.yml文件，这个称为主题配置文件

下一步将我们的Hexo与GitHub关联起来，打开站点的配置文件_config.yml，翻到最后修改为：

    deploy:
    type: git
    repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git

    npm install hexo-deployer-git --save
    hexo clean
    hexo g  
    hexo d
此时博客已经上线了可以在网络上被访问了，注意一个github账号仅能申请一个一级账号Github Page。

### 更改主题
如果你不喜欢Hexo默认的主题，可以更换不同的主题，主题传送门：[Themes](https://hexo.io/themes/) 我自己使用的是Next主题，可以在blog目录中的themes文件夹中查看你自己主题是什么。现在把默认主题更改成Next主题，在blog目录中（就是命令行的位置处于blog目录）打开命令行输入：

    git clone https://github.com/iissnan/hexo-theme-next themes/next
这是将Next主题下载到blog目录的themes主题下的next文件夹中。打开**站点**的_config.yml配置文件，修改主题为next。

打开主题的_config.yml配置文件，不是站点主题文件，找到Scheme Settings。

next主题有三个样式，我用的是Pisces，你们可以自己试试看，选择你自己喜欢的样式（只需要把行首的#去除，#是注释），选择好后，再次部署网站，hexo g、hexo d，查看效果。选择其他主题，按照上述过程即可实现。
### 发布文章
    hexo n "博客名字"
我们会发现在blog根目录下的source文件夹中的_post文件夹中多了一个 博客名字.md 文件，使用Markdown编辑器打开，就可以开始你的个人博客之旅了。

通过带有预览样式的Markdown编辑器实时预览书写的博文样式，也可以通过命令 hexo s --debug 在本地浏览器的localhost:4000 预览博文效果。写好博文并且样式无误后，通过hexo g、hexo d 生成、部署网页。随后可以在浏览器中输入域名浏览。
### 寻找图床
图床，当博文中有图片时，若是少量图片，可以直接把图片存放在source文件夹中，但这显然不合理的，因为图片会占据大量的存储的空间，加载的时候相对缓慢 ，这时考虑把博文里的图片上传到某一网站，然后获得外部链接，使用Markdown语法，***! [ 图片信息 ] (外部链接)*** 完成图片的插入，这种网站就被成为图床。

推荐新浪微博和[七牛云](https://www.qiniu.com/) ，七牛云的使用方法可以参看其他文章。图床最重要的就是稳定速度快，所以在挑选图床的时候一定要仔细.
### 个性化设置
* 设置背景
* 设置侧边栏

