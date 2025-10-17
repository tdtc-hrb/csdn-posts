---
title: "GNU PG的使用"
description: "CentOS"
date: 2022-09-25T00:08:08+08:00
---

The GNU Privacy Guard

# 1. install
```bash
$yum install gnupg
```

```bash
$yum install rng-tools
```

# 2. gen key
```bash
$gpg --gen-key
```
Decompose 20180804215500757.png step:
![gen 1 key 1](https://github.com/tdtc-hrb/csdn/raw/master/images/gpg1key-1.png)
Press Key: enter
![gen 1 key 2](https://github.com/tdtc-hrb/csdn/raw/master/images/gpg1key-2.png)
Press Key: enter
![gen 1 key 3](https://github.com/tdtc-hrb/csdn/raw/master/images/gpg1key-3.png)
Press Key: enter
![gen 1 key 4](https://github.com/tdtc-hrb/csdn/raw/master/images/gpg1key-4.png)
Press Key: y
![gen 1 key 5](https://github.com/tdtc-hrb/csdn/raw/master/images/gpg1key-5.png)
```bash
Real name: xiaobin
Email address: veic_2005@163
Comment: enter(press key)
Press Key: O
```
![gen key 2](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804215653837.png)
输入完，按“Tab”切换到“OK”,按回车。
![gen key 3](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804215907772.png)
再输入一次，
![gen key 4](https://github.com/tdtc-hrb/csdn/raw/master/images/2018080422004955.png)

![gen key 5](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804220108336.png)

## 2.1 gen random
Generate random numbers and done.    
Open a new terminal:
```
rngd -f -r /dev/urandom
```
The old terminal shows the result：
![gen key 6](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804220815965.png)


# 3. gen revoke
```bash
$gpg gen-revoke 7209DBAD
```

![gen revoke 1](https://github.com/tdtc-hrb/csdn/raw/master/images/2018080422175625.png)
Press Key: y
![gen revoke 2](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804221854384.png)

![gen revoke 3](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804221940975.png)

![gen revoke 4](https://github.com/tdtc-hrb/csdn/raw/master/images/20180804222142625.png)


# 4. encrypt file(password)
## 1) password-file
```bash
$vi love_shidie-password
```
> 密码文件(明文密码)
```bash
thisispassword
```

## 2) gen gpg file
> Gen file: love_shidie-password.gpg

```bash
$gpg -e -r veic_2005@163.com love_shidie-password
```

# Reference
- [How to use Mutt email client with encrypted passwords](http://xmodulo.com/mutt-email-client-encrypted-passwords.html)
- [GPG key generation: Not enough random bytes available.](https://linux-audit.com/gpg-key-generation-not-enough-random-bytes-available/)
