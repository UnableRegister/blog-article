要在同一台电脑配置多个 GitHub 账号，需要通过SSH方式来连接远程仓库。

## 创建 ssh key

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -C "your-email-1"  
#创建账号1 的 SSH key
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): id_rsa_first
...
$ ssh-keygen -t rsa -C "your-email-2"  
#创建账号2 的 SSH key
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/xxx/.ssh/id_rsa): id_rsa_second
```

## 添加新密钥到 ssh-agent 中

```
$ ssh-add ~/.ssh/id_rsa_first
$ ssh-add ~/.ssh/id_rsa_second
```

## 将 ssh key 添加到对应的  GitHub 账号
```
#将公钥内容拷贝的剪切板
$ pbcopy < ~/.ssh/id_rsa_first.pub
```
登录 github 并在设置中新增 ssh key。

## 创建 config 文件

```
# 创建 ssh 配置文件
touch ~/.ssh/config
```

在 config 文件添加以下内容：
```
host 1st
    hostname github.com
    Port 22
    User YourUserName1
    IdentityFile /Users/x/.ssh/id_first
host 2nd
    hostname github.com
    Port 22
    User YourUserName2
    IdentityFile /Users/x/.ssh/id_rsa_second
```

## 使用

```
#使用账号1添加远程仓库
$ git remote add origin git@1st:user/repository.git
#使用账号2添加远程仓库
$ git remote add origin git@2st:user/repository.git
#clone 账号1的仓库到本地
$ git clone git@1st:user/repository.git

#取消 global 配置
$ git config --global --unset user.name
$ git config --global --unset user.email

#设置每个项目的 user.email
$ git config  user.email "xxxx@xxx.com"
$ git config  user.name "xxx"
```
