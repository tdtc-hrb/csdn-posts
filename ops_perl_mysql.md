---
title: "数据库的查询"
description: "Perl实现"
date: 2020-05-08T16:08:08+08:00
---
Perl操作数据库与其他的语言操作数据库没有什么区别。


- 首先，要连接数据库。

- 然后，执行SQL语句。

- 最后，关闭连接。


# code
循环中为使用游标读取每行数据。

test.pl
```perl
#! /usr/bin/perl

# test DBI and DBD::mysql

use DBI;

$dsn = "DBI:mysql:database=carnumber;host=localhost;port=3306";

my $dbh = DBI->connect($dsn, "root", "qazxsw", {'RaiseError' => 1});


my $strSQL = "select train_number, seriary_number, car_number,".
    " car_marque, past_time from trainOrder where train_number < 100";

my $sth = $dbh->prepare($strSQL);
$sth->execute();

print "TN\tSN\tNumber\tMarque\tPastTime\n";
while (my $ref = $sth->fetchrow_hashref()) {
	print "$ref->{'train_number'}\t".
	    "$ref->{'seriary_number'}\t".
	    "$ref->{'car_number'}\t".
	    "$ref->{'car_marque'}\t".
	    "$ref->{'past_time'}\n";
}

$sth->finish();

$dbh->disconnect();
```
