## Git用法学习

#### Git安装

- Linux系统安装

  通过使用linux命令进行安装。

```java
sudo apt-get install git
```

- Windows 系统安装

  直接前往Git官网进行下载，安装。安装完毕后，在开始菜单找到“Git” -> "Git Bash" 运行Git命令。

#### Git安装后设置

​		通过如下命令行配置`user.name`以及`user.email`确定用户的唯一标识。

```java
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

​		`--global` 表示全局配置，即整个机器上的Git仓库的用户标识全部配置为当前设置。

#### 创建版本仓库

​		通过`cd`命令行进入新建项目目录下或者进入已有项目目录下，通过`git init`命令建立Git仓库。

```java
$ cd learn-git
$ git init
$ Initialized empty Git repository in D:/learn-git/.git/
```

​		至此Git仓库就建立好了，此时项目文件夹下出现了一个`.git`的隐藏文件夹，存放Git的相关文件，使用`ls -ah`可以看到此文件夹。

#### 将文件添加到版本库

​		使用`git add`命令将文件添加到版本仓库，例如项目中新建了一个`readme.txt`文件，通过命令将新文件加入到Git仓库缓存区。

```java
$ git add readme.txt
```

​		运行命令后，若没有显示任何内容则表示新文件添加成功了。

​		之后，使用`git commit`命令，将刚刚加入缓存区的文件提交到新的版本仓库。

```java
$ git commit -m "create new file readme"
[master (root-commit) b7b686e] create new file readme
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
```

​		`-m` 后面用双引号`""`包裹本次提交的说明，可以输入任何内容，但最好是有意义的，说明本次提交修改的内容。

​		运行命令后提示文件提交成功，其中`1 file changed`:说明了有一个文件发生了改动（此次新添加的readme文件），`1 insertion`：表示提交的文件中有一行发生了改变（新建文件时写入了一行内容）。

​		其中，`add`命令相当于将文件加入了缓存区，而`commit`命令才是将整个缓存区提交到了版本仓库中，使用`add`命令时需要将一个一个的文件加入，而`commit`则可以将`add`后的所有文件进行提交，如下所示，`add`可以一次提交多个文件。

```java
$ git add file1.txt
$ git add file2.txt file3.txt file4.txt
$ git commit -m "add 4 file."
```

#### 版本及文件管理

- 文件修改

  ​	首先对上一步提交的`readme.txt`文件进行修改，在其中加入一行内容并保存。之后运行`git status`命令查看当前项目的文件状态。

```java
$ git status
On branch master
Changes not staged for commit:
 (use "git add <file>..." to update what will be committed)
 (use "git checkout -- <file>..." to discard changes in working directory)
  
       modified:   readme.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
```

  ​		运行命令后，显示了当前仓库文件的状态，状态显示`readme.txt`文件修改没有被提交。可以使用`git diff`命令查看文件修改的内容。

```java
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index 4c7a7dd..2f8c943 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1,2 @@
-1231231231231313
\ No newline at end of file
+1231231231231313
+123123
\ No newline at end of file
```

  ​		输出结果显示文件增加了一行内容，之后可以通过使用`add`命令将文件加入缓存区，通过`commit`命令将文件修改提交到版本仓库。提交成功后，使用`git status`命令查看当前状态，显示没有文件需要提交。

```java
$ git status
On branch master
nothing to commit, working tree clean
```

- 版本回退

  ​	首先修改`readme.txt`文件，增加一行内容，并进行提交。

  ​	使用`git log`命令查看提交过的历史版本日志

```
$ git log
commit a4a9d8bbcbe15658b28bd31d8003c3b4f8369763 (HEAD -> master)
Author: zhuhaoran <68935795@163.com>
Date:   Mon Jul 29 16:16:50 2019 +0800

    test

commit 0554248a78ca3a8cb50b9aad2565286941c738f2
Author: zhuhaoran <68935795@163.com>
Date:   Mon Jul 29 16:06:35 2019 +0800

    add123

