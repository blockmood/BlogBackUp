---
title: n切换node版本
date: 2019-04-26 15:10:24
tags: linux
---

## 安装n

首先宣布一个不幸的消息，n不支持win系统。win系统的同学，可以放弃了。nvm也不支持win。

```
npm install n -g
```

再次，说明一个震惊的消息。大多数n命令，都必须sudo。

## 安装各个版本的node

```
sudo n lts   //列出当前的node版本
sudo n stable //更新到最新的Node
sudo n latest //更新到最新的稳定的Node
sudo n 8.4.0  //安装指定的版本
```

## 切换node版本

```
sudo n
```

## 切换node版本后，node版本号不变的问题(node可能被nvm接管)

命令行代码如下：

```
node -v
sudo n
node -v
```

切换后，node的版本居然没有发生变化，解决方案如下：

```
export NODE_HOME=/usr/local
export PATH=$NODE_HOME/bin:$PATH
export NODE_PATH=$NODE_HOME/lib/node_modules:$PATH
```

## n删除一个node版本

```
sudo n rm <版本号>
```

注意:当前版本无法删除
