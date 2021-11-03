---
title: 以太坊与truffle开发平台环境搭建
date: 2020-07-19 12:35:55
tags:
    - 区块链 
    - 以太坊
---

# 以太坊与truffle开发平台环境搭建

## 以太坊是什么

[以太坊（Ethereum）](https://ethereum.org/zh/)，一个集成的区块链环境，支持通过[solidity语言](https://solidity-cn.readthedocs.io/zh/develop/)编写智能合约并编译部署到区块链上，理论上可以实现所有高级语言能够实现的功能。

以太坊基于一个区块链上的虚拟机运作（EVM）。这为其提供了足够高的灵活性，但是这个虚拟机的性能损耗较高。此外，由于这个虚拟机的许多机制并不完善，使得solidity语言也存在着许多缺陷（如局部变量不能超过16个，public型函数不能返回结构体等）。

总的来说，以太坊已经是目前最好用的区块链平台之一，适合于实现一些对区块链的扩展功能。

## truffle是什么

[truffle](https://www.trufflesuite.com/)是一个以太坊的开发框架，里面集成了以太坊本体，solidity编译器与以太坊控制台等等常用功能。同时提供了一键初始化以太坊应用的功能。类似于anaconda之于python。

[truffle官方文档](https://www.trufflesuite.com/docs/truffle/overview)

[solidity中文文档](https://solidity-cn.readthedocs.io/zh/develop/)

[solidity英文原版文档](https://solidity.readthedocs.io/en/v0.6.11/)

> 本文后续内容均基于truffle框架

---

## 以太坊环境搭建

### 1. 安装nodejs
去[nodejs官网](https://nodejs.org/en/download/)下载对应系统的安装包。然后傻瓜式安装。

> ps: 以太坊官方推荐的node版本是 8.10.0LTS。但是经过实际测试，更高的版本并不会导致显著问题。本文后续均基于**Node 12.14.0**

安装完成后在命令行使用以下命令验证安装

```bash
$ node -v
```

### 2. 安装truffle

安装完成nodejs之后，使用以下命令安装truffle

```bash
$ npm install –g truffle
```

注意truffle的版本，这会影响到solidity版本，不同版本的solidity语法与库函数上有较大差别。如果需要指定版本(以4.1.8为例，这个版本也是公认的最稳定，bug最少的版本)，使用`npm install –g truffle@4.1.8`即可。

> 这一过程国内可能有网络问题，可以使用 **科学上网** 或者 **npm换源** 解决，关于npm换源请自行百度

安装完成后使用

```bash
$ truffle version
```

验证安装，这里会输出truffle与其附带的solidity编译器的版本。

### 3. 安装节点仿真器ganache

这个工具是一个专用于开发过程的区块链客户端，可以一键新建区块链账户，同时为区块链的运行过程提供了详细的log显示。

```bash
$ npm install –g ganache-cli
```

安装完成使用以下命令验证安装

```bash
$ ganache-cli
```

这一命令会启动一个区块链网络，默认是本地8545端口，如果没有报错，并显示了10个区块链账户的地址与密钥等信息，最后一行显示`Listening on 127.0.0.1:8545`表明安装正确，使用 Ctrl + C 退出。

[ganache-cli项目与相关文档](https://github.com/trufflesuite/ganache-cli)

### 4. 安装web3.js （可选）

如果你需要使用JavaScript开发区块链应用的话，才需要进行此项目安装。

```bash
$ npm install –g web3@0.20.2
```

> ！！！ 如果此前安装的truffle是4.1.8版本，此处指定web3版本非常重要，新版本的web3与truffle 4.1.8存在兼容性问题

### 5. 安装web3.py （可选）

如果你需要使用python开发区块链应用的话，才需要进行此项目安装。

```bash
$ pip install web3
```

[web3.py官方文档](https://web3py.readthedocs.io/en/stable/quickstart.html#installation)

---

## 建立一个简单的helloworld区块链项目

使用`truffle init`命令下载新项目模板

> 自2019年底，该途径疑似被墙，如果显示下载失败，可以使用`git clone https://github.com/truffle-box/bare-box`直接clone该模板项目，后续操作不变

该模板已经包含一个DApp应用，使用

```bash
$ cd bare-box
```

命令进入该目录后

```bash
$ truffle complie
```

编译智能合约，编译好的合约位于 ./build/contracts 目录下

！！！ 后续操作将与区块链进行交互，现在使用`ganache-cli`命令启动客户端，这一过程可以加上很多参数调整区块链，详情关注[ganache-cli的github页面](https://github.com/trufflesuite/ganache-cli)

接下来部署智能合约，该过程实质上是运行了truffle complie然后执行了./migrate目录中的.js脚本

```bash
$ truffle migrate
```

现在应当可以在ganache-cli中看到输出的区块信息。合约部署完成。随后启动DApp应用

```bash
$ npm run dev
```

接下来可以在[localhost:8080](http://localhost:8080)访问js应用