commit b7b686ec2d18e9cdf3fbdcc8ec20ed8815669ec6
Author: zhuhaoran <68935795@163.com>
Date:   Mon Jul 29 15:19:52 2019 +0800

    create new file readme

```

​		日志中显示了提交的版本号、提交者、日期、说明信息等内容，可以通过在`git log`命令后加上`--pretty=oneline`参数对显示内容进行简化，简化后的内容如下。

```
$ git log --pretty=oneline
a4a9d8bbcbe15658b28bd31d8003c3b4f8369763 (HEAD -> master) test
0554248a78ca3a8cb50b9aad2565286941c738f2 add123
b7b686ec2d18e9cdf3fbdcc8ec20ed8815669ec6 create new file readme
```

​		其中前面的例如`a4a9d.....`的是当前的版本号Git的版本号是用`SHA1`算出来的十六进制数，能有效的防止版本号冲突。后面跟着的内容是用户提交时输入的修改信息内容，其中`test`为最后一次提交的内容版本，上面的`HEAD`表示当前版本。

​		若想将版本回退到之前的版本，需要使用`git reset`命令，具体使用如下。

```
$ git reset --hard HEAD^
HEAD is now at 0554248 add123
```

​		通过如上命令，项目已经回退到了当前版本的上一个版本，`HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本，以此类推。同时可以使用`HEAD~100`这种表示法表示几十上百个之前的版本。也可以通过十六进制的版本号进行回退。

```
$ git reset --hard a4a9d8
HEAD is now at a4a9d8b test
```

​		注意，后面的十六进制版本号不需要输入完全，但也不能输入的位数太少，Git会自动判断部分 输入的版本号属于哪个版本。

​		如果回退了版本之后，通过`git log`也没法查看回退前的版本号了，Git也提供了`git reflog`命令来查看版本回退的历史记录，其中记录了项目更改的记录。

```
$ git reflog
a4a9d8b (HEAD -> master) HEAD@{0}: reset: moving to a4a9d8
0554248 HEAD@{1}: reset: moving to HEAD^
a4a9d8b (HEAD -> master) HEAD@{2}: commit: test
0554248 HEAD@{3}: commit: add123
b7b686e HEAD@{4}: commit (initial): create new file readme
```

- 撤销修改

​		当对文件做出修改后，想要撤销此次的修改，需要使用`git checkout`命令将当前修改回退到当前版本。

```
$ git checkout -- readme.txt
```

​		命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

​		一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

​		一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

​		总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

