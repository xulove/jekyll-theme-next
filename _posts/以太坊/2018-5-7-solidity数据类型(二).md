---
title: solidity基本数据类型（二）
description: 介绍solidity中的数据类型，固定大小数组，可变长度数组,solidity结构体，字典/映射。
categories:
 - 以太坊
 - solidity
 - 智能合约
tags:
 - 以太坊
 - solidity
 - 智能合约
---

keywords: "Solidity开发，以太坊开发、智能合约开发，Solidity数据类型，Solidity数组、结构体、"

## 1.8 solidity数组

### 1.8.1 固定大小的数组

#### 固定长度类型数组的声明

```
pragma solidity ^0.4.4;

contract C {
    
    // 数组的长度为5，数组里面的存储的值的类型为uint类型
    uint [5] T = [1,2,3,4,5];
}
```

#### 通过length方法获取数组长度遍历数组求总和

```
pragma solidity ^0.4.4;

contract C {
    
    // 数组的长度为5，数组里面的存储的值的类型为uint类型
    uint [5] T = [1,2,3,4,5];
    
    
    // 通过for循环计算数组内部的值的总和
    function numbers() constant public returns (uint) {
        uint num = 0;
        for(uint i = 0; i < T.length; i++) {
            num = num + T[i];
        }
        return num;
    }

}
```

#### 尝试修改T数组的长度

```
pragma solidity ^0.4.4;

contract C {
    
    uint [5] T = [1,2,3,4,5];
    
    
    function setTLength(uint len) public {
        
        T.length = len;
    }
    

}
```

