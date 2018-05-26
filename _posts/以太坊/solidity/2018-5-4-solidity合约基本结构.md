---
title: solidity合约基本结构及生命周期
description: 合约的状态变量、局部变量、构造函数、析构函数、生命周期
categories:
 - 以太坊
 - solidity
tags:
 - 以太坊
 - solidity
 - 初步认识合约
---

合约的状态变量、局部变量、构造函数、析构函数、生命周期

# 合约的介绍

简单来说，合约就是运行在区块链上的一段程序

> 下面我们写一段简单的合约

```js
pragma solidity ^0.4.4; //版本声明

contract Counter {  //合约声明
 
    uint count = 0; //count:状态变量
    address owner;  //

    function Counter() {  //构造函数
       owner = msg.sender;
    } 

    function increment() public {
       uint step = 10;
       if (owner == msg.sender) {
          count = count + step;
       }
    }
 
    function getCount() constant returns (uint) {
       return count;
    }

    function kill() {
       if (owner == msg.sender) { 
          selfdestruct(owner);
       }
    }
}
```

- 版本声明

`pragma solidity ^0.4.4;`

0.4.4代表以太坊版本，^代表向上兼容，0.4.4～0.5.0就可以编译此段代码。如果版本高过0.5.0的话，就不能保证这段代码还适用了。

- 合约声明

`contract`是合约关键字，`Counter`是合约名。

如果用java中的类来对比的话：

`contract Counter`就相当与`contract Counter extends Contract`

- 状态变量

`count` 和`owner` 就是状态变量，就相当于我们之前所说类中的属性

- 构造函数

合约对象初始化时调用

- 成员函数

`increament`和`getCount` 都是成员函数，就是java中对象的方法

- 本地变量

成员函数中声明的变量。作用范围在离他最近的`{}`

- 析构函数

`析构函数`和`构造函数`对应，构造函数是初始化数据，而析构函数是销毁数据。在`counter`合约中，当我们手动调用`kill`函数时，就会调用`selfdestruct(owner)`销毁当前合约。