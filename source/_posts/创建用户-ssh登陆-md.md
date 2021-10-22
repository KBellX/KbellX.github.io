---
title: 创建用户&ssh登陆
date: 2021-10-13 11:16:33
tags:
categories:
---



#### ssh密码方式先连上去

先大概看一遍[SSH方式登陆文档](https://cloud.tencent.com/document/product/1207/44643)

然后，如果急冲冲地想先登上机器再说，尝试用密码登陆时，输入命令

```bash
ssh root@ip
```

会提示`Permission denied (publickey,gssapi-keyex,gssapi-with-mic)`，输入密码的机会都没有。

这时回过头想一想，你根本不知道密码是什么。

需先重置一次root密码后，才能通过SSH方式登陆的。[重置密码文档](https://cloud.tencent.com/document/product/1207/44575)





#### 创建用户

连上去之后，用自己的名字创建一个用户吧

```bash
useradd kbellx
passwd kbellx

visudo
# 添加,这样kbellx在使用sudo时就不用输密码
kbellx  ALL=(ALL)       NOPASSWD:ALL
```



#### ssh配置

1. [添加秘钥](https://cloud.tencent.com/document/product/1207/44573)

2. [绑定密码至实例](https://cloud.tencent.com/document/product/1207/54228)

4. 指定私钥，root用户登录

  ```bash
  ssh ~/.ssh/my-tencent root@ip
  ```

4. 让刚才创建的用户kbellx也能ssh登录。第二步绑定至实例，只是把秘钥绑定到实例的root用户上了，要绑定到自建用户，还得自己操作。



#### 自建用户ssh登录

1. root用户先把用密码登陆方式[打开](https://cloud.tencent.com/document/product/1207/44573)，因为绑定秘钥后会把密码登陆方式关闭

   ```bash
   vim /etc/ssh/sshd_config 
   # PasswordAuthentication改为yes
   systemctl restart sshd
   ```

2. kbellx用户用ssh密码方式登录

3. 配置

```bash
cd ~
mkdir .ssh
chmod 700 ~/.ssh
vim ~/authorized_keys
# 将root用户的authorized_keys内容拷贝过来
chmod 600 ~/.ssh/authorized_keys
```

3. 然后就可以`ssh -i ~/my-tencent kbellx@ip`登录了。
