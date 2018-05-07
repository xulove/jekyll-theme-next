---
title: solidity合约基本结构及生命周期
description: 以太坊合约的生命周期
categories:
 - 以太坊
 - solidity
tags:
 - 以太坊
 - solidity
 - 智能合约
---

keywords: "Solidity开发，Solidity文件导入，Solidity注释"

## 一、部署简单合约

首先我们在http://remix.ethereum.org，部署下面的合约代码

```javascript
pragma solidity ^0.4.4;
contract Power{
	uint value;
	function Power(uint number,uint p){
		value=number**p;
	}
	function getPower() constant returns(uint) {
		return value;
	}
}
```

remix算是给我们提供的一个开发环境，首先点击`conpile` ，完成后点击`run` ,在`run`的选项卡中，我们可以选择环境部署我们的合约。

`javascript VM` 是这个编辑器给我们提供的一个私有链环境，并且给我们提供了5个初始账户，每个账户有100个eth。

我们传入构造函数所需要的参数，点击`create` 就可以创造出合约对象。

创建成功后，下面就会显示出合约地址，和此合约对象对外可以调用的方法。此处是`getCount`，我们点击`getCount`就会调用合约中的`getCount `方法。

> 注意：
>
> 合约中的构造函数只能有一个。不能像java中构造函数参数不同的话，可以有多个。



## 二、合约升级

我们先给上面的合约增加一个功能

```javascript
pragma solidity ^0.4.4;
contract Counter{
	uint count;
	address owner;
	function Counter(){
		owner=msg.owner;
	}
	function increment()public{
		if(owner==msg.owner){
			count=count+1;
		}
	}
	function getCount()constant returns(uint){
		return count;
	}
	function kill()public{
		if(msg.owner==owner){
			selfdestruct(owner)
		}
	}
}
```

和上面的合约相比，这个合约新增了两点功能

- 在`increment` 方法中，我们进行了判断，如何是这个合约的创始人，才可以增加value。如果不是调用这个方法没有任何操作
- 在`kill` 方法中，我们也进行了一次判断，如果是合约的创世人，才可以销毁合约。

> 上面我们主要到有个`msg` 对象，这个就是每次和合约进行交互的对象。比如我们用账户1创建合约，msg对象就是此时的msg。合约创建成功后，我们用账户2调用合约中的方法，那此时的msg对象就指的是账户2.

>合约销毁之后，我们的合约地址就相当于指向了一个空对象，我们再调用方法，都会报错。



