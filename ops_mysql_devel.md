---
title: "无mysql-dev导致的错误"
description: "centos"
date: 2020-05-15T05:08:08+08:00
---
> 应用程序要访问mysql必须安装mysql-dev，因为它是各种语言与mysql的接口。

# error info

```bash
[root@localhost DBD-mysql-4.022]# make
cp lib/DBD/mysql.pm blib/lib/DBD/mysql.pm
cp lib/DBD/mysql/GetInfo.pm blib/lib/DBD/mysql/GetInfo.pm
cp lib/DBD/mysql/INSTALL.pod blib/lib/DBD/mysql/INSTALL.pod
cp lib/Bundle/DBD/mysql.pm blib/lib/Bundle/DBD/mysql.pm
cc -c  -I/usr/local/perl/lib/site_perl/5.16.2/i686-linux/auto/DBI -I/usr/include/mysql  -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m32 -fasynchronous-unwind-tables -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -fno-strict-aliasing -fwrapv -DDBD_MYSQL_WITH_SSL -DDBD_MYSQL_INSERT_ID_IS_GOOD -g  -fno-strict-aliasing -pipe -fstack-protector -I/usr/local/include -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -O2   -DVERSION=\"4.022\" -DXS_VERSION=\"4.022\" -fPIC "-I/usr/local/perl/lib/5.16.2/i686-linux/CORE"   dbdimp.c
In file included from dbdimp.c:20:
dbdimp.h:23:49: error: mysql.h: No such file or directory
dbdimp.h:24:45: error: mysqld_error.h: No such file or directory
dbdimp.h:26:49: error: errmsg.h: No such file or directory
In file included from dbdimp.c:20:
dbdimp.h:159: error: expected specifier-qualifier-list before ‘MYSQL’
dbdimp.h:257: error: expected specifier-qualifier-list before ‘MYSQL_RES’
In file included from dbdimp.c:20:
dbdimp.h:318: error: expected ‘)’ before ‘*’ token
dbdimp.h:321: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘mysql_st_internal_execute’
dbdimp.h:357: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘*’ token
dbdimp.h:363: error: expected declaration specifiers or ‘...’ before ‘MYSQL_RES’
dbdimp.c:519: error: expected declaration specifiers or ‘...’ before ‘MYSQL’
dbdimp.c: In function ‘parse_params’:
dbdimp.c:767: error: ‘sock’ undeclared (first use in this function)
dbdimp.c:767: error: (Each undeclared identifier is reported only once
dbdimp.c:767: error: for each function it appears in.)
dbdimp.c: At top level:
dbdimp.c:822: error: ‘FIELD_TYPE_VAR_STRING’ undeclared here (not in a function)
dbdimp.c:832: error: ‘FIELD_TYPE_DECIMAL’ undeclared here (not in a function)
dbdimp.c:842: error: ‘FIELD_TYPE_TINY’ undeclared here (not in a function)
dbdimp.c:852: error: ‘FIELD_TYPE_SHORT’ undeclared here (not in a function)
dbdimp.c:862: error: ‘FIELD_TYPE_LONG’ undeclared here (not in a function)
dbdimp.c:872: error: ‘FIELD_TYPE_FLOAT’ undeclared here (not in a function)
dbdimp.c:882: error: ‘FIELD_TYPE_DOUBLE’ undeclared here (not in a function)
dbdimp.c:905: error: ‘FIELD_TYPE_TIMESTAMP’ undeclared here (not in a function)
dbdimp.c:915: error: ‘FIELD_TYPE_LONGLONG’ undeclared here (not in a function)
dbdimp.c:925: error: ‘FIELD_TYPE_INT24’ undeclared here (not in a function)
dbdimp.c:935: error: ‘FIELD_TYPE_DATE’ undeclared here (not in a function)
dbdimp.c:945: error: ‘FIELD_TYPE_TIME’ undeclared here (not in a function)
dbdimp.c:955: error: ‘FIELD_TYPE_DATETIME’ undeclared here (not in a function)
dbdimp.c:965: error: ‘FIELD_TYPE_YEAR’ undeclared here (not in a function)
dbdimp.c:975: error: ‘FIELD_TYPE_NEWDATE’ undeclared here (not in a function)
dbdimp.c:985: error: ‘FIELD_TYPE_ENUM’ undeclared here (not in a function)
dbdimp.c:995: error: ‘FIELD_TYPE_SET’ undeclared here (not in a function)
dbdimp.c:1005: error: ‘FIELD_TYPE_BLOB’ undeclared here (not in a function)
dbdimp.c:1015: error: ‘FIELD_TYPE_TINY_BLOB’ undeclared here (not in a function)
dbdimp.c:1025: error: ‘FIELD_TYPE_MEDIUM_BLOB’ undeclared here (not in a function)
dbdimp.c:1035: error: ‘FIELD_TYPE_LONG_BLOB’ undeclared here (not in a function)
dbdimp.c:1045: error: ‘FIELD_TYPE_STRING’ undeclared here (not in a function)
dbdimp.c:1515: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘*’ token
dbdimp.c: In function ‘my_login’:
dbdimp.c:2001: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2002: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2002: error: ‘MYSQL’ undeclared (first use in this function)
dbdimp.c:2002: warning: comparison between pointer and integer
dbdimp.c:2002: error: invalid operands to binary /
dbdimp.c:2002: error: invalid operands to binary >
dbdimp.c:2002: error: expected expression before ‘)’ token
dbdimp.c:2002: error: invalid operands to binary *
dbdimp.c:2002: warning: passing argument 2 of ‘Perl_safesyscalloc’ makes integer from pointer without a cast
dbdimp.c:2002: error: called object ‘<erroneous-expression>’ is not a function
dbdimp.c:2004: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c: In function ‘mysql_db_login’:
dbdimp.c:2042: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:2042: error: request for member ‘auto_reconnects_ok’ in something not a structure or union
dbdimp.c:2043: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:2043: error: request for member ‘auto_reconnects_failed’ in something not a structure or union
dbdimp.c:2044: error: ‘imp_dbh_t’ has no member named ‘bind_type_guessing’
dbdimp.c:2045: error: ‘imp_dbh_t’ has no member named ‘bind_comment_placeholders’
dbdimp.c:2046: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2048: error: ‘imp_dbh_t’ has no member named ‘auto_reconnect’
dbdimp.c:2057: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2058: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2058: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c: In function ‘mysql_db_commit’:
dbdimp.c:2095: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:2097: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2100: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2105: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2105: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2106: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c: In function ‘mysql_db_rollback’:
dbdimp.c:2125: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:2127: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2130: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2135: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2136: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2136: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c: In function ‘mysql_db_disconnect’:
dbdimp.c:2174: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2175: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c: In function ‘mysql_db_destroy’:
dbdimp.c:2263: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2267: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2275: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c: In function ‘mysql_db_STORE_attrib’:
dbdimp.c:2314: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2322: error: ‘imp_dbh_t’ has no member named ‘no_autocommit_cmd’
dbdimp.c:2328: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2359: error: ‘imp_dbh_t’ has no member named ‘use_mysql_use_result’
dbdimp.c:2361: error: ‘imp_dbh_t’ has no member named ‘auto_reconnect’
dbdimp.c:2363: error: ‘imp_dbh_t’ has no member named ‘use_server_side_prepare’
dbdimp.c:2365: error: ‘imp_dbh_t’ has no member named ‘no_autocommit_cmd’
dbdimp.c:2367: error: ‘imp_dbh_t’ has no member named ‘bind_type_guessing’
dbdimp.c:2369: error: ‘imp_dbh_t’ has no member named ‘bind_type_guessing’
dbdimp.c: At top level:
dbdimp.c:2398: error: expected ‘)’ before ‘val’
dbdimp.c: In function ‘mysql_db_FETCH_attrib’:
dbdimp.c:2428: error: ‘imp_dbh_t’ has no member named ‘has_transactions’
dbdimp.c:2445: error: ‘imp_dbh_t’ has no member named ‘auto_reconnect’
dbdimp.c:2445: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2451: error: ‘imp_dbh_t’ has no member named ‘bind_type_guessing’
dbdimp.c:2451: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2456: error: ‘imp_dbh_t’ has no member named ‘bind_comment_placeholders’
dbdimp.c:2456: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2462: warning: initialization makes pointer from integer without a cast
dbdimp.c:2468: warning: passing argument 1 of ‘Perl_sv_2mortal’ makes pointer from integer without a cast
dbdimp.c:2473: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2477: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2477: warning: initialization makes pointer from integer without a cast
dbdimp.c:2491: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:2491: error: request for member ‘auto_reconnects_ok’ in something not a structure or union
dbdimp.c:2491: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2498: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:2498: error: request for member ‘auto_reconnects_failed’ in something not a structure or union
dbdimp.c:2498: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2512: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2512: warning: initialization makes pointer from integer without a cast
dbdimp.c:2521: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2521: warning: initialization makes pointer from integer without a cast
dbdimp.c:2526: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2526: warning: passing argument 1 of ‘Perl_sv_2mortal’ makes pointer from integer without a cast
dbdimp.c:2531: error: ‘imp_dbh_t’ has no member named ‘no_autocommit_cmd’
dbdimp.c:2531: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:2536: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2542: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2542: warning: initialization makes pointer from integer without a cast
dbdimp.c:2547: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2547: warning: passing argument 1 of ‘Perl_sv_2mortal’ makes pointer from integer without a cast
dbdimp.c:2549: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2551: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2551: error: ‘sql_type_info_t’ has no member named ‘net’
dbdimp.c:2551: error: request for member ‘fd’ in something not a structure or union
dbdimp.c:2554: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2554: warning: initialization makes pointer from integer without a cast
dbdimp.c:2561: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:2561: warning: initialization makes pointer from integer without a cast
dbdimp.c:2566: error: ‘imp_dbh_t’ has no member named ‘use_server_side_prepare’
dbdimp.c:2571: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c: In function ‘mysql_st_prepare’:
dbdimp.c:2621: error: ‘MYSQL_VERSION_ID’ undeclared (first use in this function)
dbdimp.c:2649: error: ‘imp_sth_t’ has no member named ‘done_desc’
dbdimp.c:2650: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:2651: error: ‘imp_sth_t’ has no member named ‘currow’
dbdimp.c:2655: error: ‘imp_sth_t’ has no member named ‘use_mysql_use_result’
dbdimp.c:2656: error: ‘imp_dbh_t’ has no member named ‘use_mysql_use_result’
dbdimp.c:2656: warning: pointer/integer type mismatch in conditional expression
dbdimp.c:2659: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:2659: error: assignment of read-only location
dbdimp.c:2846: error: ‘imp_dbh_t’ has no member named ‘bind_comment_placeholders’
dbdimp.c:2850: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c: In function ‘mysql_st_free_result_sets’:
dbdimp.c:2921: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:2923: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:2924: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c: At top level:
dbdimp.c:3089: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘mysql_st_internal_execute’
dbdimp.c: In function ‘mysql_st_execute’:
dbdimp.c:3431: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:3443: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:3443: error: used struct type value where scalar is required
dbdimp.c:3444: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:3444: error: incompatible types in initialization
dbdimp.c:3446: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:3446: error: assignment of read-only location
dbdimp.c:3472: error: ‘imp_sth_t’ has no member named ‘row_num’
dbdimp.c:3477: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:3478: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3479: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3480: error: ‘imp_sth_t’ has no member named ‘use_mysql_use_result’
dbdimp.c:3483: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c: At top level:
dbdimp.c:3490: error: expected identifier or ‘(’ before ‘if’
dbdimp.c:3510: error: expected ‘=’, ‘,’, ‘;’, ‘asm’ or ‘__attribute__’ before ‘->’ token
dbdimp.c:3512: error: expected identifier or ‘(’ before ‘if’
dbdimp.c:3524: error: expected identifier or ‘(’ before ‘return’
dbdimp.c:3525: error: expected identifier or ‘(’ before ‘}’ token
dbdimp.c: In function ‘mysql_describe’:
dbdimp.c:3648: error: ‘imp_sth_t’ has no member named ‘done_desc’
dbdimp.c: In function ‘mysql_st_fetch’:
dbdimp.c:3678: error: ‘MYSQL_ROW’ undeclared (first use in this function)
dbdimp.c:3678: error: expected ‘;’ before ‘cols’
dbdimp.c:3680: error: ‘MYSQL’ undeclared (first use in this function)
dbdimp.c:3680: error: ‘svsock’ undeclared (first use in this function)
dbdimp.c:3680: error: invalid operands to binary *
dbdimp.c:3680: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3686: error: ‘MYSQL_FIELD’ undeclared (first use in this function)
dbdimp.c:3686: error: ‘fields’ undeclared (first use in this function)
dbdimp.c:3686: error: invalid operands to binary *
dbdimp.c:3691: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:3692: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3692: error: too many arguments to function ‘mysql_db_async_result’
dbdimp.c:3732: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3739: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3739: error: ‘sql_type_info_t’ has no member named ‘net’
dbdimp.c:3739: error: request for member ‘last_errno’ in something not a structure or union
dbdimp.c:3889: error: ‘imp_sth_t’ has no member named ‘currow’
dbdimp.c:3889: error: invalid lvalue in increment
dbdimp.c:3894: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3896: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3898: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3900: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3902: error: ‘imp_sth_t’ has no member named ‘currow’
dbdimp.c:3905: error: ‘cols’ undeclared (first use in this function)
dbdimp.c:3905: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3911: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3912: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3913: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:3914: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c:3924: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3925: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3926: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:3926: warning: assignment makes pointer from integer without a cast
dbdimp.c:3966: error: incompatible types in initialization
dbdimp.c: In function ‘mysql_st_finish’:
dbdimp.c:4047: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:4048: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4048: error: too many arguments to function ‘mysql_db_async_result’
dbdimp.c: In function ‘mysql_st_destroy’:
dbdimp.c:4163: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:4165: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:4165: warning: passing argument 1 of ‘free_param’ from incompatible pointer type
dbdimp.c:4166: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:4172: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4172: error: used struct type value where scalar is required
dbdimp.c:4173: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4173: error: incompatible types in initialization
dbdimp.c:4174: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4174: error: assignment of read-only location
dbdimp.c: In function ‘mysql_st_STORE_attrib’:
dbdimp.c:4219: error: ‘imp_sth_t’ has no member named ‘use_mysql_use_result’
dbdimp.c: At top level:
dbdimp.c:4267: error: expected declaration specifiers or ‘...’ before ‘MYSQL_RES’
dbdimp.c: In function ‘mysql_st_FETCH_internal’:
dbdimp.c:4274: error: ‘MYSQL_FIELD’ undeclared (first use in this function)
dbdimp.c:4274: error: ‘curField’ undeclared (first use in this function)
dbdimp.c:4274: error: invalid operands to binary *
dbdimp.c:4281: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4281: error: invalid operands to binary &&
dbdimp.c:4282: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4282: error: incompatible types in assignment
dbdimp.c:4285: error: ‘res’ undeclared (first use in this function)
dbdimp.c:4299: error: ‘sql_type_info_t’ has no member named ‘name’
dbdimp.c:4299: error: ‘sql_type_info_t’ has no member named ‘name’
dbdimp.c:4299: warning: passing argument 1 of ‘strlen’ from incompatible pointer type
dbdimp.c:4299: warning: passing argument 1 of ‘Perl_newSVpv’ from incompatible pointer type
dbdimp.c:4303: error: ‘sql_type_info_t’ has no member named ‘table’
dbdimp.c:4303: error: ‘sql_type_info_t’ has no member named ‘table’
dbdimp.c:4303: warning: passing argument 1 of ‘strlen’ from incompatible pointer type
dbdimp.c:4303: warning: passing argument 1 of ‘Perl_newSVpv’ from incompatible pointer type
dbdimp.c:4307: error: ‘sql_type_info_t’ has no member named ‘type’
dbdimp.c:4311: error: ‘sql_type_info_t’ has no member named ‘type’
dbdimp.c:4311: warning: passing argument 1 of ‘native2sql’ makes integer from pointer without a cast
dbdimp.c:4314: error: ‘sql_type_info_t’ has no member named ‘flags’
dbdimp.c:4318: error: ‘sql_type_info_t’ has no member named ‘flags’
dbdimp.c:4322: error: ‘sql_type_info_t’ has no member named ‘flags’
dbdimp.c:4326: error: ‘sql_type_info_t’ has no member named ‘length’
dbdimp.c:4330: error: ‘sql_type_info_t’ has no member named ‘type’
dbdimp.c:4330: warning: passing argument 1 of ‘native2sql’ makes integer from pointer without a cast
dbdimp.c:4334: error: ‘sql_type_info_t’ has no member named ‘type’
dbdimp.c:4334: warning: passing argument 1 of ‘native2sql’ makes integer from pointer without a cast
dbdimp.c:4338: error: ‘sql_type_info_t’ has no member named ‘max_length’
dbdimp.c:4350: error: ‘sql_type_info_t’ has no member named ‘flags’
dbdimp.c:4350: error: ‘PRI_KEY_FLAG’ undeclared (first use in this function)
dbdimp.c:4350: error: ‘UNIQUE_KEY_FLAG’ undeclared (first use in this function)
dbdimp.c:4350: error: invalid operands to binary |
dbdimp.c:4350: error: ‘MULTIPLE_KEY_FLAG’ undeclared (first use in this function)
dbdimp.c:4350: error: invalid operands to binary |
dbdimp.c:4350: error: invalid operands to binary &
dbdimp.c:4354: error: ‘sql_type_info_t’ has no member named ‘flags’
dbdimp.c:4358: error: ‘sql_type_info_t’ has no member named ‘decimals’
dbdimp.c:4362: error: ‘sql_type_info_t’ has no member named ‘length’
dbdimp.c:4362: error: ‘sql_type_info_t’ has no member named ‘max_length’
dbdimp.c:4362: error: ‘sql_type_info_t’ has no member named ‘length’
dbdimp.c:4362: error: ‘sql_type_info_t’ has no member named ‘max_length’
dbdimp.c:4362: warning: passing argument 1 of ‘Perl_newSViv’ makes integer from pointer without a cast
dbdimp.c:4377: error: ‘imp_sth_t’ has no member named ‘av_attr’
dbdimp.c:4377: error: assignment of read-only location
dbdimp.c: In function ‘mysql_st_FETCH_attrib’:
dbdimp.c:4429: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4429: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4429: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4431: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4431: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4431: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4435: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4435: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4435: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4447: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:4447: error: ‘struct sql_type_info_s’ has no member named ‘value’
dbdimp.c:4447: warning: passing argument 1 of ‘Perl_newSVsv’ from incompatible pointer type
dbdimp.c:4456: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4456: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4456: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4460: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4460: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4460: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4466: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4466: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4466: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4470: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4470: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4470: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4474: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4474: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4474: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4476: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4476: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4476: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4478: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4478: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4478: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4480: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4484: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4484: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4484: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4491: error: ‘imp_sth_t’ has no member named ‘insertid’
dbdimp.c:4493: error: ‘imp_sth_t’ has no member named ‘insertid’
dbdimp.c:4493: warning: passing argument 1 of ‘Perl_sv_2mortal’ makes pointer from integer without a cast
dbdimp.c:4498: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4498: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4498: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4502: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4502: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4502: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4504: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4504: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4504: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c:4506: error: ‘imp_sth_t’ has no member named ‘use_mysql_use_result’
dbdimp.c:4510: error: ‘imp_sth_t’ has no member named ‘warning_count’
dbdimp.c:4522: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:4522: warning: passing argument 3 of ‘mysql_st_FETCH_internal’ makes integer from pointer without a cast
dbdimp.c:4522: error: too many arguments to function ‘mysql_st_FETCH_internal’
dbdimp.c: In function ‘mysql_bind_ph’:
dbdimp.c:4612: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:4655: error: ‘imp_sth_t’ has no member named ‘params’
dbdimp.c:4655: warning: passing argument 1 of ‘bind_param’ from incompatible pointer type
dbdimp.c: In function ‘mysql_db_reconnect’:
dbdimp.c:4801: error: ‘MYSQL’ undeclared (first use in this function)
dbdimp.c:4801: error: expected ‘;’ before ‘save_socket’
dbdimp.c:4811: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4811: error: ‘CR_SERVER_GONE_ERROR’ undeclared (first use in this function)
dbdimp.c:4811: warning: comparison between pointer and integer
dbdimp.c:4815: error: ‘imp_dbh_t’ has no member named ‘auto_reconnect’
dbdimp.c:4829: error: ‘save_socket’ undeclared (first use in this function)
dbdimp.c:4829: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4830: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4830: warning: passing argument 3 of ‘__builtin___memcpy_chk’ makes integer from pointer without a cast
dbdimp.c:4830: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4830: warning: passing argument 3 of ‘__memcpy_ichk’ makes integer from pointer without a cast
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: warning: passing argument 1 of ‘__builtin___memset_chk’ discards qualifiers from pointer target type
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4831: warning: passing argument 1 of ‘__memset_ichk’ discards qualifiers from pointer target type
dbdimp.c:4839: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4839: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4840: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c:4841: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4841: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4841: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4841: warning: passing argument 1 of ‘__builtin___memcpy_chk’ discards qualifiers from pointer target type
dbdimp.c:4841: warning: passing argument 3 of ‘__builtin___memcpy_chk’ makes integer from pointer without a cast
dbdimp.c:4841: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:4841: warning: passing argument 1 of ‘__memcpy_ichk’ discards qualifiers from pointer target type
dbdimp.c:4841: warning: passing argument 3 of ‘__memcpy_ichk’ makes integer from pointer without a cast
dbdimp.c:4842: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:4842: error: request for member ‘auto_reconnects_failed’ in something not a structure or union
dbdimp.c:4842: error: invalid lvalue in increment
dbdimp.c:4851: error: ‘imp_dbh_t’ has no member named ‘stats’
dbdimp.c:4851: error: request for member ‘auto_reconnects_ok’ in something not a structure or union
dbdimp.c:4851: error: invalid lvalue in increment
dbdimp.c: In function ‘mysql_db_quote’:
dbdimp.c:5013: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c: In function ‘mysql_db_last_insert_id’:
dbdimp.c:5038: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5039: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:5039: warning: passing argument 1 of ‘Perl_sv_2mortal’ makes pointer from integer without a cast
dbdimp.c: At top level:
dbdimp.c:5044: error: expected declaration specifiers or ‘...’ before ‘MYSQL_RES’
dbdimp.c: In function ‘mysql_db_async_result’:
dbdimp.c:5049: error: ‘MYSQL’ undeclared (first use in this function)
dbdimp.c:5049: error: ‘svsock’ undeclared (first use in this function)
dbdimp.c:5049: error: invalid operands to binary *
dbdimp.c:5050: error: ‘MYSQL_RES’ undeclared (first use in this function)
dbdimp.c:5050: error: ‘_res’ undeclared (first use in this function)
dbdimp.c:5050: error: invalid operands to binary *
dbdimp.c:5054: error: ‘resp’ undeclared (first use in this function)
dbdimp.c:5069: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5073: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5073: warning: comparison of distinct pointer types lacks a cast
dbdimp.c:5077: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5079: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:5082: error: assignment of read-only location
dbdimp.c:5085: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c:5086: error: wrong type argument to unary exclamation mark
dbdimp.c:5090: warning: comparison of distinct pointer types lacks a cast
dbdimp.c:5098: error: ‘my_ulonglong’ undeclared (first use in this function)
dbdimp.c:5098: warning: comparison between pointer and integer
dbdimp.c:5099: error: wrong type argument to unary exclamation mark
dbdimp.c:5100: error: ‘imp_sth_t’ has no member named ‘insertid’
dbdimp.c:5106: error: ‘imp_sth_t’ has no member named ‘result’
dbdimp.c:5107: error: ‘imp_sth_t’ has no member named ‘done_desc’
dbdimp.c:5108: error: ‘imp_sth_t’ has no member named ‘fetch_done’
dbdimp.c:5111: error: ‘imp_sth_t’ has no member named ‘warning_count’
dbdimp.c:5115: warning: passing argument 3 of ‘mysql_dr_error’ makes pointer from integer without a cast
dbdimp.c: In function ‘mysql_db_async_ready’:
dbdimp.c:5139: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5140: error: ‘imp_dbh_t’ has no member named ‘async_query_in_flight’
dbdimp.c:5140: warning: comparison of distinct pointer types lacks a cast
dbdimp.c:5144: error: ‘imp_dbh_t’ has no member named ‘pmysql’
dbdimp.c:5144: error: ‘sql_type_info_t’ has no member named ‘net’
dbdimp.c:5144: error: request for member ‘fd’ in something not a structure or union
dbdimp.c:5144: warning: assignment makes integer from pointer without a cast
make: *** [dbdimp.o] Error 1
```


# mysql-devel-xxx与 mysql-devel-community-xxx
区别：    
  前者为redhat加工过mysql，     
  而后者为oracle没有针对特定os（CentOS）加工的mysql。

  也就是说一个是优化过的，而另一个是没有优化的。
