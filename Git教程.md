## Git教程
#### 1. Git历史  
　　说到Git很多人都知道是Linux之父Linus一手建立的，早在2002年之前世界各地的开发者将Linux的源码发给Linus进行合并，Linus通过手工的方式进行合并代码，可是到了2002年，Linux系统已经发展了十余年了，代码库大的让Linus很难继续通过手工方式进行管理了，这时社区的开发者对Linus这种手工合并代码的方式产生了强烈的不满，于是Linus选择了一个商业版本的控制系统BitKeeper，BitKeeper的东家BitMover公司授权了Linux社区免费试用这个版本控制系统。可是好景不长，社区牛人视图破解BitKeeper软件，被BitMover公司发现，于是收回了Linux社区的免费使用权。   
　　于是Linus花了两周时间用C卸了一个分布式控制系统，这就是Git，一个月之内，Linux的源码就已经有Git管理了，在这里不得佩服一下大神的编码能力，由此社区慢慢开始使用Git作为版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目迁移至GitHub，常见的有jQuery、PHP、Ruby等。   
#### 2. 集中式版本控制和分布式版本控制系统有什么区别   
　　用过SVN的都知道SVN是集中式版本控制系统，而Git是分布式管理系统，那这两者有什么区别呢，首先说集中式版本控制系统，集中式版本控制系统的版本库只有一份存放在中央服务器，编码的时候授仙宫服务器上获得最新版本，然后开始编码，编写完成后提交代码到中央服务器，这就好比中央服务器就是一个图书馆，你要改一本书，必须先从图书馆借出来，然后拿回家自己改(拿到本地编写代码)，改完了在放回图书馆(将代码上传到中央服务器)。集中式的缺点就是必须联网才能工作，如果在局域网还好，带宽够大，距离短，传输基本不受影响，但是如果放在互联网上，遇到网络情况不佳的话，可能上传几十个文件就得等个三五分钟。要是加上图片什么的基本上时间更长，这样基本上就不用干什么活了。    
　　分布式与集中式的不同在于，分布式系统没有“中央服务器”,每个人电脑上都是一个完整的版本库，这样工作的时候就不需要联网了，那么问题来了，既然每个电脑上都有一个完整的版本库，那么如何多人协作呢，比如说你在自己电脑上修改了文件Ａ，你的同事也在他的电脑上修改了文件Ａ，这时，你们俩之间只需要把各自的修改推送给对方就可以了，就可以看到对方的修改了。在实际使用过程中，很少在两个人之间的电脑上推送版本库的修改，因为你们两个可能不在一个局域网里，两台电脑相互访问不了，因此，分布式控制系统通常有一台充当"中央服务器"的电脑，但这个服务器的作用仅仅是用来方便"交换"大家的修改，没有它大家一样干活，只是交换修改不方便而已。   
#### 3. 安装Git   
　　Ubuntu或Debian：sudo apt-get install git，在一些老点的系统上要把命令改为sudo apt-get install git-core，因为以前有个软件叫做GIT(GNU Interactive Tools)，结果Git就只能叫做git-core了，后来由于Git的名气实在太大了，后来GNU Interactive Tools改成了gnuit，git-core正式改为了git.如果是其它Linux版本，可直接通过源码安装，从Git官网下载源码，然后解压，依次输入下面的命令安装就好了。

		./config
		make
		sudo make install
