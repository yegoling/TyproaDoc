# git 相关

## git 安装及配置步骤

### git安装

1. [git下载地址](https://git-scm.com/downloads)

2. 安装时注意修改默认安装路径，其它直接next即可，下载后打开电脑高级系统设置，环境变量，系统变量，添加PATH。

![image-20241020110339962](./image/image-20241020110339962.png)

3. 打开cmd，输入"git --version"，出现版本信息则安装成功。

   ![image-20241020110803670](./image/image-20241020110803670.png)

### git配置

1. 打开cmd，输入命令查看有没有历史注册信息：

   ```
   查看用户名 ：git config user.name
   
   查看密码： git config user.password
   
   查看邮箱：git config user.email
   
   查看配置信息： $ git config --list  
   ```

   没有则进行配置：

   ```
   修改用户名
   git config --global user.name "xxxx(新的用户名)"
   
   修改密码
   git config --global user.password "xxxx(新的密码)"
   
   修改邮箱
   git config --global user.email "xxxx@xxx.com(新的邮箱)"
   ```

   若报错则说明用户名过多，尝试修改：

   ```
   解决办法：$ git config --global --replace-all user.name "你的 git 的名称"
   
   　　　　　$ git config --global --replace-all uesr.email "你的 git 的邮箱
   ```

2. 在cmd中cd进入想进行git管理的文件夹中，输入"git init"初始化，可以看见文件夹中出现.git文件夹，说明初始化成功。

   ![image-20241020111926427](./image/image-20241020111926427.png)

3. 输入git status可以查看当前git**本地**仓库信息：![image-20241020112237865](./image/image-20241020112237865.png)

### git简单使用

1. 输入"git add ."将当前目录下所有项目添加到**本地**仓库，输入"git commit"，将项目进行提交。

   ```
   将当前目录下所有项目添加到**本地**仓库,"."表示全部
   git add . 
   将项目提交,-m 后面引号里面是本次提交的注释内容，必须写，不然会报错
   git commit -m "firstCommit"
   ```

   ![image-20241020112820620](./image/image-20241020112820620.png)

2. 创建github账号，为本次项目新建一个仓库（repository）:

   ![image-20241020113135222](./image/image-20241020113135222.png)

   创建好之后复制仓库的http链接：![image-20241020113325390](./image/image-20241020113325390.png)

   在cmd中输入指令添加远程仓库：

   ```
   git remote add origin https://github.com/yegoling/TyproaDoc.git
   ```

   然后查看远程仓库配置：

   ```
   git remote -v
   ```

   可以看到已经配置成功。![image-20241020113726416](./image/image-20241020113726416.png)

   需要注意这里的远程URL仓库只对本项目有效（即输入"git init"时所在的目录），对不同的项目则需要重新配置。
   
3. 最后将commit到**本地**仓库的项目push到**远程**仓库去:

   ```
   git push
   ```

   这里报错了，意思是：当前分支没有与远程分支关联，因此导致了提交代码失败。![image-20241020114938723](./image/image-20241020114938723.png)

   查看本地分支与远程分支：

   ![image-20241020115152312](./image/image-20241020115152312.png)

   ```
   解决方法1，暴力push到指定分支。
   git push origin master
   解决方法2，先把本地分支push到指定分支中，然后再建立本地分支与该分支的关联。好处是以后就不用再指定分支，可以直接输入git push。
   git push --set-upstream origin master
   ```

   可以看到github仓库已经更新：![image-20241020115826962](./image/image-20241020115826962.png)

   

