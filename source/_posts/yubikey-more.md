---
title: YubiKey 与 BitLocker
date: 2020-02-09 10:07:00
update: 2020-02-09 10:07:00
tags: 安全
category: bash
copyright: true
comment: true
---

前年的这个时候[买了一个 YubiKey](https://totoro.ink/yubikey.html) ，具体的应用实际上就是并没有什么，除了给我输入一下超长的主密码。

日常逛 V站看到一个[三年前的坟帖子](https://www.v2ex.com/t/376472) 更新了，有人回复了配置 YubiKey 以支持 BitLocker 的方法。

微软的官方教程中就有用智能卡加密硬盘的方法

WIN10 可用的3种方法 <!--more-->`https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/dd875530(v=ws.10)` | WIN7 使用的方法 `https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-7/ee424307(v=ws.10)` Hexo会自动渲染链接里面的括号，所以，请复制到浏览器打开

简单的来说，就是 WIN10 支持使用 CA 的证书、自签名证书、文件加密（EFS）证书作为 BitLocker 智能卡的证书，但是需要简单的配置一下。而 WIN7 只能使用自签名证书。

实际上*从Windows Server 2012和Windows 8开始，微软通过硬盘加密规范完善了BitLocker，该规范允许将BitLocker的加密操作下放到存储设备的硬件中完成*，所以谈 BitLocker 默认当前在用的是 WIN10系统了（WIN8一边去）。


首先我们需要去官网下载 YubiKey Smart Card Minidriver ，也就是 YubiKey 的智能卡证书，确保windows 能够识别我们的智能卡

## CA证书

土豪的选择,基本会送智能卡，可以直接用。

## 自签名证书

注意需要修改注册表

在 `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\FVE` 下新建一个`DWORD`的值，数值名称为`SelfSignedCertificates ` 数值数据为`1`

这样就使得系统允许使用自签名证书加密 BitLocker

![yubikey4](https://totoro.ink/img/yubikey-more.png)

新建两个TXT文件

```
[NewRequest]
Subject = "CN=BitLocker"
KeyLength = 2048
ProviderName = "Microsoft Smart Card Key Storage Provider"
KeySpec = "AT_KEYEXCHANGE" 
KeyUsage = "CERT_KEY_ENCIPHERMENT_KEY_USAGE"
KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG"
RequestType = Cert
SMIME = FALSE
[EnhancedKeyUsageExtension]
OID=1.3.6.1.4.1.311.67.1.1
```

`blcert.txt` 这个是创建加密证书

```
[NewRequest]
Subject = "CN=BitLocker DRA"
KeyLength = 2048
ProviderName = "Microsoft Enhanced Cryptographic Provider v1.0"
Exportable = TRUE 
ExportableEncrypted = FALSE
KeySpec = "AT_KEYEXCHANGE" 
KeyUsage = "CERT_KEY_ENCIPHERMENT_KEY_USAGE"
KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG"
RequestType = Cert
SMIME = FALSE
[EnhancedKeyUsageExtension]
OID=1.3.6.1.4.1.311.67.1.2
```

`bldracert.txt` 这个是创建恢复秘钥

接着使用 CMD 调用证书创建程序创建证书即可

```
certreq –new blcert.txt
certreq –new bldracert.txt
```



## 文件加密证书（EFS)

这种方法是利用了文件加密系统的证书，我们需要在`控制面板\用户帐户\用户帐户`找到管理文件加密证书

按照流程创建一个加密证书放在本地硬盘或者智能卡中即可（推荐放在本地硬盘上可以备份）

然后修改本地策略组（gpedit.msc），在计算机配置、管理模板、Windows组件、BitLocker 驱动器加密、验证智能卡证书使用合规性，设置为已启用，并修改对象标识符为创建的证书的增强型秘钥用法：加密文件系统` (1.3.6.1.4.1.311.10.3.4)`的数值

**或者**直接修改注册表在 `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\FVE` 的`CertificateOID` 数值数据为` (1.3.6.1.4.1.311.10.3.4)`

在 `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\FVE` 下新建一个`DWORD`的值，数值名称为`SelfSignedCertificates ` 数值数据为`1`

怕麻烦的、不会修改注册表的，在桌面新建 `001.reg `复制下面的内容粘贴后运行即可

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\FVE]
"SelfSignedCertificates"=dword:00000001
"CertificateOID"="1.3.6.1.4.1.311.10.3.4"
```





## 最后

BitLocker 可以用来加密 C盘 ，但是要是重装系统的话，进入PE还是很麻烦的。

我只用来加密数据。

PS:自签名证书有效期时长为1年、文件加密（EFS）证书有效期时长为100年

我并不会修改有效期，所以用了文件加密（EFS）证书。



## 总结

EFS 文件加密系统 创建 BitLocker 的操作要点：

1. 下载官网的YubiKey Smart Card Minidriver，在设备管理器更新智能卡的驱动
2. 修改注册表`[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\FVE]`中`"CertificateOID"="1.3.6.1.4.1.311.10.3.4"`（等效于本地策略组修改数值）添加`"SelfSignedCertificates"=dword:00000001 `
3. 生成pfx证书存储到硬盘，或者直接存到 YubiKey（不可再次取出，安全性更高。无法备份，不推荐）
4. 用 YubiKey manager塞入任意一个 piv 证书插槽
5. 在需要启用 BitLocker 的硬盘上右击选择启用即可

其他电脑访问启用智能卡的 BitLocker 磁盘只需要插入 YubiKey 后在设备管理器更新智能卡的驱动包 `YubiKey Smart Card Minidriver`即可

友情提醒：记牢 BitLocker 磁盘的恢复秘钥或者直接多买几个 YubiKey 塞入证书后备用防丢，之后就把证书销毁就好了，不要留下安全隐患。



