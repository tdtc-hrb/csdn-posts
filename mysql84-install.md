---
title: "安装MySQL 8.4"
date: 2024-12-26T00:08:39+08:00
---

## Migration
- [The last release of v5.7, v8.0, and v8.4](https://dev.mysql.com/doc/refman/8.0/en/upgrade-paths.html)
- [Table 3.1 Upgrade Paths for MySQL Server](https://dev.mysql.com/doc/refman/8.4/en/upgrade-paths.html)
```
v5.7 -> v8.0 -> v8.4
```
- Re-enable MySQL Native Password Plugin
```
# Enable mysql_native_password plugin
[mysqld]
mysql_native_password=ON
```

### [mysql upgrade](https://dev.mysql.com/doc/refman/8.0/en/upgrading-what-is-upgraded.html)
|mysql_upgrade Option|Server Option|
|-|-|
|--skip-sys-schema|--upgrade=NONE or --upgrade=MINIMAL|
|--upgrade-system-tables|--upgrade=NONE or --upgrade=MINIMAL|
|--force|--upgrade=FORCE|

- v8.0
```
mysql_upgrade --updgrade-systme-tables
```
- v8.4
```
mysqld --upgrade=MINIMAL
```


## [What Is New in MySQL 8.4 since MySQL 8.0](https://dev.mysql.com/doc/refman/8.4/en/mysql-nutshell.html)
- MySQL native password    
Beginning with MySQL 8.4.0, the deprecated mysql_native_password authentication plugin is no longer enabled by default.

-  [Replication Metadata Repositories](https://dev.mysql.com/doc/refman/8.4/en/replica-logs-status.html)
```
sync-relay-log-info' is deprecated and will be removed in a future release.
```
removed configure at my.ini:
```
# If the value of this variable is greater than 0, a replica synchronizes its relay-log.info file to disk.
# (using fdatasync()) after every sync_relay_log_info transactions.
sync_relay_log_info=10000
```

### authentication policy
```
[ERROR] [MY-013797] [Server] Option --authentication-policy is set to an invalid value. Please check if the specified authentication plugins are valid.
```
to set:
```
authentication_policy=*,,
```

### SQL Mode
```
sql-mode="ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION"
```
