# BASE64
[介绍](https://zh.wikipedia.org/wiki/Base64)  
是一种传输编码，不是加密解算法  
JDK:sun.misc.BASE64Encoder & sun.misc.BASE64Decoder  
Apache Commons Codec:Base64  
Bouncy Castle: org.bouncycastle.util.encoders.Base64  



# 消息摘要算法
- MD(Message Digest) 

算法|摘要长度|实现方
-|-|-|
MD2|128|JDK
MD4|128|Bouncy Castle
MD5|128|JDK

- SHA(Secure Hash Algorithm) 安全的摘要算法  

算法|摘要长度|实现方
-|-|-|
SHA-1|160|JDK
SHA-224|224|Bouncy Castle
SHA-256|256|JDK
SHA-384|384|JDK
SHA-512|512|JDK

- MAC(Message Authentication Code) 消息认证码算法  
HMAC(keyed-Hash Message Authentication Code)含有密钥的散列函数算法  
整合MD、SHA  

算法|摘要长度|实现方
-|-|-|
HmacMD2|128|Bouncy Castle
HmacMD4|128|Bouncy Castle
HmacMD5|128|JDK
HmacSHA1|160|JDK
HmacSHA224|224|Bouncy Castle
HmacSHA256|256|JDK
HmacSHA384|384|JDK
HmacSHA512|512|JDK


验证数据完整性

# JAVA安全组成
JCA(Java Cryptography Architecture):提供基本的java安全的基本加密框架，如消息摘要、数字签名等  
JCE(Java Cryptography Extension):对JCA的扩展，提供了很多加密、消息摘要和密钥管理等功能，DES、AES、RSA  
JSSE(Java Secure Socket Extension):提供基于SSL的加密功能，主要用于网络传输  
JAAS(Java Authentication and Authentication Service):  

使用第三方加解密提供者  
jdk中路径：$JAVA_HOME/jre/lib/security/java.security
```
# Note: Providers can be dynamically registered instead by calls to
# either the addProvider or insertProviderAt method in the Security
# class.

#
# List of providers and their preference orders (see above):
#
security.provider.1=sun.security.provider.Sun
security.provider.2=sun.security.rsa.SunRsaSign
security.provider.3=sun.security.ec.SunEC
security.provider.4=com.sun.net.ssl.internal.ssl.Provider
security.provider.5=com.sun.crypto.provider.SunJCE
security.provider.6=sun.security.jgss.SunProvider
security.provider.7=com.sun.security.sasl.Provider
security.provider.8=org.jcp.xml.dsig.internal.dom.XMLDSigRI
security.provider.9=sun.security.smartcardio.SunPCSC
security.provider.10=apple.security.AppleProvider
```

## 相关包、类
### java.security 
消息摘要
### javax.crypto
安全消息摘要、消息认证（鉴别）码
### java.net.ssl
安全套接字 HttpsURLConnection、SSLContext

### 第三方扩展
- Bouncy Castle
支持两种方案：配置、在代码里编码调用
- Apache commons codec


# 术语
- 明文：待加密信息
- 密文：经过加密后的明文
- 加密：明文转密文
- 加密算法：明文转为密文的转换算法
- 加密密钥：通过加密算法进行加密操作 用的密钥
- 解密：将密文转换为明文的过程
- 解密算法： 密文转为明文的算法
- 解密密钥：通过解密算法进行解密操作的密钥
- 密码分析：截获密文者试图通过分析截获的密文从而推断出原来的明文或密钥的过程
- 主动攻击：攻击者非法入侵密码系统，采用伪造、修改、删除等手段向系统注入假消息进行欺骗。对密文具有破坏作用
- 被动攻击：对一个保密系统采取截获密文并对其进行分析和攻击。对密文没用破坏作用
- 密码体制：由明文空间、密文空间、密钥空间、加密算法和解密算法五部分构成
- 密码协议：也称安全协议，指以密码学为基础的消息交换的通信协议，目的是在网络环境中提供安全的服务
- 密码系统：指用于加密、解密的系统
- 柯克霍夫原则：数据的安全基于密钥而不是算法的保密。即系统的安全取决于密钥，对密钥保密，对算法公开。是现代密码学设计的基本原则
# 加解密基础
## 加解密分类
### 按时间分
- 古典密码：以字符为基本加密单元
- 现代密码：以信息块为基本加密单元
### 按保密内容
- 受限制算法：算法的保密性基于保持算法的秘密，应用于军事领域
- 基于密钥算法：算法公开密钥公开
### 按密码体制
- 对称密码：单钥密码和私钥密码，加密密钥与解密密钥相同
- 非对称密码：双钥密码或公钥密码，加密密钥与解密密钥不同，密钥分公钥与私钥
### 明文处理方法 
- 分组密码：指加密时将明文分成固定长度的组，用同一密钥和算法对第一块加密，输出也是固定长度的密文。多用于网络加密
- 流密码：也称序列密码。指加密时每次加密一位或者一个字节明文

## 散列函数
也称hash函数、消息摘要函数、单向函数。  
不是用于数据的加解密，用来验证数据的完整性  

### 特点
- 长度不受限制
- 哈希值容易计算
- 散列计算过程不可逆
### 散列函数相关的算法
- 消息摘要算法MD5等
- SHA 安全散列算法
- MAC 消息认证码算法


## 数字签名
主要是针对以数字的形式存储的消息进行的处理，会生成带有操作者身份信息的编码  
签名者  
数字签名算法

## OSI（Open System Interconnection)安全体系
### 网络通信
应用层  
表示层  
会话层  
传输层  
网络层  
数据链路层  
物理层  
### 安全机制
公正机制、加密机制、数据签名机制、访问控制机制、数据完整性机制、认证机制、业务流填充机制、路由控制机制、公证机制  
### 安全服务
抗否认性服务、 认证/鉴别、访问控制服务、数据保密性服务、数据完整性服务