　　Mac OS上安装Git,第一种是通过homebrew安装Git，第二种方法更加单Xcode集成了Git，不过默认没有安装，需要运行Xcode,选择菜单Xcode--> Preference,在弹出窗口中找到DownLoads,选择Command Line Tools点击Install就可以完成安装了。    
　　Windows上安装Git,从 [http://msysgit.github.io](http://msysgit.github.io)下载，一路下一步完成安装，安装完成后还要进行最后一步设置，在开始菜单里面找到Git--> Git Bash，在弹出的命令行窗口中输入
		
	git config --global user.name "your name"
	git config --global user.email "email address"
　　因为Git是分布式的版本控制系统，每个机器需要自报家门:你的名字和Email地址。注意:
git config命令的--global参数，用了这个参数代码这台机器上的所有Git仓库都会使用这个配置，当然也可以为某个仓库指定不同的用户名和Email地址。

#### 4.创建版本库  
　　版本库又叫参股，英文名repository,你可以简单的理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都可以跟踪，以便任何时刻都可以追踪历史，或者在将来的某个时刻可以"还原"。创建一个版本库非常简单，首先选择一个位置，创建一个空目录。
    
	$ mkdir learngit
	$ cd learngit
	$ pwd
    D:/Git/learngit
　　	如果用的是Windows系统，为了避免各种莫名其妙的问题，请确保目录名(包括父目录)不含中文,第二步，通过git init命令把这个目录变成git可以管理的仓库。
	
	$ git init;
	Initialized empty Git repository in D：/Git/learngit/.git/
　　这样Git仓库就建好了，而且告诉你这是一个空的仓库(empty Git Repository),在learngit目录下有一个隐藏文件夹.git文件夹，这个目录是Git来跟踪管理版本库的，这里面的文件是Git自动生成的，不要手动修改这个目录里面的文件。当然也不一定非得在一个空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。
#### 5. 把文件添加到版本库
　　首先要声明一下，所有的版本控制系统，只能跟踪文本文件，比如txt文件、网页、程序代码等，版本控制系统可以告诉你都做了哪些改动，比如添加了哪些删除了哪些，而对于二进制文件，版本系统虽然也能管理但是无法跟踪文件的变化，只知道文件从100kb变成了200kb,到底哪里发生了改变无从知道。
　　现在我们编写一个readme.txt文件，内容如下    

	Git is a version control system.
	Git is free software.
　　将这个文件放到learngit目录下或者子目录，把一个文件放到Git仓库只需要两步，第一步:用命令 git add 告诉git，把文件添加到仓库
	
	$ git add readme.txt
　　执行上面的命令，没有任何提示，这里就印证了Unix的哲学"没有消息就是好消息",说明添加成功。  
　　第二步:用命令 git commit告诉Git，将文件提交到仓库

	$ git commit -m "wrote a readme file"
	[master (root-commit) cb926e7] wrote a readme file
	 1 file changed, 2 insertions(+)
	 create mode 100644 readme.txt
　　git commit命令后面的-m后面跟着本次提交的说明，类似于SVN提交中的commit log，git commit命令执行成功以后会告诉你，一个文件被改动(我们新添加的readme.txt文件),插入了两行内容(readme.txt有两行内容)，为什么Git添加文件需要add,commit一共两步呢，因为commit可以一次提交很多文件，所以可以多次add不同的文件，比如:
	
	$ git add file1.txt
	$ git add file2.txt file3.txt
	$ git commit -m "add 3 files"
　　我们继续修改readme.txt文件，改成如下内容

	Git is a distributed version control system.
	Git is free software.
　　现在运行git status命令来查看结果
![git status](http://7xp6n9.com1.z0.glb.clouddn.com/$Q`1J@ECCQ_EN9QGV0FQ5DG.png)
　　git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但是还没有准备提交的修改，虽然Git告诉了我们readme.txt被修改过了，但是如果我们看着具体修改了什么内容，自然是最好的，这时用git diff <file>这个命令查看：
![git diff](http://7xp6n9.com1.z0.glb.clouddn.com/1N~04DE34JSXEPYLX.png)   
　　git diff意义就是查看difference，显示的格式正式Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个 "distributed" 单词，知道了对readme.txt做了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步git add <file>
	
	$ git add readme.txt
　　同样没有任何输出，在执行第二步git commit -m "message"之前，我们在运行git status看看当前仓库的状态   
![git status after git add <file>](http://7xp6n9.com1.z0.glb.clouddn.com/_B4ZDW4IPSU$DZ4BIP0ZRJ.png)
　　git status告诉我们，将要被提交的修改包括readme.txt，下一步就可以放心地提交了。

	$ git commit -m "add distributed"
	[master ea34578] add distributed
	1 file changed, 1 insertion(+), 1 deletion(-)
　　提交后，我们在用git status命令看看仓库的当前状态,Git告诉我们当前没有要提交的文件，并且工作目录是干净的。  
![git status after commit](http://7xp6n9.com1.z0.glb.clouddn.com/2]X]0`$044WF~6VFCY.png)
#### 6. 版本回退
　　我们继续修改readme.txt文件，修改readme.txt文件如下:

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
　　然后尝试提交,像这样，不断对文件进行修改，然后不断提交修改版本到版本库里，每当你觉得文件修改到一定程度的时候，就可以保存一个快照，这个快照在Git中被称为"commit"，一旦你把文件该错了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是损失几个月的工作成果。现在，我们回顾一下readme.txt文件一共有几个版本被提交到了Git仓库里：
　　版本1:wrote a readme.txt 

	Git is a version control system.
	Git is free software.
　　版本2：add distributed

	Git is a distributed version control system.
	Git is free software.
　　版本3：append GPL

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
　　然而在实际工作中，我们怎么可能记住上千航文件中每次都修改了什么内容，这是我们可以使用git log命令查看。  
![git log](http://7xp6n9.com1.z0.glb.clouddn.com/[9WG%256ICQ9VDN]$I`_57.png)
　　git log显示了从最近到最远的提交日志，我们可以看到最近的四次提交，最近一次的是add GPL,最早的一次式wrote readme.txt，如果嫌输出信息泰国，可以试下--pretty=oneline参数
![git log --pretty=oneline](http://7xp6n9.com1.z0.glb.clouddn.com/R``]LKKGKNCQDFO53FH.png)

　　在这里要注意的是，黄色字体表示的是commit id(版本号),版本号是一个SHA1计算出来的一个非常大的数字的十六进制表示。每提交一个新版本，实际上Git就会把他们自动串成一条时间线，如果使用可视化工具查看git历史，就可以清楚地看到提交历史的时间线。
　　现在我们开始回退版本，首先我们必须知道当前是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^,网上100个版本写100个显然不太实际，所以写成HEAD~100.现在我们会退到上一个版本"add distributed"，使用 git reset命令

	$ git reset --hard HEAD^
	HEAD is now at ec92c5 add distributed
　　当然还可以继续回退到上一个版本，不过我们先使用 git log 看看现在的版本库的状态
![git log after git reset](http://7xp6n9.com1.z0.glb.clouddn.com/5~X4F[K_DNPOU146PITQA[4.png)  
　　这样最新的那个版本 add GPL就看不到了，那怎么返回到最新版本呢，办法还是有的，只要找到最新版的commit id，依然使用git reset --hard commit id就可以了，commit id没有要写全部，只要写部分足以唯一区分版本库中的commit id就可以了。Git的版本回退速度非常快，因为Git在内部有一个只想当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从只想add GPL改为 add distributed;然后顺便把工作区的文件更新了，所以你让HEAD只想哪个版本号，你就把当前版本定位在哪。然而会出现这一种情况，当你想会退到最新版的时候发现找不到最新版的commit id了，怎么办，这时候使用git reflog命令来显示你最近执行的每一次命令。第三行add GPL前面显示的就是commit id,现在就可以通过该commit id返回到最新了。  
![git reflog](http://7xp6n9.com1.z0.glb.clouddn.com/FT0CH0%257O8B~6~VM36Q5.png)  
#### 7. 工作区和暂存区
##### 名字解释
　　工作区：工作区就是learngit这个目录，
　　版本库：工作区里面有一个隐藏目录 .git,这个就是Git的版本库，版本库里面存了很多东西，其中最重要的就是成为stage(或者index)的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针HEAD。     
![工作空间](http://7xp6n9.com1.z0.glb.clouddn.com/4AAT@_%25WJNATR03RLD.png)  
　　前面讲了，把文件添加到Git仓库是分两步执行的:  
　　第一步: git add 把文件添加进去，实际上就是把文件修改添加到暂存区  
　　第二步: git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支  
　　因为我们创建Git仓库时，Git默认为我们创建了一个分支master,所以我们git commit提交的时候就是往master分支上提交修改的。git add就是将提交的所有修改放到暂存区(stage),然后git commit就可以一次性把暂存区的所有修改提交到主分支中去。一旦提交后，如果你有没有对工作区做任何修改，那么工作区就是 clean 的。现在版本库就变成了这样子。暂存区里面没有任何修改空空如也。  
![工作区](http://7xp6n9.com1.z0.glb.clouddn.com/E0YX5%25BAQ9~%258VJHJHJY.png)
#### 8. 管理修改
　　Git系统管理的是修改，而非文件，比如说我们修改readme.txt,新增内容如下:   

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes.
　　然后执行git add readme.txt, git status;

	$ git add readme.txt
	$ git status
	  On branch master
	　Changes to be committed:
	　  (use "git reset HEAD <file>..." to unstage)
	　
	　      modified:   readme.txt
　　然后在修改readme.txt如下:  

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
　　这个时候提交修改，git commit,然后查看状态

	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")
　　这个时候会发现第二次修改的没有提交，这个时候我们回忆一下，第一次修改工作区里面的内容，然后git add 将工作区里面的修改添加到暂存区中，然后第二次修改，git commit，这个时候提交的是将暂存区中的所有修改提交到主分支中去，也就是说第二次修改并没有被添加到暂存区也就没有被提交的主分支中去。这就是为什么git commit提交之后依然显示readme.txt是modified的状态了。那么我们怎么讲修改commit 到主分支中去呢，第一种是将第二次修改再次add到暂存区中然后将暂存区中的修改commit到主分支中区，另外一种就是第二次修改以后执行git add命令将修改添加到暂存区中，然后执行git commit将两次修改一次性的提交到主分支中。记住每次修改如果没有添加到暂存区中国，commit的时候是不会提交到主分支中去的。
#### 9. 撤销修改
　　在实际编写中难免出现错误，这个时候就得纠正，这个时候可以手动的将文件恢复到上一个版本的状态，然后用git status命令查看一下

	$ git status
	  On branch master
	  Changes not staged for commit:
	    (use "git add <file>..." to update what will be committed)
	    (use "git checkout -- <file>..." to discard changes in working directory)
	 
	        modified:   readme.txt
	 
	no changes added to commit (use "git add" and/or "git commit -a")
　　这个时候执行 git checkout --file丢弃工作区的修改   
	
    $ git checkout -- readme.txt
　　命令　git checkout -- readme.txt意思就是把readme.txt文件在工作区的修改全部撤销，这里有两种状况：  
　　第一种:readme.txt修改后还没有将修改添加到暂存区，现在撤销修改就回到和版本库一模一样的状态  
　　第二种:readme.txt修改后将修改添加到了暂存区，又做了修改，现在撤销修改就回到了添加到暂存区后的状态。总之就是讲这个文件会退到最近一次git commit或git add时的状态。
　　假定你将错误add到了暂存区，在还没有commit之前，可以使用git reset HEAD file将暂存区的修改撤销掉，重新放回工作区，git reset命令既可以回退版本，也可以把暂存区的修改会退到工作区，当我们用HEAD时，表示最新的版本。这个时候暂存区的修改被撤销了是干净的，工作区保留着最近的修改。

　　场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。  
　　场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。  
　　场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
#### 10. 删除文件
　　删除操作在Git看来也是修改，我们测试一下，首先新建一个文件 file.txt并且提交，然后把文件删了，这个时候工作区和版本库就不一致了，执行git status命令就会告诉我们文件被删除了。  
![删除文件](http://7xp6n9.com1.z0.glb.clouddn.com/3K81NZJPR2HQ0HAA.png)
　　如果确实要删除文件，有两个选择：
　　第一种:使用git rm删除文件，并且git commit将修改提交到远程仓库
　　第二种:本地删除了，版本库里面还有呢，这个时候将工作区恢复到最新版本执行 git checkout file.txt命令，将版本库的版本替换工作区的版本,然后执行第一步就可以了。
#### 12. 远程仓库
　　现在我们做的操作都是在本地进行修改、提交，怎么才能提交到服务器来进行多人协作呢，首先我们得有一个Git的服务器，现阶段没必要搭建一个Git服务器，我们可以使用GitHub的服务将Git仓库托管到GitHub上，首先注册GitHub账号，接下来创建SSH Key，在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

	$ ssh-keygen -t rsa -C "youremail@example.com"
　　你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。  
　　如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

　　第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

　　然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
![add ssh key](http://7xp6n9.com1.z0.glb.clouddn.com/ZPI`@11CT{MTDE1S_ZDEEG.png)
　　为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。  
　　当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。  
　　最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。  
　　如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。
#### 12. 添加远程仓库
　　本地创建了Git仓库后，在GitHub上创建了一个Git仓库，并且让两个仓库进行远程同步，这样GitHub上的仓库就可以供其他人来进行远程协作。首先在GitHub上创建仓库(repository),仓库名称learngit   
　　[GitHub创建仓库](https://help.github.com/articles/create-a-repo/)
　　这个时候learngit这个仓库是空的，需要和本地的仓库进行管理，然后把本地仓库提交到GitHub上，根据GitHub的提示，执行以下命令

	$ git remote add origin git@github.com:usename for github/learngit
　　添加后，远程库的名字就是origin，这是Git默认的叫法，下一步就可以把本地仓库的所有内容推送的远程仓库上
![git push -u origin master](http://7xp6n9.com1.z0.glb.clouddn.com/PQOGPTJYQQ0F0_FB3WGS.png)
　　由于远程仓库是空的，我们第一次推送master分支的时候，加上了-u这个参数，Git不但会把本地的master分支内容推送到远程新的master分支中，还会把本地的master分支和远程的master分支关联袭来，在以后的推送或者拉取时就可以简化命令。这个时候GitHub页面中看到的内容就和本地一模一样了。从现在起，只要是提交到远程仓库只用运行下面的命令就可以了。

	$ git push origin master

　　要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；  
　　关联后，使用命令git push -u origin master第一次推送master分支的所有内容；  
　　此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；  
　　分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
#### 13. 从远程仓库克隆
　　之前我们做的是先git init创建本地库，然后创建远程后并随后将本地库与远程库进行关联，假设我们从新开始，最好的方式是先创建远程仓库，然后从远程仓库克隆出本地仓库，登录GitHub，创建一个新的仓库，仓库名称gitskills，勾选Initialize this repository with a README这样在创建仓库的时候会自动创建一个README.md文件。现在将远程仓库克隆到本地使用命令 git clone git@github.com:username/gitskills.git,GitHub给出的地址除了git@github.com:username/gitskills.git之外，还有一个https://github.com/username/gitskills.git这样的地址，两者的区别是:使用https除了速度慢之外，每次push都得输入用户名和密码，而使用git不仅速度快并且也不用每次输入用户名和密码.
#### 14. 分支管理
　　分支在实际开发中有什么作用呢，加入你要开发一个新功能，但是需要两周后才能完成，第一周写了部分功能，如果立刻提交，由于代码还没写完，不完整额代码库会导致别人不能干活了，如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。现在有了分支就不用怕了，创建一个属于自己的分支，别人看不到，还继续在原来的分支上正常工作，而在自己分支上干活，想提交就提交，知道开发完成后合并到主分支上。这样既安全，又不影响别人工作。
　　
##### 创建分支和分支合并
　　在版本绘图iLike，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支，截止到目前，只有一条时间线，在Git里，这个分支交主分支，既master分支，HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以HEAD指向的就是当前分支。一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点，每次提交，master分支斗湖向前移动一步，这样随着你不断提交，master分支的先越来越长。  
![git master HEAD](http://7xp6n9.com1.z0.glb.clouddn.com/XZLD@`DAEVPJYPVQB.png)
　　当我们创建分支时，例如Dev时，Git新建了一个指针交dev，指向master相同的提交，再把HEAD指向dev,就表示当前分支在dev上:
![git dev](http://7xp6n9.com1.z0.glb.clouddn.com/~DP~BSIL9X$A40UX$AR.png)  
　　Git创建一个分支很快，穿了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化，不过从现在开始，对工作区的修改和提交就是诊断dev分支了，比如新提交一次后，dev的指针向前移动一步，而master指针位置不变。  
![](http://7xp6n9.com1.z0.glb.clouddn.com/%25O$D$DTTK00__XY$8Z5.png)  
　　假如我们在dev上的工作弯沉管理，就可以把dev合并到master上，Git怎么合并呢，最简单的方法，就是直接把master指向dev当前的提交，就完成了合并，所以Git合并分支很快，就改改指针，工作区内容也不变。  
![git merge](http://7xp6n9.com1.z0.glb.clouddn.com/G$04~F3_5SEEK@$EDX02U.png)      
　　合并分支后，甚至可以删除dev分支，删除dev分支就是把dev指针给删掉，删掉后，就剩下master分支。  
![git branch -D dev](http://7xp6n9.com1.z0.glb.clouddn.com/BQSQBD0_~DSJS6FZ{%25CX.png)  
　　创建dev分支，然后切换到dev分支:    

	$ git checkout -b dev
	Switched to a new branch 'dev'
　　git checkout 命令加上 -b 参数表示创建并切换，相当于两条命令:

	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'
　　然后用 git branch 命令查看当前分支，该命令会列出所有分支，当前分支前面会标一个 * 号    

	$ git branch
	* dev
	  master
　　然后我们就可以在dev分支上正常提价，比如对readme.txt做修改，加上一行然后提交
	
	Creating a new branch is quick
　　dev分支的工作完成，然后切换回master分支，在查看readme.txt文件，刚才添加的内容不见了，因为那个提交是在dev分支上，而master分支刺客的提交并没有变:
![git checkout master](http://7xp6n9.com1.z0.glb.clouddn.com/`K3E6M]NV$5{_S$9`M8F.png)  
　　现在我们把dev分支的工作成功合并到master分支上:
	
	$ git merge dev
	Updating d13des....
	Fast-forward
	readme.txt |   1 +
	1 file changed, 1 insertion(+)
　　git merge 命令用于合并指定分支到当前分支，合并后，再查看readme.txt的内容，就可以看到和dev分支最新的提交是完全一样的，注意到上面的fast-forward信息，Git告诉我们，这次合并是"快进模式",也就是直接把master指向dev当前的提交，所以合并速度非常快，当然，不是每次合并都能Fast-forward，后面会讲到其它合并方式。合并完成后就额可以放心地删除dev分支了

	$ git branch -d dev
	Deleted branch dev(was ar34s...)
　　删除后，查看branch，就只剩下master分支了:

	$ git branch 
	* master
　　因为创建、合并和删除分支的速度非常快，所以Git鼓励你使用分支完成某个人物，合并后在删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

	查看分支：git branch

	创建分支：git branch <name>
	
	切换分支：git checkout <name>
	
	创建+切换分支：git checkout -b <name>
	
	合并某分支到当前分支：git merge <name>
	
	删除分支：git branch -d <name>
	
	删除远程仓库分支： git push [远程名] :[分知名]
##### 解决冲突
　　创建一个新的分支feature1,在新分支上开发，修改readme.txt最后一行，改为
	
	creating a new branch is quick AND simple
　　在feature1上提交，切换到master分支，Git会自动提示我们当前master分支比远程的master分支要超前一个提交，在master分支上把readme.txt文件最后一行改为:
	create ad new branch is quic & simple
　　在master分支上提交，现在master分支和feature1分支各自都分别有新的提交，变成了这样
![](http://7xp6n9.com1.z0.glb.clouddn.com/DCNDK3JRD8N6KQ1DUC7.png)  
　　这种情况下，Git无法执行快速合并，只能视图把各自的修改合并起来，但这种合并就可能有冲突，我们试试看

	$ git merge feature1
	Auto-merging readme.txt
	CONFLICT (content): Merge conflict in readme.txt
	Automatic merge failed; fix conflicts and then commit the result.
　　果然冲突了，Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交，git status也可以告诉我们冲突的文件:

	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 2 commits.
	#
	# Unmerged paths:
	#   (use "git add/rm <file>..." as appropriate to mark resolution)
	#
	#       both modified:      readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")
	
　　我们可以直接查看readme.txt的内容

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	<<<<<<< HEAD
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick AND simple.
	>>>>>>> feature1
　　Git用<<<<<<,======,>>>>>>标记处不同分支的内容，我们修改如下后保存:

	creating a new branch is quick and simple
　　提交，现在master分支和feature1分支变成了下图所示:
![git merge confilct](http://7xp6n9.com1.z0.glb.clouddn.com/30E248@5BWR2PJ1H7_V5Q6B.png)
　　用带参数的git log可以看到分支的合并情况:

	$ git log --graph --pretty=oneline --abbrev-commit
	*   59bc1cb conflict fixed
	|\
	| * 75a857c AND simple
	* | 400b400 & simple
	|/
	* fec145a branch test
	...
　　当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。  
　　用git log --graph命令可以看到分支合并图。
##### 分支管理策略
　　合并分支时，如果可能，Git会用Fast fowrawd模式，但这种模式下，删除分支后，会丢掉分支信息，如果要强制禁用Fast fowrawd模式，Git就会在merge时生成一个新的commit,这样从分支历史上就可以看到分支信息。
　　下面我们使用 带有参数 --no-ff的命令来合并分支，首先创建并切换dev分支：

	$ git checkout -b dev
	Switched to a new branch 'dev'
　　修改readme.txt文件，并提交一个新的commit,然后切换回master，合并分支加上--no-ff参数，表示禁用Fast-forward模式

	$ git merge --no-ff -m "merge with no-ff" dev
	Merge made by the 'recursive' strategy.
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)
　　因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去，合并后，我们用git log查看分支历史

	$ git log --graph --pretty=oneline --abbrev-commit
	*   7825a50 merge with no-ff
	|\
	| * 6224937 add merge
	|/
	*   59bc1cb conflict fixed
　　可以看到不使用fast-forward模式，merge后就像这样
![git merge --no-ff -m "commit message" dev](http://7xp6n9.com1.z0.glb.clouddn.com/08SEW]18_DI497F8EFRYJL.png)  
###### 分支策略
　　在实际开发中，我们应该按照几个基本原则进行分支管理:
　　首先:master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活，那在哪儿干活呢，干活都在dev分支上，也就是说，dev分支是不稳定的，比如要发布1.0版本的时候，把dev分支合并到主分支上，在主分支上发布1.0版本。每个人都在dev分支上干活，每个人都有自己的分支，时不时的合并分支到dev分支。所以团队合作的分支看起来就像这样:
![分支管理](http://7xp6n9.com1.z0.glb.clouddn.com/MJ8L8YI~457G`}QRQS}HQ.png)
##### Bug分支
　　软件开发中难免遇到bug，有了bug就要修复，在Git中，由于分支功能是如此强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后合并分支，然后将临时分支删除。当你接到一个修复代号101的bug的任务时，很自然地，创建一个分支issure-101来修复它，但是当前正在dev上进行的工作还没有提交，并不是不想提交，而是工作只进行了一半，没法提交，预计一天完成，但是bug必须在两个小时修复，怎么办？型号，Git还提供了一个stash功能，可以把当前工作现场储藏起来，等以后恢复现场后继续工作:
	
	$ git stash
	Saved working directory and index state WIP on dev: 6224937 add merge
	HEAD is now at 6224937 add merge
　　现在用git status查看工作区，就是干净的(除非有没有被Git管理的文件)，因此可以放心的创建分支来修复bug，首先要确定在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支

	$ git checkout master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 6 commits.
	$ git checkout -b issue-101
	Switched to a new branch 'issue-101'
　　现在修复bug，把Git is free software...改为Git is a free software,然后提交。修复完成后，切换到master分支，并完成合并，最后删除issue-101分支，bug修复了，是时候接着回到dev分支干活了，工作区是干净的，刚才的工作现场存到哪里去了，用git stash list命令查看，工作现场还在，Git把stash内容存在了某个地方，要恢复一下有两个办法:
　　办法一:用git stash apply恢复，回复后stash内容并没有删除，要用git stash drop来删除
　　办法二:用git stash pop，恢复的同事把stash内容也删了。
　　可以多次stash,恢复的时候，先用git stash list查看，然后恢复到至此那个的stash,用命令:
	
	$ git stash apply stash@{0}
　　修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；  
　　当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
##### Feature分支
　　软件开发中，总有无穷无尽的新功能要不断添加进来，添加一个新功能时，你肯定不希望因为一些实验性质的代码把主分支搞乱了，所以，没添加一个新功能，最好新建一个feature分支，在上面进行开发，完成后，合并，最后，删除该feature分支。现在，你终于接到了一个新任务，开发一个新功能，在dev分支上创建一个新分支用于开发新功能，开发完成后提交，切换到dev分支，然后合并，但是就在此时，接到上级命令，因经费不足，新功能必须取消！虽然白干了，但是这个分支还是必须就地销毁：这个时候使用
	
	$ git branch -d feature1
　　删除分支会删除失败，这个时候需要我们强行删除
	
	$ git branch -D feature1

   开发一个新feature，最好新建一个分支；   
   如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。 
##### 多人协作
　　当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来，并且远程仓库的默认名称是origin,要查看远程库的信息，用git remote

	$ git remote
	origin
　　或者用 git remote -v 显示更详细的信息:
	
	$ git remote -v
	origin  git@github.com:michaelliao/learngit.git (fetch)
	origin  git@github.com:michaelliao/learngit.git (push)
　　上面显示了可以抓取和推送的origin的分支。如果没有推送权限，就看不到push的地址
###### 推送分支
　　推送分支是把该分支上的所有本地提交推送到远程库，推送时要指定本地分支，这样Git就会把给分支推送到远程库对应的远程分支上
	
	$ git push origin master
　　要推送其它分支，比如dev，就改成:

	$ git push origin dev
　　但是，并不是一定要把本地分支往远程推送，那么哪些分支需要推送，哪些不需要呢
	* master分支是主分支，因此要时刻与远程同步
	* dev分支是开发分支，团队所有成员都需要在上面工作，所以需要与远程同步
	* bug分支用于本地修复bug，就没必要推送到远程了，除非来办要看你每周到底修复了几个bug
	* feature分支是否推送远程，取决于你是否和你的同事合作在上面开发
　　总是，分支完全可以在本地自己提交修改，至于是否推送，取决于是否和团队其他成员有代码交互需要
###### 抓取分值
　　多人协作时，大家都会往master和dev分支上推送各自的修改，现在模拟一个小伙伴，可以在另一台电脑或者同一台电脑的另一个目录下克隆，默认情况下，你的小伙伴只能看到本地的master分支，现在需要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地分支

	$ git checkout -b dev origin/dev
　　现在，他就可以在dev上继续修改，然后时不时的把dev分支push到远程。你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的问题件做了修改，并试图推送:
	
	$ git push origin dev
	To git@github.com:michaelliao/learngit.git
	 ! [rejected]        dev -> dev (non-fast-forward)
	error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
	hint: before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.
　　这个时候就会推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后在本地合并，解决冲突，然后推送:

	$ git pull
	remote: Counting objects: 5, done.
	remote: Compressing objects: 100% (2/2), done.
	remote: Total 3 (delta 0), reused 3 (delta 0)
	Unpacking objects: 100% (3/3), done.
	From github.com:michaelliao/learngit
	   fc38031..291bea8  dev        -> origin/dev
	There is no tracking information for the current branch.
	Please specify which branch you want to merge with.
	See git-pull(1) for details
	
	    git pull <remote> <branch>
	
	If you wish to set tracking information for this branch you can do so with:
	
	    git branch --set-upstream dev origin/<branch>
　　git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的连接，根据提示设置dev和origin/dev的链接

	$ git branch --set-upstream dev origin/dev
	Branch dev set up to track remote branch dev from origin.
　　再次抓取最新的代码:

	$ git pull
	Auto-merging hello.py
	CONFLICT (content): Merge conflict in hello.py
	Automatic merge failed; fix conflicts and then commit the result.
　　这次git pull成功，但是合并有冲突，解决冲突的办法之前已经讲过，就不再叙述了。
　　多人协作的工作模式通常是这样：

　　首先，可以试图用git push origin branch-name推送自己的修改；

　　如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

　　如果合并有冲突，则解决冲突，并在本地提交；

　　没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

　　如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

　　这就是多人协作的工作模式，一旦熟悉了，就非常简单。

　　小结

　　查看远程库信息，使用git remote -v；

　　本地新建的分支如果不推送到远程，对其他人就是不可见的；

　　从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

　　**在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；**

　　建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

　　从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
#### 15. 标签管理
##### 标签管理
　　发布一个版本时，我们通常现在版本库中打一个标签，这样就唯一确定了打标签时的版本，将来无论什么时候，取某个标签的版本，就是那个打标签的时候的历史版本取出来，所以，标签也是版本库的一个快照。
　　Git的标签虽然是版本库的快照，但其实他就是指向某个commit的指针(根分支很想对不对，但是分支是可以移动的，标签不能移动)，所以创建和删除标签是瞬间完成的
##### 创建标签
　　在git中打标签是非常简单的，首先，切换到需要打标签的分支上:然后使用命令 git tag <name>就可以打一个新标签

	$ git tag v1.0
　　可以用git tag命令查看所有标签，默认标签是打在最新提交的commit上的，有时候如果忘了打标签，比如现在已经是周五了，但应该在周一打的标签没有打，怎么办，凡是是找到历史提交的commit id，然后打上就可以了。

	$ git tag <name> commit id
　　注意，标签不是按时间顺序列出的，是按字母顺序列出的，可以用git show <tagname>查看标签信息
　　还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字:

	$ git tag -a <name> -m "description" commit id
　　还可以通过-s用私钥签名一个标签:
	$ git tag -s <name> -m "description" commit id
　　签名采用PGP签名，因此必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有密钥对，就会报错:

	gpg: signing failed: secret key not available
	error: gpg failed to sign the data
	error: unable to sign the tag
##### 操作标签
　　如果标签打错了，也可以删除:
	
	$ git tag -d v0.1
	Deleted tag 'v0.1' (was e078af9)
　　因为创建的标签都只存储在本地，不会自动推送到远程，所以打错的标签可以在本地安全删除，如果要推送某个标签到远程库，使用命令　git push origin <tagname>,或者一次性地推送全部未推送的标签推送到远程仓库git push origin --tags
　　如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除:
	
	$ git tag -d v0.9
	Deleted tag 'v0.9' (was 6224937)
　　然后，从远程删除，删除命令也是push,但是格式如下:

	$ git push origin :refs/tags/<tagname>
	To git@github.com:michaelliao/learngit.git
	 - [deleted]         v0.9
　　要看是否从远程库删除了标签，可以登录GitHub查看
#### 8. 自定义Git
##### 自定义Git
　　在安装Git一节中，我们已经配置了user.name和user.email，实际上，Git还有很多配置项，比如，让Git显示颜色，会让命令输出看起来更醒目

	$ git config --global color.ui true
　　这样Git就会适当显示不同的颜色，比如git status命令:文件名就会标上颜色
##### 忽略特殊文件
　　有些时候，你必须把某些文件放到Git目录中，但是又不能提交他们，比如保存了数据库密码的配置文件等，每次git status都会显示untracked files...，这个问题解决起来很简单，在git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git会自动忽略这些文件，不需要从头写.gitignore文件，gitHub已经为我们准备了各种配置文件，主需要组合一下就可以使用了，所有配置文件可以直接在线浏览
[https://github.com/github/gitignore](https://github.com/github/gitignore)
　　忽略文件的原则是:
	1. 忽略操作系统自动生成的文件，比如缩略图等；
	2. 忽略编译生成的中间文件，可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如java编译产生的.class文件
	3. 忽略带有敏感信息的配置文件，比如存放口令的配置文件。
　　举个例子:
　　结社在Windows下进行Python开发，Windows会自动在图片的目录下生成影藏的缩略图文件，如果有自定义目录，目录下就会有desktop.ini文件，因此需要忽略Windows自动生成的垃圾文件

	# windows
	Thumbs.db
	ethumbs.db
	Desktop.ini
　　然后，继续忽略Python编译产生的.pyc、.pyo、.dist等文件或目录
	
	# Python:
	*.py[cod]
	*.so
	*.egg
	*.egg-info
	dist
	build
　　加上你自己定义的文件，最终得到一个完整的.gitignore文件，内容如下：

	# Windows:
	Thumbs.db
	ehthumbs.db
	Desktop.ini
	
	# Python:
	*.py[cod]
	*.so
	*.egg
	*.egg-info
	dist
	build
	
	# My configurations:
	db.ini
	deploy_key_rsa
　　最后希就是把.gitignore也提交到git,大功告成，当然检验.gitignore的标准是git status命令是不是说working directory clean。
##### 配置别名
　　觉得命令太难记了，可以使用别名的方式将难记的命令设置一个别名，这个方式时吉利赞成的，设置别名只需要一个命令：

	$ git config --global alias.别名 原名

	$ git config --global alias.st stash
	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch
　　--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用
　　在撤销修改一节中，我们知道，命令 git reset HEAD file可以把暂存区的修改撤销掉(unstage),重新放回工作区，既然这是一个unstage操作，就可以配置一个unstage别名

	$ git config --global alias unstage 'reset HEAD'
　　配置一个git last,让其中显示最后一次提交信息:

	$ git config --global alias.last 'log -1'
　　甚至有人丧心病狂的把lg配置成了

	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
##### 配置文件
　　配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用，配置文件放哪了?每个仓库的Git配置文件都放在.git/config文件中

	$ cat .git/config 
	[core]
	    repositoryformatversion = 0
	    filemode = true
	    bare = false
	    logallrefupdates = true
	    ignorecase = true
	    precomposeunicode = true
	[remote "origin"]
	    url = git@github.com:michaelliao/learngit.git
	    fetch = +refs/heads/*:refs/remotes/origin/*
	[branch "master"]
	    remote = origin
	    merge = refs/heads/master
	[alias]
	    last = log -1
　　别名就在[alias]后面，要删除别名，直接把相应的行删除即可，而当前用户的Git配置文件在用户主目录下的一个隐藏文件.gitconfig中

	$ cat .gitconfig
	[alias]
	    co = checkout
	    ci = commit
	    br = branch
	    st = status
	[user]
	    name = Your Name
    email = your@email.com
　　配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置
##### 搭建Git服务器
　　在远程仓库一节中，我们讲了远程仓库和本地仓库没有什么不同，纯粹是为了大家交换修改方便，GitHub是一个免费托管开元代码的远程仓库，但是对某些视代码视为生命的商业公司，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用，搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。假设你已经有sudo权限的用户账号，下面，正式开始安装。
　　第一步，安装git
	
	$ sudo apt-get install git
　　第二步，创建一个git用户，用来运行git服务:

	＄ sudo adduser git
　　第三部，创建证书登录
　　收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个
　　第四步，初始化Git仓库
　　先选定一个目录作为Git仓库，假定是、src/sample.git，在/src目录下输入命令

	$ sudo git init --bare sample.git
　　git就会创建一个裸仓库，罗仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都是以.git结尾，然后把owner改为git

	$ sudo chown -R git:git sample.git 
　　第五步，禁用shell登录
　　处于安全考虑，第二步创建的git用户不允许登录shell，可以通过编辑/etc/password文件完成，找到类似下面的一行:

	git:x:1001:1001:,,,:/home/git:/bin/bash
　　改为

	git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
　　这样，git用户就可以正常通过ssh使用git,但无法登录shell，因为我们为git用户指定的git-shell每次已登录就自动退出。
　　第六步，克隆远程仓库

　　管理公钥

　　如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。

　　这里我们不介绍怎么玩Gitosis了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

　　管理权限

　　有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。

　　这里我们也不介绍Gitolite了，不要把有限的生命浪费到权限斗争中。  
　　最后我们在这里列一下git的常用命令手册
![](http://7xp6n9.com1.z0.glb.clouddn.com/git_cheat_sheet.png)
