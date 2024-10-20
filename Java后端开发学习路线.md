

# Java后端开发学习路线

## Java后端学习流程图

```mermaid
graph LR
A(Java学习路线)==>B(圆角)
A(Java学习路线)==>C(圆角)
A(Java学习路线)==>D(圆角)
A(Java学习路线)==>E(圆角)
A(Java学习路线)==>F(圆角)
A(Java学习路线)==>G(圆角)
A(Java学习路线)==>H(圆角)

B(计算机基础)==>B1(数据结构与算法)==>B11(数据结构)==>B111(链表，栈，队列，树，堆)
B1(数据结构与算法)==>B12(算法)==>B121(排序查找算法)
B12(算法)==>B122(leecode)
B(计算机基础)==>B2(计算机网络)==>B21(TCP,IP)
B2(计算机网络)==>B22(HTTP版本区别，方法)
B(计算机基础)==>B3(linux操作系统)==>B31(简单的命令)
B3(linux操作系统)==>B32(简单查看服务运行日志，排查异常问题)
B3(linux操作系统)==>B33(写简单的shell脚本)
B(计算机基础)==>B4(设计模式)==>B41(单例模式，工厂模式，策略模式，观察者模式等)

C(Java相关)==>C1(Java基础)==>C11(面向对象的特性与使用)
C1(Java基础)==>C12(常用内部集合的使用：ArrayList,HashMap等)
C1(Java基础)==>C13(泛型)
C1(Java基础)==>C14(异常处理)
C(Java相关)==>C2(JVM)==>C21(JVM是什么，与Java的关系是什么，以及与JVM怎么实现的跨平台)
C2(JVM)==>C22(JVM的内存模型是什么，每个区域的作用是什么)
C2(JVM)==>C23(垃圾回收机制：垃圾回收算法和垃圾回收器)
C2(JVM)==>C24(类加载过程：加载，链接（验证，准备，解析），初始化)

C(Java相关)==>C3(Java多线程)==>C31(基本概念)
C3(Java多线程)==>C32(Java中的锁)
C3(Java多线程)==>C33(常见关键字和线程类的使用)

D(相关开发工具)==>D1(版本控制工具Git)
D(相关开发工具)==>D2(Postman)
D(相关开发工具)==>D3(IntelliJ IDEA)
D(相关开发工具)==>D4(Navicat)
D(相关开发工具)==>D5(Maven)

E(基础框架组件)==>E1(Spring boot)
E(基础框架组件)==>E2(MyBatis)==>E21(简单应用即可)

F(数据库)==>F1(MySQL)==>F11(会写基本的SQL)
F1(MySQL)==>F12(了解MySQL底层有哪些log)
F1(MySQL)==>F13(索引)
F1(MySQL)==>F14(锁)
F1(MySQL)==>F15(事务)
F(数据库)==>F2(Redis)==>F21(了解其应用场景与解决的问题)
F2(Redis)==>F22(基本数据结构,String,List,Hash,Set,Zset)
F2(Redis)==>F23(会简单使用，进行命令行的kv的插入和查询)
F2(Redis)==>F24(过期策略和淘汰策略)
F2(Redis)==>F25(持久化策略，RDB,AOF)
F2(Redis)==>F26(常见场景问题)

G(加分框架)==>G1(Kafka,Rabbit MQ)
G(加分框架)==>G2(Arthas性能调优)
G(加分框架)==>G3(容器虚拟化技术：Docker&Kubernetes)
G(加分框架)==>G4(监控组件Prometheus,Grafana)

H(拓展知识)==>H1(鉴权)
H(拓展知识)==>H2(常见攻防漏洞 XSS,CSRF,SQL注入,Dos和DDoS攻击)
H(拓展知识)==>H3(HTTPS 对称加密非对称加密，签名，证书)
H(拓展知识)==>H4(分布式锁)
H(拓展知识)==>H5(限流算法)
H(拓展知识)==>H6(布隆过滤器)
H(拓展知识)==>H7(Bitmap原理)
```

# Java学习

## Java环境搭建

### IntelliJ IDEA安装

1. 官网直接下载**旗舰版**（**试用版**）。![Snipaste_2024-10-20_20-08-50](image/Snipaste_2024-10-20_20-08-50.png)

2. 自定义安装只勾选创建桌面快捷方式，其它直接默认next，然后直接install：![Snipaste_2024-10-20_20-12-35](image/Snipaste_2024-10-20_20-12-35.png)

3. 安装好后打开会发现提示输入激活码，直接先关闭应用，下载激活工具并激活，教程见：[bilibili某贴](https://www.bilibili.com/read/cv34286633/)，亲测2023的工具能用于24的最新版IDEA

## IDEA配置

### Hello World

1. 打开IDEA，新建项目，注意选择新建**空项目**，有git的话可以新建一个git仓库：![Snipaste_2024-10-20_21-06-31](image/Snipaste_2024-10-20_21-06-31.png)

2. 进入项目，打开文件，新建Java模块，在JDK一栏选择下载17.0.13，自定义目录，安装JDK![Snipaste_2024-10-20_21-12-28](image/Snipaste_2024-10-20_21-12-28.png)

3. 在src中右键，新建包：![Snipaste_2024-10-20_21-17-33](image/Snipaste_2024-10-20_21-17-33.png)

4. 然后在包中右键新建java类，代码中输入main回车快捷添加架子，再输入sout回车快捷添加输出语句：

   ```java
   public class hello {
       public static void main(String[] args) {
           System.out.println("hello");
       }
   }
   ```

5. 最后编译并运行，可以看到成功输出helloworld，生成文件保存在项目中的**out目录**下。

### 快捷键推荐

![Snipaste_2024-10-20_21-32-22](image/Snipaste_2024-10-20_21-32-22.png)

## 相关知识（遇到不懂的知识点看这里）

### JDK的组成

![Snipaste_2024-10-20_19-22-29](image/Snipaste_2024-10-20_19-22-29.png)

![Snipaste_2024-10-20_19-24-48](image/Snipaste_2024-10-20_19-24-48.png)

### IDEA管理Java程序的结构

![Snipaste_2024-10-20_20-40-22](image/Snipaste_2024-10-20_20-40-22.png)

### 数据类型

![Snipaste_2024-10-20_22-22-40](image/Snipaste_2024-10-20_22-22-40.png)

