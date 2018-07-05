---
title: solidity基本数据类型（一）
description: 介绍solidity中的各个数据类型，整型，布尔，地址，字符串，固定大小字节数组，动态大小字节数组，数组之间的转换。
categories:
 - 以太坊
 - solidity
 - 智能合约
tags:
 - 以太坊
 - solidity
 - 智能合约
---

keywords: "Solidity开发，智能合约开发，Solidity数据类型"

# 一、数据类型

## 1.1 布尔bool

`bool:` 可能的取值为常量值`true`和`false`

`bool` 支持的运算

- `!` 逻辑非
- `&&` 逻辑与
- `||` 逻辑或
- `==` 等于
- `!=` 不等于

## 1.2 整型

#### 1.2.1 有符号整型和无符号整型

`uint`和`int`分别代表无符号整型和有符号整型。整型支持步数8递增。如int8,int16,int24,，一直到int256。在solidity中，int和uint分别代表`int256`和`uint256`

`uint8`的十进制范围：0-255

`int8`的十进制范围：-127～127

- **uint8**: **0b**`00000000` ~ **0b**`11111111`，每一位都存储值，范围为**0 ～ 255**
- **int8**：**0b**`11111111` ~ **ob**`01111111`，最左一位表示符号，`1`表示`负`，`0`表示`正`，范围为**-127 ～ 127**

> 在智能合约中我们要进准判断不同单位的存储范围。address类型等价于`uint160`

#### 1.2.2  整型的算术运算

- 比较：`<=`，`<`，`==`，`!=`，`>=`，`>`，返回值为`bool`类型。
- 位运算符：`&`，`|`，（`^`异或），（`~`非）。
- 数学运算：`+`，`-`，一元运算`+`，`*`，`/`，（`%`求余），（`**`次方），（`<<`左移），（`>>`右移）。

Solidity目前沒有支持`double/float`，如果是 `7/2` 会得到`3`，即无条件舍去。但如果运算符是字面量，则不会截断(后面会进一步提到)。

```javascript
pragma solidity ^0.4.4;

contract Math {
  function mul(int a, int b) constant returns (int) {
      int c = a * b;
      return c;
  }

  function div(int a, int b) constant returns (int) {

      int c = a / b;
      return c;
  }

  function sub(int a, int b) constant returns (int) {
      
      return a - b;
  }

  function add(int a, int b) constant returns (int) {

      int c = a + b;
      return c;
  }
```



