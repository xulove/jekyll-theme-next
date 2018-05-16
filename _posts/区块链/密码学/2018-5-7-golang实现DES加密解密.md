# 一、golang实现DES加密



## 1.1 对称性加密

> 对称加密过程中涉及到三个数据，key，data，mode ，key是密钥，data是加解密的数据，mode是工作模式。当mode是加密模式时，就把data通过密钥key加密成密文，当mode是解密模式时，就把data通过key解密成明文

<!--more-->

> 英文名称：Data Encryption Standard。

当mode为加密模式时，明文按照64位进行分组，形成明文组。在实际应用中，密钥key只用到64位中的56位，这样具有更高的安全性。

在实际应用中，还存在3DES，这个就是DES的变体，简单的理解就是对明文进行三次加密

DES算法的默认加密模式是ECB模式，就是将数据按照8个字节进行加密和解密。最后一段如果不足8个字节的话，需要补足8个字节，再按照顺序将数据连接起来就完成了加密或者解密。

最后一段补足8个字节的过过程，就是数据补位，不同的补位方式，也就是不同的填充方式。常用的填充方式，PKCS7Padding、PKCS5Padding、NoPadding

### 1.1.1 DES加密过程

>  加密过程

```go
func MyDesEncrypt(origData, key []byte) {
	block, _ := des.NewCipher(key)
	//将明文按秘钥的长度做补码运算，填充的实现
	origData = PKCS5Padding(origData, block.BlockSize())
	//设置加密模式，常见的有CBC或者EBC，有不同的填充方式
	blockMode := cipher.NewCBCDecrypter(block, key)
	//创建明文长度的字节数组
	crypted := make([]byte, len(origData))
	//加密明文
	blockMode.CryptBlocks(crypted, origData)
	//将字节数组转换成字符串
	fmt.Println(base64.StdEncoding.EncodeToString(crypted))
}
//实现明文的补码
func PKCS5Padding(ciphertext []byte, blockSize int) []byte {
	padding := blockSize - len(ciphertext)%blockSize
    //数据长度刚好是block size的整数倍时，也进行了填充，如果不进行填充，unpadding会搞不定。
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	return append(ciphertext, padtext...)
}
```

1.1.2 解密过程

> 解密过程

```go
//DES解密方法
func MyDESDecrypt(data string, key []byte) {
   //将字符串转换成字节数组
   crypted, _ := base64.StdEncoding.DecodeString(data)
   //将字节秘钥转换成block快
   block, _ := des.NewCipher(key)
   //设置解密方式
   blockMode := cipher.NewCBCEncrypter(block, key)
   //创建密文大小的数组变量
   origData := make([]byte, len(crypted))
   //解密密文到数组origData中
   blockMode.CryptBlocks(origData, crypted)
   //去补码
   origData = PKCS5UnPadding(origData)
   //打印明文
   fmt.Println(string(origData))
}



//实现去补码

func PKCS5UnPadding(origData []byte) []byte {
   length := len(origData)
   // 去掉最后一个字节 unpadding 次
   unpadding := int(origData[length-1])
   return origData[:(length - unpadding)]
}
```



