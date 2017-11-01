---
title: 配置SSH Keys
date: 2017-11-01 12:11:24
tags: git
---

SSH密钥是一个用来识别值得信赖的电脑在进行GitHub一些操作时,不用输入密码。用户可以生成一个SSH密钥，并按照本节所述的方法将公共密钥添加到你的GitHub帐户。

我们建议你定期检查SSH密钥列表，并删除任何一个长时间没有使用的秘钥.

小贴士:如果你安装的有GitHub的桌面版 ，你可以用它来克隆库而不是进行SSH密钥处理。它还配备了Git的Bash的工具，这是在Windows上运行的git命令的首选方式。

### 检测电脑中是否已有SSH 秘钥

查看是否存在SSH秘钥 
```
ls -al ~/.ssh
```
默认情况下,公共秘钥的文件名是下列之一:
```
id_dsa.pub
id_ecdsa.pub
id_ecdsa.pub
id_ecdsa.pub
```
如果没有一个现有的公共和私有密钥，或者不希望使用任何可用的SSH秘钥来连接到GitHub上，请生成一个新的SSH密钥。

### 生成新的SSH密钥并将其添加到ssh-agent中

> 输入ssh-keygen -t rsa -b 4096 -C "your_email@example.com" (将邮箱替换为你自己的地址)

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com" # Creates a new ssh key, using the provided email as a label Generating public/private rsa key pair.
```

> 当你提示“输入要保存密钥的文件”，然后按Enter键。接受默认文件位置。

```
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```

> 在提示符下，键入一个安全密码(可以为空)。有关详细信息，请参阅“[使用SSH密钥口令](https://help.github.com/articles/working-with-ssh-key-passphrases/)”一节。

```
Enter passphrase (empty for no passphrase): [Type a passphrase] Enter same passphrase again: [Type passphrase again]
```

>将ssh秘钥添加到 ssh-agent,在任意目录右键,选择Git Bash后输入命令确保ssh-agent的启用

```
 start the ssh-agent in the background $ eval "$(ssh-agent -s)" Agent pid 59566
```

> 添加你的SSH密钥到ssh-agent 。如果你使用现有的SSH密钥，而不是生成新的SSH密钥，你需要替换现有的私有密钥文件的名称，以取代id_rsa的命令。

```
$ ssh-add ~/.ssh/id_rsa
```