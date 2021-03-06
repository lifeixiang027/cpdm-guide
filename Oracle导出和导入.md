## Oracle导出和导入
### 导出Oracle数据
1. 创建文件夹，如D:\dump
2. 执行以下命令创建Oracle目录
```
sqlplus /nolog
conn /as sysdba
create directory dump as 'D:/dump';
grant read,write on directory dump to cplm;
exit
```
3. 执行以下命令导出Oracle数据
```
SET NLS_LANG=AMERICAN_AMERICA.AL32UTF8
expdp cplm/cplm schemas=cplm dumpfile=cplm.dmp directory=dump logfile=cplm_expdp.log
```

### 导入Oracle数据
1. 创建文件夹，如D:\dump，并将导出的dump文件复制到此文件夹
2. 执行以下命令创建Oracle用户
```
sqlplus /nolog
conn /as sysdba
create user cplm identified by cplm temporary tablespace TEMP default tablespace USERS;
grant connect, resource to cplm;
grant create sequence, create view, query rewrite, unlimited tablespace, select any dictionary to cplm;
exit
```
3. 执行以下命令创建Oracle目录
```
sqlplus /nolog
conn /as sysdba
create directory dump as 'D:/dump';
grant read,write on directory dump to cplm;
exit
```
4. 执行以下命令导入Oracle数据
```
SET NLS_LANG=AMERICAN_AMERICA.AL32UTF8
impdp cplm/cplm schemas=cplm dumpfile=cplm.dmp directory=dump logfile=cplm_impdp.log
```
