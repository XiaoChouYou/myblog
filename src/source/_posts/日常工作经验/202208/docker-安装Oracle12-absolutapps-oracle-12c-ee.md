---
title: docker 安装Oracle12 (absolutapps/oracle-12c-ee )
date: 2022-08-12 15:15:31
categories:
  - [日常工作经验记录, 2022,08]
tags:
  - docker
  - oracle
---
<!-- more -->
[docker hub](https://hub.docker.com/r/absolutapps/oracle-12c-ee)
#  下载
``` bash
docker pull absolutapps/oracle-12c-ee
```

<!-- more -->
# 启动（自行修改端口即可）

## 创建一个文件夹来挂载数据
``` bash
mkdir -p /data/app/oracle 
cd /data/app/oracle 
```
## 启动
```shell
docker run -d --name oracle \
  --privileged -v $(pwd)/oradata:/u01/app/oracle \
  -p 8080:8080 -p 1521:1521 absolutapps/oracle-12c-ee 
```
## 查看启动日志
```shell

(base) [root@lqz-test ~]# docker logs -f 7d84ab48f97d
ls: cannot access /u01/app/oracle/oradata/orcl: No such file or directory
No databases found in /u01/app/oracle/oradata/orcl. About to create a new database instance
Starting database listener

LSNRCTL for Linux: Version 12.1.0.2.0 - Production on 12-AUG-2022 01:01:38

Copyright (c) 1991, 2014, Oracle.  All rights reserved.

Starting /u01/app/oracle/product/12.1.0.2/dbhome_1/bin/tnslsnr: please wait...

TNSLSNR for Linux: Version 12.1.0.2.0 - Production
System parameter file is /u01/app/oracle/product/12.1.0.2/dbhome_1/network/admin/listener.ora
Log messages written to /u01/app/oracle/diag/tnslsnr/7d84ab48f97d/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=7d84ab48f97d)(PORT=1521)))

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=7d84ab48f97d)(PORT=1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
Start Date                12-AUG-2022 01:01:38
Uptime                    0 days 0 hr. 0 min. 0 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/12.1.0.2/dbhome_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/7d84ab48f97d/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=7d84ab48f97d)(PORT=1521)))
The listener supports no services
The command completed successfully
Copying database files
1% complete
3% complete
11% complete
18% complete
26% complete
37% complete
Creating and starting Oracle instance
40% complete
45% complete
50% complete
55% complete
56% complete
60% complete
62% complete
Completing Database Creation
66% complete
70% complete
73% complete
85% complete
96% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/orcl/orcl.log" for further details.

LSNRCTL for Linux: Version 12.1.0.2.0 - Production on 12-AUG-2022 01:05:54

Copyright (c) 1991, 2014, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=7d84ab48f97d)(PORT=1521)))
The command completed successfully
Database has been created in /u01/app/oracle/oradata/orcl
SYS and SYSTEM passwords are set to [oracle]
Setting HTTP port to 8080

PL/SQL procedure successfully completed.

Please login to http://<ip_address>:8080/em to use enterprise manager
User: sys; Password oracle; Sysdba: true
Fixing permissions...
Running init scripts...
Init scripts in /oracle.init.d/: Ignoring /oracle.init.d/*

Done with scripts we are ready to go

```

## 进入刚刚创建的container中,并登录oracle库
```shell
docker exec -it 7d84ab48f97d /bin/sh
sh-4.2# su oracle
[oracle@7d84ab48f97d /]$ $ORACLE_HOME/bin/sqlplus / as sysdba

SQL*Plus: Release 12.1.0.2.0 Production on Fri Aug 12 07:25:10 2022

Copyright (c) 1982, 2014, Oracle.  All rights reserved.


Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL> conn system/oracle as sysdba;
Connected.
SQL>  

```
## 修改密码：
``` sql
alter user system identified by oracle;
alter user sys identified by sys;
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```
## 创建用户
```sql
create user test identified by test;
```
并给用户赋予权限
```sql
grant connect,resource,dba to test; 
```


操作如下
```shell
[root@iz2zej4i2jdf3i8bhr61xsz ~]# docker exec -it 6fea41c62a88 /bin/bash
[root@6fea41c62a88 /]# su oracle
[oracle@6fea41c62a88 /]$ $ORACLE_HOME/bin/sqlplus / as sysdba
SQL*Plus: Release 12.1.0.2.0 Production on Sun Jan 19 05:20:04 2020
Copyright (c) 1982, 2014, Oracle.  All rights reserved.
Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing options
SQL> conn system/oracle as sysdba;
Connected.
SQL>    alter user system identified by oracle;
User altered.
SQL>  alter user sys identified by sys;
User altered.
SQL> ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
Profile altered.
SQL> create user test identified by test;
User created.
SQL> grant connect,resource,dba to test;
Grant succeeded.
```

# 然后使用Navicat链接

![222](1660291415208.jpg)