![img](http://om1c35wrq.bkt.clouddn.com/Snip20171028_32.png)

**PS:**声明数组时，一旦长度固定，将不能再修改数组的长度。

#### 尝试修改T数组内部值

```
pragma solidity ^0.4.4;

contract C {
    
    uint [5] T = [1,2,3,4,5];
    
    
    function setTIndex0Value() public {
        
        T[0] = 10;
    }
    
    // 通过for循环计算数组内部的值的总和
    function numbers() constant public returns (uint) {
        uint num = 0;
        for(uint i = 0; i < T.length; i++) {
            num = num + T[i];
        }
        return num;
    }
    
}
```

![img](http://om1c35wrq.bkt.clouddn.com/TindexValue.gif)

`T`数组初始的内容为`[1,2,3,4,5]`，总和为`15` ，当我点击`setTIndex0Value`方法将`第0个`索引的`1`修改为`10`时，总和为`24`。

**PS：**通过一个简单的试验可证明固定长度的数组只是不可修改它的长度，不过可以修改它内部的值，而`bytes0 ~ bytes32`固定大小字节数组中，大小固定，内容固定，长度和字节均不可修改。

#### 尝试通过push往T数组中添加值

```
pragma solidity ^0.4.4;

contract C {
    
    uint [5] T = [1,2,3,4,5];
    
    
    function pushUintToT() public {
        
        T.push(6);
    }
}
```

![尝试通过push往T数组中添加值](http://om1c35wrq.bkt.clouddn.com/Snip20171028_33.png)

**PS:**固定大小的数组不能调用`push`方法向里面添加存储内容，声明一个固定长度的数组，比如：`uint [5] T`，数组里面的默认值为`[0,0,0,0,0]`，我们可以通过索引修改里面的值，但是不可修改数组长度以及不可通过`push`添加存储内容。

### 1.8.2 可变长度的数组

#### 可变长度类型数组的声明

```
pragma solidity ^0.4.4;

contract C {
    
    uint [] T = [1,2,3,4,5];
    
    function T_Length() constant returns (uint) {
        
        return T.length;
    }
    
}
```

`uint [] T = [1,2,3,4,5]`，这句代码表示声明了一个可变长度的`T`数组，因为我们给它初始化了`5`个无符号整数，所以它的长度默认为`5`。

![可变长度类型数组的声明](http://om1c35wrq.bkt.clouddn.com/Snip20171028_34.png)

#### 通过length方法获取数组长度遍历数组求总和

```
pragma solidity ^0.4.4;

contract C {
    
    uint [] T = [1,2,3,4,5];
    
    // 通过for循环计算数组内部的值的总和
    function numbers() constant public returns (uint) {
        uint num = 0;
        for(uint i = 0; i < T.length; i++) {
            num = num + T[i];
        }
        return num;
    }

}
```

![通过length方法获取数组长度遍历数组求总和](http://om1c35wrq.bkt.clouddn.com/Snip20171028_35.png)

#### 尝试修改T数组的长度

```
pragma solidity ^0.4.4;

contract C {
    
    uint [] T = [1,2,3,4,5];
    
    function setTLength(uint len) public {
        
        T.length = len;
    }
    
    function TLength() constant returns (uint) {
        
        return T.length;
    }
}
```

![尝试修改T数组的长度](http://om1c35wrq.bkt.clouddn.com/setTlength.gif)

**PS：**对可变长度的数组而言，可随时通过`length`修改它的长度。

#### 尝试通过push往T数组中添加值

```
pragma solidity ^0.4.4;

contract C {
    
    uint [] T = [1,2,3,4,5];
    
    function T_Length() constant public returns (uint) {
        
        return T.length;
    }
    
    function pushUintToT() public {
        
        T.push(6);
    }
    
    function numbers() constant public returns (uint) {
        uint num = 0;
        for(uint i = 0; i < T.length; i++) {
            num = num + T[i];
        }
        return num;
    }
}
```

![尝试通过push往T数组中添加值](http://om1c35wrq.bkt.clouddn.com/TarrayPush.gif)

**PS：**当往里面增加一个值，数组的个数就会加1，当求和时也会将新增的数字加起来。

### 1.8.3 二维数组 - 数组里面放数组

```
pragma solidity ^0.4.4;

contract C {
    
    uint [2][3] T = [[1,2],[3,4],[5,6]];
    
    function T_len() constant public returns (uint) {
        
        return T.length; // 3
    }
}
```

`uint [2][3] T = [[1,2],[3,4],[5,6]]`这是一个三行两列的数组，你会发现和Java、C语言等的其它语言中二位数组里面的列和行之间的顺序刚好相反。在其它语言中，上面的内容应该是这么存储`uint [2][3] T = [[1,2,3],[4,5,6]]`。

上面的`数组T`是`storage`类型的数组，对于`storage`类型的数组，数组里面可以存放任意类型的值（比如：其它数组，结构体，字典／映射等等）。对于`memory`类型的数组，如果它是一个`public`类型的函数的参数，那么它里面的内容不能是一个`mapping(映射／字典)`，并且它必须是一个`ABI`类型。

#### 创建 Memory Arrays

创建一个长度为`length`的`memory`类型的数组可以通过`new`关键字来创建。`memory`数组一旦创建，它不可通过`length`修改其长度。

```
pragma solidity ^0.4.4;

contract C {
    
    function f(uint len) {
        uint[] memory a = new uint[](7);
        bytes memory b = new bytes(len);
        // 在这段代码中 a.length == 7 、b.length == len
        a[6] = 8;
    }
}
```

#### 数组字面量 Array Literals / 内联数组 Inline Arrays

```
pragma solidity ^0.4.4;


contract C {
    
    function f() public {
        g([1, 2, 3]);
    }
    
    function g(uint[3] _data) public {
        // ...
    }
}
```

![数组字面量 Array Literals / 内联数组 Inline Arrays](http://om1c35wrq.bkt.clouddn.com/Snip20171028_36.png)

在上面的代码中，`[1, 2, 3]`是 `uint8[3] memory` 类型，因为`1、2、3`都是`uint8`类型，他们的个数为`3`，所以`[1, 2, 3]`是 `uint8[3] memory` 类型。但是在`g`函数中，参数类型为`uint[3]`类型，显然我们传入的数组类型不匹配，所以会报错。

**正确的写法如下：**

```
pragma solidity ^0.4.4;

contract C {
    
    function f() public {
        g([uint(1), 2, 3]);
    }
    
    function g(uint[3] _data) public {
        // ...
    }
}
```

在这段代码中，我们将`[1, 2, 3]`里面的第`0`个参数的类型强制转换为`uint`类型，所以整个`[uint(1), 2, 3]`的类型就匹配了`g`函数中的`uint[3]`类型。

#### **memory类型的固定长度的数组不可直接赋值给storage/memory类型的可变数组**

- TypeError: Type uint256[3] memory is not implicitly convertible to expected type uint256[] memory.

```
pragma solidity ^0.4.4;

contract C {
    function f() public {
        
        uint[] memory x = [uint(1), 3, 4];
    }
}
```

```
browser/ballot.sol:8:9: TypeError: Type uint256[3] memory is not implicitly convertible to expected type uint256[] memory.
        uint[] memory x = [uint(1), 3, 4];
        ^-------------------------------^
```

![img](http://om1c35wrq.bkt.clouddn.com/Snip20171028_37.png)

- TypeError: Type uint256[3] memory is not implicitly convertible to expected type uint256[] storage pointer

```
pragma solidity ^0.4.4;

contract C {
    function f() public {
        
        uint[] storage x = [uint(1), 3, 4];
    }
}
```

```
browser/ballot.sol:8:9: TypeError: Type uint256[3] memory is not implicitly convertible to expected type uint256[] storage pointer.
        uint[] storage x = [uint(1), 3, 4];
        ^--------------------------------^
```

- 正确使用

```
pragma solidity ^0.4.4;


contract C {
    function f() public {
        
        uint[3] memory x = [uint(1), 3, 4];
    }
}
```

## 1.9 Solidity 结构体（Structs）

### 1.9.1 自定义结构体

```
pragma solidity ^0.4.4;

contract Students {
    
    struct Person {
        uint age;
        uint stuID;
        string name;
    }

}
```

`Person`就是我们自定义的一个新的结构体类型，结构体里面可以存放任意类型的值。

### 1.9.2 初始化一个结构体

**初始化一个storage类型的状态变量。**

- 方法一

```
pragma solidity ^0.4.4;

contract Students {
    
    struct Person {
        uint age;
        uint stuID;
        string name;
    }

    Person _person = Person(18,101,"liyuechun");

}
```

- 方法二

```
pragma solidity ^0.4.4;

contract Students {
    
    struct Person {
        uint age;
        uint stuID;
        string name;
    }

    Person _person = Person({age:18,stuID:101,name:"liyuechun"});

}
```

**初始化一个memory类型的变量。**

```
pragma solidity ^0.4.4;

contract Students {
    
    struct Person {
        uint age;
        uint stuID;
        string name;
    }
    
    function personInit() {
        
        Person memory person = Person({age:18,stuID:101,name:"liyuechun"});
    }
}
```

## 1.10 Solidity 字典／映射（Mappings）

### 1.10.1 语法

```
mapping(_KeyType => _ValueType)
```

`字典／映射`其实就是一个一对一键值存储关系。

**举个例子：**

**{age: 28, height: 172, name: liyuechun, wx: liyc1215}**

这就是一个映射，满足`_KeyType => _ValueType`之间的映射关系，`age`对应一个`28`的值，`height`对应`160`，`name`对应`liyuechun`, `wx`对应`liyc1215`。

**PS：**同一个映射中，可以有多个相同的**值**，但是**键**必须具备唯一性。

### 1.10.2 案例

```
pragma solidity ^0.4.4;

contract MappingExample {
    
    // 测试账号
    
    // 0xca35b7d915458ef540ade6068dfe2f44e8fa733c
    
    // 0x14723a09acff6d2a60dcdf7aa4aff308fddc160c
    
    // 0x4b0897b0513fdc7c541b6d9d7e929c4e5364d2db
    
    mapping(address => uint)  balances;

    function update(address a,uint newBalance) public {
        balances[a] = newBalance;
    }
    
    // {0xca35b7d915458ef540ade6068dfe2f44e8fa733c: 100,0x14723a09acff6d2a60dcdf7aa4aff308fddc160c: 200,0x4b0897b0513fdc7c541b6d9d7e929c4e5364d2db: 300 }
    
    function searchBalance(address a) constant public returns (uint) {
        
        return balances[a];
    }
}
```

### 1.10.3 结构体和字典综合案例

```
pragma solidity ^0.4.4;

contract CrowdFunding {
    // Defines a new type with two fields.
    struct Funder {
        address addr;
        uint amount;
    }

    struct Campaign {
        address beneficiary;
        uint fundingGoal;
        uint numFunders;
        uint amount;
        mapping (uint => Funder) funders;
    }

    uint numCampaigns;
    mapping (uint => Campaign) campaigns;

    function newCampaign(address beneficiary, uint goal) public returns (uint campaignID) {
        campaignID = numCampaigns++; // campaignID is return variable
        // Creates new struct and saves in storage. We leave out the mapping type.
        campaigns[campaignID] = Campaign(beneficiary, goal, 0, 0);
    }

    function contribute(uint campaignID) public payable {
        Campaign storage c = campaigns[campaignID];
        // Creates a new temporary memory struct, initialised with the given values
        // and copies it over to storage.
        // Note that you can also use Funder(msg.sender, msg.value) to initialise.
        c.funders[c.numFunders++] = Funder({addr: msg.sender, amount: msg.value});
        c.amount += msg.value;
    }

    function checkGoalReached(uint campaignID) public returns (bool reached) {
        Campaign storage c = campaigns[campaignID];
        if (c.amount < c.fundingGoal)
            return false;
        uint amount = c.amount;
        c.amount = 0;
        c.beneficiary.transfer(amount);
        return true;
    }
}
```