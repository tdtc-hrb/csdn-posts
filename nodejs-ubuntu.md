---
title: "在Ubuntu下安装NodeJS"
date: 2024-12-12T23:08:39+08:00
---
Note: Migrate to Debian before Ubuntu 20.04 ends support (April, 2025).

# 1.node.js
[v22.x/20.x/18.x/16.x](https://github.com/nodesource/distributions)
- v22.x
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```

- v20.x
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
```

- v18.x
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
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
curl --silent --show-error https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn
```

# Ref
- [How to Install Yarn on Ubuntu 18.04](https://phoenixnap.com/kb/how-to-install-yarn-ubuntu)