![img](http://om1c35wrq.bkt.clouddn.com/WX20171001-174243@2x.png)

![http://om1c35wrq.bkt.clouddn.com/WX20171001-185835@2x.png](http://om1c35wrq.bkt.clouddn.com/WX20171001-185835@2x.png)

## 1.3 地址(Address)

以太坊中的地址的长度为`20`字节，一字节等于8位，一共`160`位，所以`address`其实亦可以用`uint160`来声明。

以太坊中地址用16进制表示，两个16进制数是1个字节，8位。所以16进制数共有：160/8×2=40,所以地址的长度是40。

> 几个常识

- 合约拥有者

`msg.sender`就是当前调用方法时的发起人，一个合约部署后，通过钱包地址操作合约的人很多，但是如何正确判断谁是合约的拥有者，判断方式很简单，就是第一次部署合约时，谁出的`gas`，谁就对合约具有拥有权。

- 合约地址

一个合约部署后，会有一个合约地址，这个合约地址就代表合约自己。

**`this`在合约中到底是`msg.sender`还是`合约地址`，由上图不难看出，`this`即是当前合约地址。**

- 余额  balance

  如果我们需要查看一个地址的余额，我们可以使用`balance`属性进行查看

  `address.balance` 即可获得address的地址的余额

  如果在代码中我们只是想简单查询当前合约余额，我们可以直接通过`this.balance`进行查询。

- #### transfer

  **transfer：**从合约发起方向某个地址转入以太币(单位是wei)，地址无效或者合约发起方余额不足时，代码将抛出异常并停止转账。

  solidity中的转账函数和我们之前理解的有偏差，谁调用`transfer`函数，谁就是接受人。

  `address.transfer()`和`address.send()` 都是如此。

  ```javascript
  pragma solidity ^0.4.4;

  contract PayableKeyword{ 
      
      // 从合约发起方向 0x14723a09acff6d2a60dcdf7aa4aff308fddc160c 地址转入 msg.value 个以太币，单位是 wei
      function deposit() payable{
          address Account2 = 0x14723a09acff6d2a60dcdf7aa4aff308fddc160c;
          Account2.transfer(msg.value);
      }
    
      // 读取 0x14723a09acff6d2a60dcdf7aa4aff308fddc160c 地址的余额
      function getAccount2Balance() constant returns (uint) {
          address Account2 = 0x14723a09acff6d2a60dcdf7aa4aff308fddc160c;
          return Account2.balance;
      }  
      
      // 读取合约发起方的余额
      function getOwnerBalance() constant returns (uint) {
          address Owner = msg.sender;
          return Owner.balance;
      } 
      
  }
  ```

- **send**

`send`相对`transfer`方法较底层，不过使用方法和`transfer`相同，都是从合约发起方向某个地址转入以太币(单位是wei)，地址无效或者合约发起方余额不足时，`send`不会抛出异常，而是直接返回`false`

> send方法的风险

**send()方法执行时有一些风险**

- 调用递归深度不能超1024。
- 如果gas不够，执行会失败。
- 所以使用这个方法要检查成功与否。
- `transfer`相对`send`较安全

## 1.4 字符串(String Literals)

solidity中字符串可以通过`""`或者`''`来表示

我们可以声明一个成员变量

`string  _name; // 状态变量`

然后在后面的方法中赋值

`_name = "liyc1215";`

> string字符串中没有length方法。

## 1.5 固定大小的字节数组

Fixed-size byte arrays

#### 1.5.1 声明方式

固定大小字节数组可以通过 `bytes1`, `bytes2`, `bytes3`, …, `bytes32`来进行声明。PS：`byte`的别名就是 `byte1`。

- `bytes1`只能存储`一个`字节，也就是二进制`8位`的内容。
- `bytes2`只能存储`两个`字节，也就是二进制`16位`的内容。

`byte`和`bytes1`等价，只能存储一个字节，当超过它的存储范围时就会报错

![img](http://om1c35wrq.bkt.clouddn.com/Fixed-size%20byte%20arrays%201.gif)

> 注意我们声明是用`bytes1`或者`bytes2` 而不是`byte1`或者`byte2` 

#### 1.5.2 成员函数

> .length   返回字节的个数

.length只能读取字节的长度，不能修改字节的长度

#### 1.5.3 两个不可变

- 数组长度不可变

  `array.length=newLength;`  字节数组的长度不可变,会报错

- 数组内容不可变

  `array[0]=b;`  会报错。

#### 1.5.4  固定大小数组的强制转换

```javascript
bytes1 public g1 = 0x12;
bytes2 public g2 =0x120a;
bytes2(g1);  //把1字节大小的数组转化成2字节大小的数组，不够两个字节，后面直接添00
bytes1(g2);  //把2字节大小的数组转化成1字节大小的数组，后面一个字节直接丢失
```
## 1.6 动态大小字节数组

(Dynamically-sized byte array)

### 1.6.1 声明

`bytes name = new bytes(length)` - `length`为字节数组长度，来声明可变大小和可修改字节内容的可变字节数组

#### - 创建bytes字节数组

> 这里创建的就是动态字节数组，在数组中我们可以通过`name.length`更改数组的大小，当然也可以通过`.length` 获取到数组的字节长度。

```javascript
pragma solidity ^0.4.4;

contract C {
    
    
    bytes public name = new bytes(1);
    
    
    function setNameLength(uint length) {
        
        name.length = length;
    }
    
    function nameLength() constant returns (uint) {
        
        return name.length;
    }
    
}
```



### 1.6.2 必备知识

- `string` 是一个动态尺寸的`UTF-8`编码字符串，它其实是一个特殊的可变字节数组，`string`是引用类型，而非值类型。
- `bytes` 动态字节数组，引用类型。

根据经验，在我们不确定字节数据大小的情况下，我们可以使用`string`或者`bytes`，而如果我们清楚的知道或者能够将字节书控制在`bytes1` ~ `bytes32`，那么我们就使用`bytes1` ~ `bytes32`，这样的话能够降低存储成本。

### 1.6.3 动态数组和其他类型之间的转换

#### 1.6.3.1  常规字符串和动态数组之间的转换

> 需求：字符串没有提供修改长度和修改内容的方法，但我们可以将字符串转化位动态数组，让后修改动态数组，最终达到修改字符串的目的。

```javascript
pragma solidity ^0.4.4;

contract Obj4{
	string public name = "xxf";
	//bytes3 bt  =  0x787866;  这里的0x787866就是xxf的十六进制表示，3个英文字符，刚好6位十六进制数
	function getBytes()constant returns(bytes){
		return bytes(name);  //0x787866
	}
    function changeName() constant returns (string){
        bytes(name)[0]=87;
        return name;   //Wxf  ，到此我们已经更改了字符串
    }
    function changeName2() constant returns(string){
        bytes(name)[0]=bytes1("d"); //bytes(name)[0]刚好是一个字节
        return name;  //dxf
    }
}
```

#### 1.6.3.2  中文字符串转化为bytes

```javascript
pragma solidity ^0.4.4;

contract Obj5{
	string name  = "徐晓峰";
	function getByte()constant returns(bytes){
		return bytes(name); //0xe5be90e69993e5b3b0  18个数字，9个字节，3个汉字
	}
}
```

> 看结果也符合一个汉字3个字节

### 1.6.4 bytes可变数组length和push两个函数的使用案例

```javascript
pragma solidity ^0.4.4;

contract C {
    
    // 0x6c697975656368756e
    // 初始化一个两个字节空间的字节数组
    bytes public name = new bytes(2);
    
    // 设置字节数组的长度
    function setNameLength(uint len) {     
        name.length = len;
    }
    
    // 返回字节数组的长度
    function nameLength() constant returns (uint) {
        return name.length;
    }
    
    // 往字节数组中添加字节
    function pushAByte(byte b) {
        name.push(b);
    }
    
}
```

> 当字节数组的长度只有2时，如果你通过push往里面添加了一个字节，那么它的长度将变为3，当字节数组里面有3个字节，但是你通过length方法将其长度修改为2时，字节数组中最后一个字节将被从字节数组中移除。

### 1.6.5 动态大小字节数组和固定大小字节数据的区别

- 不可变字节数组

我们之前的文章中提到过如果我们清楚我们存储的字节大小，那么我们可以通过`bytes1`,`bytes2`,`bytes3`,`bytes4`,……,`bytes32`来声明字节数组变量，不过通过`bytesI`来声明的字节数组为不可变字节数组，字节不可修改，字节数组长度不可修改。

- 可变字节数组

我们可以通过`bytes name = new bytes(length)` - `length`为字节数组长度，来声明可变大小和可修改字节内容的可变字节数组。

## 1.7 数组之间的转换

### 1.7.1固定大小字节数组之间的转换

这个在1.5小节已经讲过，这里就不再叙述了

### 1.7.2 固定大小字节数组转化为动态大小字节数组

```javascript
pragma solidity ^0.4.4;

contract C {

   bytes9 name9 = 0x6c697975656368756e;
    
   function fixedSizeByteArraysToDynamicallySizedByteArray() constant returns (bytes) {
       
       return bytes(name9);
   }
    
}
```

> 我们在remix中运行时，发现报错

![åºå®å¤§å°å­èæ°ç»è½¬å¨æå¤§å°å­èæ°ç»](http://om1c35wrq.bkt.clouddn.com/Snip20171027_18.png)

下面是`固定大小字节数组转动态大小字节数组`正确的姿势。

```javascript
pragma solidity ^0.4.4;

contract C {
    
   bytes9 name9 = 0x6c697975656368756e;
    
   function fixedSizeByteArraysToDynamicallySizedByteArray() constant returns (bytes) {
       
       bytes memory names = new bytes(name9.length);
       
       for(uint i = 0; i < name9.length; i++) {
           
           names[i] = name9[i];
       }
       
       return names;
   }
}
```

> 据固定字节大小数组的长度来创建一个`memory`类型的动态类型的字节数组，然后通过一个`for循环`将固定大小字节数组中的字节按照索引赋给动态大小字节数组即可。

### 1.7.3固定大小字节数组不能直接转换为string

```
pragma solidity ^0.4.4;

contract C {
    
    bytes9 names = 0x6c697975656368756e;
    
    function namesToString() constant returns (string) {
        
        return string(names);
    }
   
}
```

![定大小字节数组（Fixed-size byte arrays）不能直接转换为string](http://om1c35wrq.bkt.clouddn.com/Snip20171027_21.png)

### 1.7.4 动态大小字节数组转string

**重要：**因为string是特殊的动态字节数组，所以string只能和动态大小字节数组(Dynamically-sized byte array)之间进行转换，不能和固定大小字节数组进行转行。

- 如果是现成的动态大小字节数组(Dynamically-sized byte array)，如下：

```
pragma solidity ^0.4.4;

contract C {
    
    bytes names = new bytes(2);
    
    function C() {
        
        names[0] = 0x6c;
        names[1] = 0x69;
    }
    
    
    function namesToString() constant returns (string) {
        
        return string(names);
    }
   
}
```

![如果是现成的动态大小字节数组(Dynamically-sized byte array)](http://om1c35wrq.bkt.clouddn.com/Snip20171027_22.png)

### 1.7.5 固定大小字节数组转string

那么就需要先将字节数组转动态字节数组，再转字符串

- 果是固定大小字节数组转string，那么就需要先将字节数组转动态字节数组，再转字符串

```
pragma solidity ^0.4.4;

contract C {

   function byte32ToString(bytes32 b) constant returns (string) {
       
       bytes memory names = new bytes(b.length);
       
       for(uint i = 0; i < b.length; i++) {
           
           names[i] = b[i];
       }
       
       return string(names);
   }
   
}
```

![如果是固定大小字节数组转string，那么就需要先将字节数组转动态字节数组，再转字符串](http://om1c35wrq.bkt.clouddn.com/Snip20171027_25.png)可以通过`0x6c697975656368756e`作为参数进行测试，右边的返回结果**看似**为`liyuechun`，它的实际内容为`liyuechun\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000`，所以在实际的操作中，我们应该将后面的一些列`\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000`去掉。

- 正确的固定大小字节数组转string的代码

```
pragma solidity ^0.4.4;

contract C {
    
    function bytes32ToString(bytes32 x) constant returns (string) {
        bytes memory bytesString = new bytes(32);
        uint charCount = 0;
        for (uint j = 0; j < 32; j++) {
            byte char = byte(bytes32(uint(x) * 2 ** (8 * j)));
            if (char != 0) {
                bytesString[charCount] = char;
                charCount++;
            }
        }
        bytes memory bytesStringTrimmed = new bytes(charCount);
        for (j = 0; j < charCount; j++) {
            bytesStringTrimmed[j] = bytesString[j];
        }
        return string(bytesStringTrimmed);
    }

    function bytes32ArrayToString(bytes32[] data) constant returns (string) {
        bytes memory bytesString = new bytes(data.length * 32);
        uint urlLength;
        for (uint i = 0; i< data.length; i++) {
            for (uint j = 0; j < 32; j++) {
                byte char = byte(bytes32(uint(data[i]) * 2 ** (8 * j)));
                if (char != 0) {
                    bytesString[urlLength] = char;
                    urlLength += 1;
                }
            }
        }
        bytes memory bytesStringTrimmed = new bytes(urlLength);
        for (i = 0; i < urlLength; i++) {
            bytesStringTrimmed[i] = bytesString[i];
        }
        return string(bytesStringTrimmed);
    }    
}
```

![img](http://om1c35wrq.bkt.clouddn.com/Snip20171027_26.png)

`byte char = byte(bytes32(uint(x) * 2 ** (8 * j)))`在上面的代码中，估计大家最难看懂的就是这一句代码，我们通过下面的案例给大家解析：

```
pragma solidity ^0.4.4;

contract C {
    
    // 0x6c
    
    function uintValue() constant returns (uint) {
        
        return uint(0x6c);
    }
    
    function bytes32To0x6c() constant returns (bytes32) {
        
        return bytes32(0x6c);
    }
    
    function bytes32To0x6cLeft00() constant returns (bytes32) {
        
        return bytes32(uint(0x6c) * 2 ** (8 * 0));
    }
    
    function bytes32To0x6cLeft01() constant returns (bytes32) {
        
        return bytes32(uint(0x6c) * 2 ** (8 * 1));
    }
    
    function bytes32To0x6cLeft31() constant returns (bytes32) {
        
        return bytes32(uint(0x6c) * 2 ** (8 * 31));
    }
}
```

![img](http://om1c35wrq.bkt.clouddn.com/Snip20171027_28.png)

- `bytes32(uint(0x6c) * 2 ** (8 * 31));`左移31位
- `bytes32(uint(0x6c) * 2 ** (8 * 1));` 左移1位

通过`byte(bytes32(uint(x) * 2 ** (8 * j)))`获取到的始终是第0个字节。

**总结**



`string`本身是一个特殊的动态字节数组，所以它只能和`bytes`之间进行转换，不能和固定大小字节数组进行直接转换，如果是固定字节大小数组，需要将其转换为动态字节大小数组才能进行转换。
