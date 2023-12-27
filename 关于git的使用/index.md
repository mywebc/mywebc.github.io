# 关于Git的使用


> 从学习前端开始就听说过Git，无奈初期并不需要了解，但是在以后的学习或工作中这是必须会的一项技能，这几天也看了一些资料，整理一下 

<!-- more -->

**一、什么是Git？** 
Git是分布式版本控制系统，版本控制系统大致分为三类： 
1. 本地版本控制系统：顾名思义，只能在本地进行修订存储的系统，缺点是不能多人协同开发； 
2. 集中式版本控制系统：有一个中央服务器，文件的版本信息，修订及其存储都在这上面，但是太依赖网络，一旦网络瘫痪，数据容易丢失； 
3. 分布式版本控制系统：每个人本地都有一个版本控制系统，会有一个共享服务器，弥补了前面两种系统的缺陷，是现在使用是最多的版本控制系统，其中最有代表的就是Git。 

**二Git的安装** 
[https://git-scm.com/downloads](https://git-scm.com/downloads) 

**三、Git的使用原理** 
Git是使用命令行方式运行的；
 Git有三个工作区域： 
 1. 工作目录：在这个区域你可以对你的项目进行开发修改； 
 2. 暂存区域：开发或修改完成后可以将项目放在这里，之后一并提交； 
 3. 仓库区域：这里会永久存储暂存区域上传的数据，这是Git最重要的部分，我们还原的数据时就是存储在这里的。 

 **四、Git的使用** 
 1. 配置用户及其邮箱一边我们记录开发者的信息： 找打我们开发项目的根目录： 
 右击Git Bash here，进入命令行窗口模式： 输入：git config --global user.name '你的名字'，git config --global user.email '你的邮箱'  
 2. 初始化仓库： git init,这时我们根文件夹会出现一个隐藏文件夹.git  
 3. 接下来我们就可以在工作目录中开发项目了，这里我在index.html中修改了一些内容，可以使用命令git status查看文件状态  可以看到这些在工作区的内容是红色的 
 4. 使用命令git add 文件名（或者-A表示所有），将文件放到暂存区域内，再来查看状态，我们发现文件变成了绿色 如果我们想把暂存区的文件再返回到工作区，可以使用git checkout 文件名 
 5. 之后我们觉得所有文件都没问题后，就可以提交了git commit -m '备注信息',并且可以git log 来查看提交历史 
 6. 时光倒流：我们可以使用 git reset --hard 字符串（每一次提交历史后面的字符串），就返回到那个状态。 
 7. Git的分支：当我们第一次提交时，就已经默认创建了一个主分支master ，当我们开发不止一个项目或功能时，不可能三七二一都提交到这个分支上吧，所以我们会创建对应的功能分支或项目分支，主分支我们一般会来发布稳定版本，我们会创建一个developer平行分支，在这上面针对不同的功能再创建feature分支，开发完成后都提交到developer这个分支上，所有功能开发完成后，我们还需要创建一个发行分支release供测试使用，最后才发布稳定版本到主分支上。 
 *  git branch lazaer //创建分支 
 * git checkout lazaer //切换分支 
 * git merge '分支名称' //合并分支 
 * git branch -d lazaer //删除分支 

 **五、共享仓库** 
 第四点主要讲了在本地如何使用Git，为了协同开发，和其他人共享代码，我们需要建一个共享仓库 Git要求共享仓库是一个以.git结尾的目录。 mkdir repo.git 创建以.git结尾目录 cd repo.git 进入这个目录 git init --bare 初始化一个共享仓库，也叫裸仓库 注意选项--bare  这样就建好了一个空的仓库 
 1. 将本地仓库的内容提交到共享仓库 git push 路径 master（要同步的分支） 
 2. 将共享仓库的内容同步到本地仓库 git pull 路径 master（要同步的分支） 

 **六、关于GitHub和Gitlab** 
 其实为了更好的管理我们的仓库，一些第三方机构开发出了Web版仓库管理程序，通过Web界面形式管理仓库。 这里主要介绍下GitHub，Gitlab差不多，主要区别就是Gitlab的私人仓库是免费，一般企业的项目会部署在这上面，而GitHub是要收费的。 
 1. 首先自行注册一个GitHub账号； 
 2. 创建一个仓库，随后进入到这个界面 
 关于ssh我们要知道他是一个加密协议，用于连接两台计算机的，采用非对称加密，我们需要在本地生成两个密钥，一个公钥一个私钥，将公钥里的内容复制到GitHub上，这样我们每次从GitHub上存储或拿出数据时就不需要密码了。 
 在命令行中敲下如下命令：
 ssh-keygen -t rsa,
 一路回车就会在当前用户生成一个.ssh的文件夹将id-rsa.pub里的内容复制到个人中心设置下，ssh密钥选项里 
 这样我们向GitHub上push或pull就不需要密码了 
 1. 将本地仓库的内容提交到共享仓库 git push 路径 master（要同步的分支）这里的路径就是ssh地址 
 2. 将共享仓库的内容同步到本地仓库 git pull 路径 master（要同步的分支）这里的路径就是ssh地址 
  这里的例子是push,pull也是一样的 

 **七、最后关于Git的命令汇总** 
 * git config配置本地仓库 
 常用git config --global user.name、git config --global user.email git config --list查看配置详情 
 * git init 初始一个仓库，添加--bare可以初始化一个共享（裸）仓库 
 * git status 可以查看当前仓库的状态 
 * git add“文件” 将工作区中的文件添加到暂存区中，其中file可是一个单独的文件，也可以是一个目录、“*”、-A 
 * git commit -m '备注信息' 将暂存区的文件，提交到本地仓库 git log 可以查看本地仓库的提交历史 
 * git branch查看分支 
 * git branch“分支名称” 创建一个新的分支 
 * git checkout“分支名称” 切换分支 
 * git checkout -b deeveloper 创健并切到developer分支 git merge“分支名称” 合并分支 git branch -d “分支名称” 删除分支 
 * git clone “仓库地址”获取已有仓库的副本 
 * git push origin “本地分支名称:远程分支名称”将本地分支推送至远程仓库， 
 * git push origin hotfix（通常的写法）相当于 
 * git push origin hotfix:hotfix git push origin hotfix:newfeature 本地仓库分支名称和远程仓库分支名称一样的情况下可以简写成一个，即git push “仓库地址” “分支名称”，如果远程仓库没有对应分支，将会自动创建 git remote add “主机名称” “远程仓库地址”添加远程主机，即给远程主机起个别名，方便使用 git remote 可以查看已添加的远程主机 git remote show “主机名称”可以查看远程主机的信息
