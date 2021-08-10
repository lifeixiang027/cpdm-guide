# 概述
Liquibase是一种开源数据库架构变更管理解决方案，可让您轻松管理数据库变更的修订。Liquibase使参与应用程序发布过程的任何人都可以轻松地：
*   消除发布数据库时的错误和延迟。
*   部署和回滚特定版本的更改，而无需知道已部署的内容。
*   一起部署数据库和应用程序更改，以便它们始终保持同步。
# 使用
## 引入依赖
在pom.xml中引入Liquibase依赖，由于Liquibase已经在spring依赖BOM中，所以不需要指定版本号。
```xml
<dependency>
  <groupId>org.liquibase</groupId>
  <artifactId>liquibase-core</artifactId>
</dependency>
```
## 修改配置
在application.yml中添加以下配置项
> 经过实际应用，发现xml用起来比较方便，所以本文档采用xml的方式进行更改的配置管理。
```yml
spring:
  liquibase:
    change-log: classpath:db/db.changeLog.xml
    enabled: true
```
 ## 创建变更文件
根据上面的配置,在resource下面创建db文件夹并创建文件db.changeLog.xml,填充以下内容
```xml
<?xml version="1.1" encoding="UTF-8" standalone="no"?>
<databaseChangeLog
  xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:pro="http://www.liquibase.org/xml/ns/pro"
  xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
  http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.8.xsd
  http://www.liquibase.org/xml/ns/pro
  http://www.liquibase.org/xml/ns/pro/liquibase-pro-3.8.xsd">

  <changeSet id="Requirement_20210610_001" author="GuoxiaoYong">
    <preConditions onFail="MARK_RAN">
      <columnExists tableName="c_requirement_relation_Link" columnName="VERSIONS"/>
      <columnExists tableName="c_requirement_document" columnName="VERSIONS"/>
    </preConditions>
    <renameColumn tableName="c_requirement_relation_Link" oldColumnName="VERSIONS"
      newColumnName="DOC_VERSION" columnDataType="varchar"/>
    <renameColumn tableName="c_requirement_document" oldColumnName="VERSIONS"
      newColumnName="DOC_VERSION" columnDataType="varchar"/>
  </changeSet>

</databaseChangeLog>
```
1、 数据更改记录文件由databaseChangeLog作为根标签
2、 databaseChangeLog下面可以存在多个changeSet标签
3、 每个changeSet标签下可以有多个更改
**规范**
1.changeSet的id属性格式为： 模块名 + 日期 +序列号，例如
  > Requirement_20210610_001
2. changeSet 必须填写author属性
3. 每一个changeSet编写完成之后不可更改，如果有新的更改，再添加一个changeSet

# 参考文档
> 以下只列出常用链接，更多高级功能自行探索

[先决条件](https://docs.liquibase.com/concepts/advanced/preconditions.html)
[更改类型](https://docs.liquibase.com/change-types/community/home.html)