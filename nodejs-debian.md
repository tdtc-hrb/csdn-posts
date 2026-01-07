---
title: "在Debian下安装NodeJS"
date: 2026-01-06T23:08:39+08:00
---
- 12 (Bookworm)
- 13 (Trixie)
> Official website support: Page not updated.
# 1.node.js
[v24/v22.x/20.x/18.x/16.x](https://github.com/nodesource/distributions)
- v24.x
```
sudo apt install -y curl
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
```
- v22.x
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```
- v20.x
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
```

## install
```bash
sudo apt-get install -y nodejs
```

## update
first remove:
```
sudo apt remove nodejs
```
and then, remove [libnode72](https://unix.stackexchange.com/a/722477):
```
sudo dpkg --remove --force-remove-reinstreq libnode72:amd64
```
last install:
```bash
sudo apt-get install -y nodejs
```


# 2. yarn js
```bash
curl --silent --show-error https://dl.yarnpkg.com/debian/pubkey.gpg | sudo gpg --dearmor -o /usr/share/keyrings/pubkey-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/pubkey-archive-keyring.gpg] \
https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn
```

# Ref
- [How to Install Yarn on Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-yarn-ubuntu)
- [How To: ‘apt-key’ is deprecated, here’s how to fix it](https://community.learnlinux.tv/t/how-to-apt-key-is-deprecated-heres-how-to-fix-it/489)
