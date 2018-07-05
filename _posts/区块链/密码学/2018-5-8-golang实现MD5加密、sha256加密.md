---
title: golang实现MD5、sha256加密、解密
description: 利用golang自带的库，实现MD5和sha256的加密和解密
categories:
 - golang
 - 密码学
tags:
 - sha256
 - MD5
 - 密码学
 - golang
---

keywords: "golang 密码学"

# 1、标准的hash算法

```go
//标准的hash算法
func  bernstein(key string)int{
 	var hash int;
	for i:=0;i< len(key); i++ {
		hash = 33*hash + int(key[i]);
	}
	return hash;
}
```



# 2、ripemd160 

```go
//ripemd160 利用hash原理，实现加密
//ripemd160 是一个 160 位加密哈希算法。它旨在用于替代 128 位哈希函数 MD4、MD5 和 RIPEMD。
func MyRipem160(){
	fmt.Println("ripem160")
	hasher:=ripemd160.New()
	hasher.Write([]byte("hello world"))
	hashString:=fmt.Sprintf("%x",hasher.Sum(nil))
	fmt.Println(hashString)
}
```

# 3、sha256加密算法

```go
//go中sha256算法,调用自带的sha256的库
func MySha256(){
	//方法一
	sum:=sha256.Sum256([]byte("hello world"))
	fmt.Printf("%x\n",sum)

	//方法二
	h:=sha256.New()
	h.Write([]byte("hello world"))
	fmt.Printf("%x\n",h.Sum(nil))
}
```

## 给文件进行sha256加密

```go

//将文件中内容做256加密
func MyIOSha256(){
	f,_:=os.Open("text");
	defer f.Close()
	h:=sha256.New()
	io.Copy(h,f)
	fmt.Printf("%x\n",h.Sum(nil))

}
```

# 4、MD5加密

```go
//调用go环境自带的MD5
func MyMd5(){
	fmt.Println("md5加密")
	//方法一
	data:=[]byte("hello worlld")
	s:=fmt.Sprintf("%x",md5.Sum(data))
	fmt.Println(s)
	//方法二
	h:=md5.New()
	h.Write(data)
	s=hex.EncodeToString(h.Sum(nil))
	fmt.Println(s)
}
```