​		当修改了某文件，并且已经将其`add`进缓存区了，可以使用`git reset HEAD`命令将缓存区的文件撤回。

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
```

​		然后再通过`git checkout`命令将修改的文件撤销滚回到最新版本。如果修改了文件，并且已经`commit`了，如果还没有推送到远程库，还是有办法滚回。

- 文件删除

​		当一个已经存在于Git版本仓库中的文件被删除时，通过`git status`可以看到文件被删除的状态。

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

​		上述内容可以看出，Git已经判断出文件已经被删除了，如若真的想要删除此文件，需要使用`git rm`命令将文件从版本库中删除，之后再通过`commit`命令对删除内容进行提交，至此，文件就真正从版本库中删除了。

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master ead5dd3] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

如若不小心将本地的文件删除了，可以通过使用`git checkout`命令将工作区的文件回复

`checkout`本质上是用版本库中的文件版本替换工作区的文件版本，所以可以用来被修改和删除的内容。

```
$ git checkout -- test.txt
```



#### 本地Git命令总结

- `git init` 再当前目录下建立Git仓库

- `git add <filename 1> [filename 2]...[filename N]` 将修改后或新建的文件加入Git仓库暂存区，可多次使用，同时可一次带有多个参数，其中`filename`表示文件名。

- `git commit -m <message>` 将暂存区的所有文件提交到新版本，其中`message`表示本次提交的改动信息。

- `git status`可以看到当前工作区的状态，可以看到那些文件修改后没有提交，那些文件已经删除之类的相关信息。

- `git diff <filename>`可以查看指定文件中与当前版本修改的内容对比。

- `git log`可以查看版本的历史记录，主要显示版本号，日期，提交人，修改信息等内容，可以通过添加参数，显示精简的历史记录。命令`git log --pretty=oneline`。

- `git reset --hard <版本>` 可以将版本回退到版本号对应的版本，其中<版本>可以使用类似`1094adb...`的版本号，也可以使用`HEAD`，其中`HEAD`表示当前版本，`HEAD^`表示前一个版本，`HEAD^^`表示前两个版本，以此类推，同时，使用`HEAD~X`表示之前第几个版本，X表示之前的版本数，具体的两种用法例子如下：

  ```
  git reset --hard HEAD^    //或者
  git reset --hard HEAD~5   //或者
  git reset --hard 1094a    //版本号无需输入全，可以唯一标识即可
  ```

- `git reflog` 可以查看命令历史，即查看此前版本切换历史记录，从而可以看到回退前的版本号，方便之后修改版本。

- `git checkout -- <filename>`将当前工作区的文件恢复成当前最新已提交版本的内容。

- `git reset HEAD <filename>`使用此命令将以经`add`到暂存区的文件撤回。

- `git rm <filename>` 将工作区的文件删除。需要通过`commit`提交版本才能在新版本中真正删除此文件，如果误删了某个文件，并且没有提交，可以通过`checkout`命令从版本库中找回。

#### 使用SSH方式远程连接GitHub

​		首先查看是否拥有SSH的公钥与私钥，即`id_rsa`和`id_rsa.pub`这两个文件。Linux在用户主目录下查找，Windows用户在C盘User目录下查看，是否存在`.ssh`目录，其中存在着这两个文件。

​		如果没有发现，请打开Shell(Windows打开Git Bash命令行工具)创建SSH Key。

```
$ ssh-keygen -t rsa -C "<emailAddress>"
```

​		需要填入初始化Git时设置的邮箱地址，之后一路回车，直到创建Key成功。

```
$ ssh-keygen -t rsa -C "xxx@xxx.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/xxx/.ssh/id_rsa):
Created directory '/c/Users/xxx/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/xxx/.ssh/id_rsa.
Your public key has been saved in /c/Users/xxx/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:ejtj2Fh2Bbh7SVMBtG8RUVDXLpA8/1nOiO1F2ZgXFJQ xxx@xxx.com
The key's randomart image is:
+---[RSA 2048]----+
|         ooo=B=+=|
|        . ..*. E.|
|         ..o.+ ++|
|        . o...=.*|
|        So +oo Oo|
|       .+ +.. o.=|
|      .*.o   . . |
|      o.=.    .  |
|       ..o       |
+----[SHA256]-----+
```

​		之后就可以在`.ssh`目录中看到那两个文件了，其中`id_rsa`为私钥，需要自己保存不能泄露，`id_rsa.pub`为公钥，可以提交给GitHub作为校验，公钥私钥需要配对进行认证，请保管好自己的私钥。

​		创建成功密钥后，打开自己的GitHub，点击头像旁的箭头，选择`Settings`，之后点击         `SSH and GPG keys` 点击`New SSH Key`按钮，在进入的页面中随意输入`Title`，之后打开`id_rsa.pub`文件，将其中的内容复制到下面的Key输入框中，点击确认，即创建了一个SSH Key。

#### 将本地仓库提交至GitHub

​		为了将我们本地的仓库提交到GitHub，需要在GitHub上创建一个同名的仓库。首先进入自己的GitHub主页，点击旁边`repository`的`new`按钮，在`Repository name`中填入之前本地创建库的名称，点击`Create repository`创建仓库。此时，由于创建的GitHub仓库和本地仓库有同名内容，GitHub给了我们一系列的提示，教我们如何将本地内容同步到GitHub上。

​		现在我们根据提示，在本地运行以下命令。

```
$ git remote add origin git@github.com:<yourGitHubUserName>/<Repository>.git
```

​		其中`git@github.com:`后面跟上自己GitHub的用户名(注意，不是邮箱，而是在自己GitHub主页上显示的用户名)后面跟上`/`后面跟项目名字.git。即`xxx.git`文件。

​		添加成功后，可以把本地内容推送到远程库中。

```
$ git push -u origin master
Logon failed, use ctrl+c to cancel basic credential prompt.
Username for 'https://github.com': XXXX
Enumerating objects: 17, done.
Counting objects: 100% (17/17), done.
Delta compression using up to 8 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (17/17), 1.30 KiB | 63.00 KiB/s, done.
Total 17 (delta 0), reused 0 (delta 0)
To https://github.com/XXXXXX/learn-git.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

