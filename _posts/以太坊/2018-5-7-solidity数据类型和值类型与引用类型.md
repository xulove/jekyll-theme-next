---
title: solidity数据类型及值类型与引用类型-深拷贝与浅拷贝
description: 介绍solidity中的各个数据类型，什么是值类型、什么是引用类型，还有不同数据类型的特点。
categories:
 - 以太坊
 - solidity
 - 智能合约
tags:
 - 以太坊
 - solidity
 - 智能合约
---

keywords: "Solidity开发，Solidity文件导入，Solidity数据类型"

## 一、数据类型

### 1.1 布尔bool

`bool:` 可能的取值为常量值`true`和`false`

`bool` 支持的运算

- `!` 逻辑非
- `&&` 逻辑与
- `||` 逻辑或
- `==` 等于
- `!=` 不等于

### 1.2 整型

#### 1.2.3 有符号整型和无符号整型

`uint`和`int`分别代表无符号整型和有符号整型。整型支持步数8递增。如int8,int16,int24,，一直到int256。在solidity中，int和uint分别代表`int256`和`uint256`

![img](http://om1c35wrq.bkt.clouddn.com/WX20171001-174243@2x.png)



