# javascript 中!!(两个感叹号，双感叹号) 的作用

---
title: javascript 中!!(两个感叹号，双感叹号) 的作用
description: 使用 javascript 时，有时会在变量前面加上两个感叹号，这样做表示什么含义呢？Javascript 中，! 表示运算符“非”，如果变量不是布尔类型，会将变量自动转化为布尔类型，再取非，那么 用两个!! 就可以将变量转化为对应布尔值。
categories:
 - tutorial
 - javascript
tags:
 -javascript
---

##一、 含义

使用 javascript 时，有时会在变量前面加上两个感叹号，这样做表示什么含义呢？Javascript 中，! 表示运算符“非”，如果变量不是布尔类型，会将变量自动转化为布尔类型，再取非，那么 **用两个!! 就可以将变量转化为对应布尔值**。

# 二、 Javascript 中哪些情况下!! 值为真？

Javascript 中哪些情况下!! 值为真？当变量转化为布尔值 true 时为真咯！

# 三、Javascript 中各种类型如何转换为布尔值？

我们至少可以想到 undifined 和 null 一定是转化为 false 的，数字 0 也一定是 false，那么，空字符串，空数组，空对象呢？别急，下面的实验会有清晰的答案。

首先，定义三个转化布尔值的函数，我们后面会看到这三个函数是等价的, 并且同时输出三个函数的转化结果

```
function trueOrFalseIf(toTest){if(toTest){return true;}
    else{return false;}
}

function trueOrFalseDouble(toTest){return !!toTest;}

function trueOrFalseBoolean(toTest){return new Boolean(toTest);
}

function print(toTest){document.write(trueOrFalseIf(toTest)+","+trueOrFalseDouble(toTest)+","+trueOrFalseBoolean(toTest)+"<br/>");
}
```

依次测试 undefined,null, 空字符串，负 0，正 0，不确定数值，布尔值 false, 布尔值 true，字符串 0，数字 1，数字无穷大，字符串 true, 字符串 false, 空数组，空对象，函数

```
function test(){

var toTestArray=[undefined,null,"",-0,0,NaN,false

,true,"0",1,Infinity,"true","false",[],{},function(){}];

for(var i=0;i<toTestArray.length;i++){print(toTestArray[i]);

}

}
```

测试结果如下：

false,false,false

false,false,false

false,false,false

false,false,false

false,false,false

false,false,false

false,false,false

true,true,true

true,true,true

true,true,true

true,true,true

true,true,true

true,true,true

true,true,true

true,true,true

true,true,true

可以看到前面 7 个值都是 false, 后面 9 个值都是 true。比较值得一提的结论如下：

众望所归，undefined 和 null 为 false。

任意数组，对象，函数（函数是特殊的对象）都转化为真，即使是空数组，空对象。

空字符串为 false，非空字符串为 true。

数值正负 0，不确定值为 false，其它为 true, 无穷大也是 true。

字符串”0″ 和数值 0 可以相互转换，但它们转换为不同的布尔值，即 0 可转换为”0″，”0″ 可转换为 true, 但 0 却转换为 false, 可见 Javascript 中类型转换不具有传递性。

一个更有趣的现象是布尔值 false 会转化为字符串”false”，而字符串”false”却转换为布尔值 true。

```
document.write(new String(false));
document.write(new Boolean("false"));
```

结果为

falsetrue

# 四、总结

回到我们的题目，Javascript 中!!(两个感叹号，双感叹号)可以用来做什么，可以用来判断一些变量的值啊！如果值为真，首先可以排除 undefined 和 null, 根据对象类型，可以做出如下判断：

数值：表示不是 0，且有确定含义的值（包括无穷大）

字符串：表示长度大于 0 的字符串

数组，对象，函数：只能表示不是 undefined 或 null, 并不能判断是否有元素和内容。

另外，我们上面比较了三个函数，结果是一样的，所以下面两个用法其实是 **完全等价** 的：

```
if(!!value){

}

if(value){

}
```

如果作为条件表达式，不需要使用!! 进行转换，Javascript 会自动转换，!! 就只应用于将类型转换为布尔值