# Linux命令

## 系统相关

### 查看系统版本

```bash
uname -a
```

## 内存相关

### 查看CPU的进程利用率

```bash
top
```

查看cpu数量

```bash
lscpu
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/659e8ed0aa023cd1f8f5bc0dce92037a.png)

### 查看显存

每0.5秒刷新并显示显卡设置。实时查看你的GPU的使用情况，这是GPU的设置相关。

```bash
watch -n 0.5 nvidia-smi 
```

