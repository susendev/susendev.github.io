---
title: ubuntu-swift
tags:
---
个人准备着一个小项目，但涉及到一些后端要处理的东西，作为一个 iOS 开发者，又有一台搬瓦工小机器，就想着使用 swift 做一下后端。

## 安装 Swift

这台机器本来是用来实现无障碍访问互联网的，开始系统为 Centos-7-bbr 的，但要在 Centos 上安装 swift 是比较麻烦的，只能换 Ubuntu-16.04,奈何 16.04 的版本想要换内核开启 bbr 的时候怎么也启动不起来，只能换成了 14.04 来开启 bbr，速度杠杠的。

<!-- more -->

接下来就是安装 swift 了，首先安装依赖关系：

``` bash
sudo apt-get install clang libicu-dev
```

下载 swift：

``` bash
wget https://swift.org/builds/swift-4.1-release/ubuntu1404/swift-4.1-RELEASE/swift-4.1-RELEASE-ubuntu14.04.tar.gz
```

解压：

``` bash
tar xzf swift-4.1-RELEASE-ubuntu14.04.tar.gz
```

配置环境变量：

``` bash
vim ~/.bashrc
```

在最后一行添加：

``` bash
export PATH=~/swift-4.1-RELEASE-ubuntu14.04/usr/bin:"${PATH}"
```

使之生效：

``` bash
source ~/.bashrc
```

验证是否安装成功：

``` bash
swift --version
```

成功输出：

``` bash
Swift version 4.1 (swift-4.1-RELEASE)
Target: x86_64-unknown-linux-gnu
```

则表示安装完成。

``` bash
Compile Swift Module 'PerfectCrypto' (7 sources)
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/ByteIO.swift:282:43: error: cannot convert value of type 'UnsafeRawPointer?' to expected argument type 'UnsafeMutableRawPointer!'
                super.init(bio: BIO_new_mem_buf(pointer.baseAddress, Int32(pointer.count)))
                                                ~~~~~~~~^~~~~~~~~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/ByteIO.swift:363:34: error: cannot convert value of type 'String' to expected argument type 'UnsafeMutablePointer<Int8>!'
                super.init(bio: BIO_new_accept(name))
                                               ^~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/ByteIO.swift:394:35: error: cannot convert value of type 'String' to expected argument type 'UnsafeMutablePointer<Int8>!'
                super.init(bio: BIO_new_connect(name))
                                                ^~~~
//usr/include/openssl/bn.h:187:19: error: integer literal is too large to be represented in any integer type
#define BN_MASK         (0xffffffffffffffffffffffffffffffffLL)
                         ^
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:428:31: error: use of unresolved identifier 'EVP_des_ede3_wrap'
                case .des_ede3_wrap:    return EVP_des_ede3_wrap()
                                               ^~~~~~~~~~~~~~~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:456:31: error: use of unresolved identifier 'EVP_aes_128_wrap'
                case .aes_128_wrap:             return EVP_aes_128_wrap()
                                                       ^~~~~~~~~~~~~~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:466:31: error: use of unresolved identifier 'EVP_aes_192_wrap'
                case .aes_192_wrap:             return EVP_aes_192_wrap()
                                                       ^~~~~~~~~~~~~~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:477:31: error: use of unresolved identifier 'EVP_aes_256_wrap'
                case .aes_256_wrap:             return EVP_aes_256_wrap()
                                                       ^~~~~~~~~~~~~~~~
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:480:41: error: use of unresolved identifier 'EVP_aes_128_cbc_hmac_sha256'
                case .aes_128_cbc_hmac_sha256:  return EVP_aes_128_cbc_hmac_sha256()
                                                       ^~~~~~~~~~~~~~~~~~~~~~~~~~~
COpenSSL.EVP_aes_128_cbc_hmac_sha1:1:13: note: did you mean 'EVP_aes_128_cbc_hmac_sha1'?
public func EVP_aes_128_cbc_hmac_sha1() -> UnsafePointer<EVP_CIPHER>!
            ^
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:481:41: error: use of unresolved identifier 'EVP_aes_256_cbc_hmac_sha256'
                case .aes_256_cbc_hmac_sha256:  return EVP_aes_256_cbc_hmac_sha256()
                                                       ^~~~~~~~~~~~~~~~~~~~~~~~~~~
COpenSSL.EVP_aes_256_cbc_hmac_sha1:1:13: note: did you mean 'EVP_aes_256_cbc_hmac_sha1'?
public func EVP_aes_256_cbc_hmac_sha1() -> UnsafePointer<EVP_CIPHER>!
            ^
/root/MyAwesomeProject/.build/checkouts/Perfect-Crypto.git--2502725871431209354/Sources/PerfectCrypto/OpenSSLInternal.swift:361:64: error: cannot convert value of type 'UnsafePointer<UInt8>?' to expected argument type 'UnsafeMutablePointer<UInt8>!'
                guard 1 == EVP_DigestVerifyFinal(ctx, signature.baseAddress?.assumingMemoryBound(to: UInt8.self), mdLen) else {
                                                      ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
error: terminated(1): /root/swift-4.0-RELEASE-ubuntu14.04/usr/bin/swift-build-tool -f /root/MyAwesomeProject/.build/debug.yaml main
```