​		在第一此Push时可能会被要求输入用户名和密码，可能时SSH密钥没有生效？需要继续研究。

​		之后，项目就会被推到GitHub上，可以在GitHub上看到本地仓库的文件。在此后的Push中，就不需要使用`-u`这个参数了。即`git push origin master`就可以进行推送.



#### 从GitHub克隆库

​		首先在GitHub上建立一个新库，并选择建立`README.md`文件。仓库创建完会自动生成一个文件，下面使用`git clone`命令对库进行克隆。

```
$ git clone git@github.com:XXX/Git-Learn.git
Cloning into 'Git-Learn'...
The authenticity of host 'github.com (XX.XX.XX.XX)' can't be established.
RSA key fingerprint is SHA256:XXXXXXkXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,XX.XX.XX.XX' (RSA) to the list of know hosts.
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (6/6), 5.25 KiB | 1.31 MiB/s, done.
```

​		此时GitHub上的项目就被克隆到的当前目录下，可以打开文件夹查看其中的文件。

​		此时发现了上一步需要输入密码的原因，GitHub克隆或者Push等操作有两种不同的方式，一种是SSH方式，将公钥上传后，仅第一次要在显示的提示处输入yes回车即可，之后都无需进行验证。

​		第二种方式是HTTP方式，用户在输入完命令后会弹出GitHub登录界面进行登录，登录成功后，会要求在命令行输入GitHub用户名并回车，之后会弹出OpenSSH的界面，在其输入密码点击确定，才可以继续运行。具体两种连接如下。

```
git@github.com:XXX/Git-Learn.git       //SSH方式
https://github.com/XXX/Git-Learn.git   //HTTP方式
```

#### 创建与合并分支

​		创建分支可以方便多用户协同开发时，每个用户的修改都不会影响其他人及主分支，每个人都在自己的分支上进行开发，最终合并成一个分支。

​		首先使用`checkout`创建新的`dev`分支，分支名可以根据实际项目起名，`dev`只是举个例子。

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

​		其中`-b`参数标识创建并切换，相当与以下命令。

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

​		使用`git branch`命令查看当前分支。

```
$ git branch
* dev
  master
```

​		显示`*`符号的为当前分支，剩余显示的为其他分支。下面修改readme文件，将内容提交到新分支。

```
$ git add readme.txt
$ git commit -m "branch test"
[dev 0a91fe6] branch test
 1 file changed, 2 insertions(+), 1 deletion(-)
```

​		现在切换到`master`分支，查看文件是否发生变化。

```
$ git checkout master
Switched to branch 'master'
```

​		查看发现，之前修改的内容不见了，此时已经切换到了之前分支的内容。下面，把`dev`分支的内容合并到`master`分支上。

```
$ git merge dev
Updating 2c53992..0a91fe6
Fast-forward
 readme.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
```

​		`git merge`命令用于将指定分支合并到当前分支，查看文件发现内容已经被合并了，之后删除`dev`分支。

```
$ git branch -d dev
Deleted branch dev (was 0a91fe6).
```

删除后，查看`branch`只剩下`master`分支了

```
$ git branch
* master
```

