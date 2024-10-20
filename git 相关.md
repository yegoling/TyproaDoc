# git 相关

## git 安装及配置步骤

### git安装

1. [git下载地址](https://git-scm.com/downloads)

2. 安装时注意修改默认安装路径，其它直接next即可，下载后打开电脑高级系统设置，环境变量，系统变量，添加PATH。

![image-20241020110339962](C:\Users\野狗岭闪电\AppData\Roaming\Typora\typora-user-images\image-20241020110339962.png)

3. 打开cmd，输入"git --version"，出现版本信息则安装成功。

   ![image-20241020110803670](C:\Users\野狗岭闪电\AppData\Roaming\Typora\typora-user-images\image-20241020110803670.png)

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

   ![image-20241020111926427](C:/Users/野狗岭闪电/AppData/Roaming/Typora/typora-user-images/image-20241020111926427.png)

3. 输入git status可以查看当前git仓库信息：![image-20241020112237865](C:/Users/野狗岭闪电/AppData/Roaming/Typora/typora-user-images/image-20241020112237865.png)

### git简单使用

1. 输入"git add ."将当前目录下所有项目添加到**本地**仓库，输入"git commit"，将项目进行提交。

   ```
   将当前目录下所有项目添加到**本地**仓库,"."表示全部
   git add . 
   将项目提交,-m 后面引号里面是本次提交的注释内容，必须写，不然会报错
   git commit -m "firstCommit"
   ```

   