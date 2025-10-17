---
title: "mail的使用"
description: "mail的其他文章：mail的发送、GNU PG"
date: 2020-04-15T23:08:08+08:00
---


# installation
 [Mutt](http://www.mutt.org/) is a small but very powerful text-based mail client for Unix operating systems.

## 1) install
```bash
yum install mutt
```

## 2) config
```bash
$vi ~/.muttrc
```

```bash
set sendmail="/usr/local/msmtp/bin/msmtp"
set from = "user@domain.com"
set realname = "Realname of the user"
```

received mail:
```bash
set pop_host="pop.163.com"
set pop_user="love_shidie"
set pop_pass=`getpassword email_id`
```

# test

## 1) send mail
```bash
gpg -d ~/.msmtp/love_shidie-passoword.gpg
$mutt -s "test message" -a ~/message121.txt
```

## 2) received mail
```bash
$mutt
```
Press key: G

# Reference
- [Mutt – A Command Line Email Client to Send Mails from Terminal](https://www.tecmint.com/send-mail-from-command-line-using-mutt-command/)
- [Mutt: how to safely store password?](https://unix.stackexchange.com/questions/20570/mutt-how-to-safely-store-password)
- [mail send](https://xiaobin80.gitee.io/csdn/post/ops_mail_msmtp/)
- [gnupg - xiaobin_HLJ80](https://xiaobin80.gitee.io/csdn/post/ops_gnupg/)
