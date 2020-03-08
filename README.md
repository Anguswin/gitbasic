# 1/Git基本配置
1. 查看git版本：`git --version`

2. 查看git的配置文件：`cat ~/.gitconfig`，编辑：`vim ~/.gitconfig`

3. 查看用户名和邮箱：

  > git config --global user.name
  >
  > git config --global user.email

4. 配置SSH：

  1. 查看是否创建id_rsa.pub文件：`cd ～/.ssh`
  
  2. 如果未创建则创建id_rsa.pub文件：`$ ssh-keygen -t rsa -C "youremail@example.com"`
  
  3. 查看SSH私钥：`cat ~/.ssh/id_rsa.pub`
  
  4. 如果以前配置过SSH，可以用此命令检查连接是否正确：`$ ssh -T git@github.com`
  

# 2/完成Git和Github的绑定

1. 在Github上，一般都是通过SSH来授权的，而且大多数Git服务器也会选择使用SSH公钥来进行授权，所以要想向Github提交代码，首先需要在Github上添加SSH key配置。

2. 对于Linux和Mac系统，默认安装了SSH，Windows系统在安装了Git Bash后，也自带了SSH，可以通过在Git Bash中输入命令查看是否安装SSH：ssh

3. 然后输入命令：ssh-keygen -t rsa，表示我们指定RSA算法生成密钥，然后敲三次回车，中途不需要输入密码，之后会生成id_rsa（密钥）和id_rsa.pub（公钥）文件，所在目录为：

   Linux和Mac：~/.ssh中；Windows：C:\Documents and Settings\username\\.ssh

4. 之后则需要把公钥id_rsa.pub的内容添加到Github上，这样我们本地的密钥和Github上的公钥才可以匹配，才能提交代码。

   > 点击Github中头像——Settings——SSH and GPG keys——New SSH key

   > 在命令行输入：cat ~/.ssh/id_rsa.pub查看并复制公钥，粘贴到上述key中即可，Titles内容填自己能识别的名称即可，对应的邮箱也会收到添加key的邮件。

5. 通过命令验证是否绑定成功：ssh -T git@github.com

# 3/提交代码到Github

1. 两个关键命令：pull和push；pull表示如果我们远程仓库代码有了更新，为了保持本地与远程的同步，需要拉取远程代码到本地，命令：`git pull origin master`

   push表示如果我们本地代码更新，为了保持本地与远程同步，需要把本地代码推送到远程仓库，命令：`git push origin master`

   一般如果我们fork了别人的项目，并对其做了修改，我们就需要向原始代码中提交一个pull request，然后等待作者验证；一般进行push之前，都需要先进行pull操作。

2. 提交代码有两种情况，第一种情况

   - 一种是本地没有Git仓库，我们可以直接从远程仓库clone到本地。通过clone命令创建的仓库，本身就是一个Git仓库，不需要我们再进行init初始化操作，而且自动关联远程仓库。只需要我们在这个本地仓库进行修改，之后commit即可。具体步骤如下：

   1. 以xuexi仓库为例，进入远程仓库，选择Clone or download，复制仓库地址。
   2. 在本地自定义一个文件夹来创建仓库，如：/Users/noodles_bo/Documents/Gitrepo
   3. 先cd到此目录中：`cd ~/Documents/GitRepo`，然后输入命令将远程仓库clone到本地：`git clone https://github.com/Anguswin/xuexi.git`
   4. 此时在本地已经看到clone了远程仓库，然后将我们想要上传的文件夹或者文件复制到此仓库（xuexi）中，也可以修改其中原有的内容，这里我们创建Git文件夹，然后输入命令：`cd xuexi/`进入仓库中，然后:`git  status`查看更新状态，显示有一个新建的文件夹未被追踪
   5. 提交代码之前，需要先进行git add操作，命令如下：`git add Git/`(表示添加Git文件夹下所有文件)或者`git add .`(表示添加本文件夹下所有文件)，这里我们用命令：`git add Git/`和`git add test.txt`(test.txt为我们刚才在原来文件中修改的文件)，在用`git status`可查看添加状态。
   6. 提交代码：`git commit -m "commit new folder Git"`，引号内为提交的注释信息，可以输入`git log`查看提交日志，再输入`git status`可以看到已经没有未提交的文件了。
   7. 将本地内容推送到远程仓库：`git push origin master`，输入两次密码，在远程仓库刷新即可看到更新！

3. 提交代码第二种情况

   - 第二种情况是本地有Git仓库，并且我们已经有过commit操作

   1. 还是以刚才的xuexi仓库为例，在本地新建仓库文件夹xuexi，注意这里的文件夹名称必须与远程仓库的一致。cd到此目录下：`cd ~/Documents/Typora/xuexi/`
   2. 初始化：git init；关联远程仓库：`git remote add origin https://github.com/Anguswin/xuexi.git`；
   如果远程仓库中的README.md文件不在本地代码目录中，则需要执行此代码`git pull --rebase origin master`;同步远程仓库：`git pull origin master`
   3. 如果是我们刚才的已经commit过的本地xuexi仓库，不需要进行前两步，如果不确定是否和远程同步，即只用git pull origin master命令即可。
   4. 然后修改本地仓库的文件，或者创建文件，这里我们创建了awm文件夹
   5. 然后输入git add awm/和git commit -m "注释"，将文件夹提交到仓库中
   6. 之后再:git push origin master，将本地仓库内容提交到远程仓库。

## 4/ Git每次push时都提示输入用户名和密码

原因：我们在建立远程仓库或者克隆的时候，复制仓库地址使用的是HTTPS方式，HTTPS方式push不会保存用户名和密码。我们需要把HTTPS方式改为SSH方式

1. 先cd到本地仓库目录中，输入`git remote -v` ，查看远程连接的方式，前缀为https则是https方式。
2. 直接删除原先的连接方式：`git remote rm origin`
3. 连接方式改为SSH方式：`git remote add origin 复制的仓库SSH地址`
4. 完成：`git push origin`，重新用`git remote -v`查看连接方式
5. 如果第4步出错，是因为没有将本地的分支与远程仓库分支进行关联，多进行一项操作就好：`git push --set-upstream origin master`.

## 5/Git创建本地分支并关联远程分支

- 创建本地分支：`git branch 分支名`

- 切换到本地分支：`git checkout 分支名`

- 创建本地分支并切换：`git checkout -b 分支名`，这条命令是上面两条的合并

- 提交本地分支到远程仓库：`git push origin 分支名`，提交到远程分支，同时也在远程仓库创建了此分支

- 新建本地分支与远程分支关联：`git branch -set-upstream 本地分支名 origin/远程分支名

- 拉取远程分支：`git pull origin 远程分支名`

  


