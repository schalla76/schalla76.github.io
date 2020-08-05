---
layout: post
title: "SQL Queries"
date: 2019-02-01
categories: sql spf
---

- [Utility Queries](#utility-queries)
  - [Setup new database and user](#setup-new-database-and-user)
  - [Search for tables and columns](#search-for-tables-and-columns)
  - [Re-index tables](#re-index-tables)
  - [Find row count in all tables](#find-row-count-in-all-tables)
- [SPF Queries](#spf-queries)
  - [Check the documents count](#check-the-documents-count)
  - [Clean up terminated records in SPF](#clean-up-terminated-records-in-spf)
  - [Find VTL Tables related to Jobs](#find-vtl-tables-related-to-jobs)
  - [Query Classification Tree](#query-classification-tree)
  - [Query EnumListTypes Tree](#query-enumlisttypes-tree)
  - [Query Classifications with Enums](#query-classifications-with-enums)
  - [Query Enums numbers](#query-enums-numbers)
  - [Query Import Definition Order Values](#query-import-definition-order-values)
  - [Query the missing files in the vault](#query-the-missing-files-in-the-vault)
  - [Password Reset](#password-reset)
  - [Find documents without security code](#find-documents-without-security-code)
  - [Find interfaces for a object by name](#find-interfaces-for-a-object-by-name)

## Utility Queries

### Setup new database and user

For **Oracle**

```sql
--Create tablespace
create tablespace username_data datafile '<oradatapath>\dbname\username_data.dbf' size 50M;
alter database datafile '<oradatapath>\dbname\username_data.dbf' autoextend on next 5M;

--Create tempaory tablespace
create temporary tablespace username_dataTemp tempfile '<oradatapath>\dbname\username_dataTemp.dbf' size 50M;
alter database tempfile '<oradatapath>\dbname\username_dataTemp.dbf' autoextend on next 5M;

--Drop existing user
DROP USER username_data CASCADE;

GRANT connect TO username_data identified BY oracle;
GRANT resource TO username_data;
ALTER USER username_data quota unlimited ON username_data;
ALTER USER username_data DEFAULT tablespace username_data;
ALTER USER username_data temporary tablespace username_dataTemp;
--GRANT dba TO username_data;
GRANT CONNECT, RESOURCE TO username_data;
GRANT unlimited tablespace TO username_data WITH admin OPTION;

COMMIT;
```

For **MsSql**

```sql
---Create database
CREATE DATABASE usernamedata

ALTER DATABASE usernamedata MODIFY FILE
   (NAME = 'usernamedata', SIZE = 400MB, FILEGROWTH = 50MB)

GO

---Create User
USE [master]
GO

CREATE LOGIN username_data WITH PASSWORD=N'oracle', DEFAULT_DATABASE=usernamedata, CHECK_POLICY=OFF
GO
```

### Search for tables and columns

For **Oracle**

```sql
--SEARCH TABLE NAMES

SELECT t.owner, t.table_name
  FROM ALL_TABLES t
 WHERE t.owner LIKE 'SRSADMIN%'
   AND UPPER(t.table_name) LIKE 'EF%'
 GROUP BY t.owner, t.table_name

--SEARCH COLUMN NAMES
SELECT t.owner, t.table_name, c.column_name
  FROM ALL_TABLES t
  join ALL_TAB_COLUMNS c ON t.table_name = c.table_name
   AND UPPER(t.owner) LIKE 'SRSADMIN%'
   AND UPPER(c.column_name) LIKE '%METHOD%'
 GROUP BY t.owner, t.table_name, c.column_name
```

For **MsSql**

```sql
--SEARCH TABLES
SELECT USER_NAME(SO.UID)
      ,SO.NAME TABLE_NAME
    ,SO.XTYPE TYPE
  FROM SYSOBJECTS SO
 WHERE UPPER(SO.NAME) LIKE '%cmpnt_func_type_name%'
   AND SO.XTYPE IN ('U','V')
GROUP BY
       USER_NAME(SO.UID)
      ,SO.NAME
    ,SO.XTYPE

--SEARCH COLUMNS
SELECT USER_NAME(SO.UID)
      ,SO.NAME TABLE_NAME
    ,SC.NAME COLUMN_NAME
    ,SO.XTYPE TYPE
  FROM SYSOBJECTS SO
  JOIN SYSCOLUMNS SC
    ON SO.ID = SC.ID
   AND UPPER(SC.NAME) LIKE '%PD_UDF_C04%'
GROUP BY USER_NAME(SO.UID)
      ,SO.NAME
    ,SC.NAME
    ,SO.XTYPE
```

### Re-index tables

For **MsSql**

```sql
EXEC sp_MSforeachtable @command1="print '?' DBCC DBREINDEX ('?', ' ', 80)"
GO
EXEC sp_updatestats
GO
```

### Find row count in all tables

For **MsSql**

```sql
SELECT SCHEMA_NAME(schema_id)   AS [SchemaName],
       [Tables].name            AS [TableName],
       SUM([Partitions].[rows]) AS [TotalRowCount]
  FROM sys.tables          AS [Tables] JOIN sys.partitions AS [Partitions]
    ON [Tables].[object_id] = [Partitions].[object_id]
       AND [Partitions].index_id IN ( 0, 1 )
 --WHERE [Tables].name = N'name of the table'
 GROUP BY SCHEMA_NAME(schema_id),
       [Tables].name
 ORDER BY TotalRowCount;
```

## SPF Queries

### Check the documents count

```sql
SELECT config   AS config,
       count(1) AS docs
  FROM DOCOBJ
 WHERE OBJDEFUID = 'FDWDocumentMaster'
 GROUP BY config;
```

### Clean up terminated records in SPF

For **MsSql**

```sql
 IF EXISTS(SELECT name FROM sysobjects WHERE id = OBJECT_ID('dbo.sp_delete_all_terminated_Objects'))
BEGIN
  DROP PROCEDURE dbo.sp_delete_all_terminated_Objects
END

GO

CREATE PROCEDURE dbo.sp_delete_all_terminated_Objects
AS
BEGIN

  DECLARE @id INT
  DECLARE @count VARCHAR(256)
  DECLARE @rowcount TABLE (Value int);
  DECLARE @SELECTSQL VARCHAR(2000)
  DECLARE @SELECTSQL2 VARCHAR(2000)
  DECLARE @IFEXISTSSQL VARCHAR(3000)
  DECLARE @tablename VARCHAR(256)
  DECLARE @schemaname VARCHAR(256)


  /*
  dbo.sp_delete_all_terminated_Objects
    */

  CREATE TABLE #vtlTempTables(
         dropSql NCHAR(2000)
        )

  CREATE TABLE #tableswithterminationDate(
         tablename NCHAR(256)
        ,numberofrows NCHAR(256)
        ,schemaname NCHAR(256)
        ,id INT identity(1,1)
                ,found BIT
                ,selectsql VARCHAR(2000)
        )

  PRINT 'Before running this proc, close all application which use this database'


  INSERT INTO #vtlTempTables(
           dropSql
      )
    SELECT 'DROP TABLE ' + RTRIM(user_name(so.uid)) + '.' +  so.name + ';'
    FROM sysobjects so
   WHERE so.xtype in ('U', 'V')
     AND so.name like 'VTL%'
     AND SO.NAME NOT IN ('VTLOBJ', 'VTLOBJIF', 'VTLOBJPR', 'VTLOBJPRDETAIL', 'VTLREL')
  GROUP BY so.xtype
       , so.name
         , user_name(so.uid)


  select * from #vtlTempTables;

  INSERT INTO #tableswithterminationDate(
           tablename
          ,schemaname)
    SELECT so.name
     , user_name(so.uid) as tableschema
    FROM sysobjects so join syscolumns sc on so.id = sc.id
     AND sc.name = 'TERMINATIONDATE'
     AND so.xtype in ('U')
     AND type_name(sc.xtype) in ('nchar', 'char', 'nvarchar', 'varchar')
  GROUP BY  sc.name
         , so.xtype
       , so.name
         , user_name(so.uid)
  ORDER BY tableschema desc  ;



  SELECT @id =MAX(id) FROM #tableswithterminationDate

  WHILE (@id is NOT NULL)
  BEGIN

    SELECT @tablename = tablename
             , @schemaname = schemaname
          FROM #tableswithterminationDate
         WHERE id = @id

    SELECT @SELECTSQL = 'SELECT count(1) FROM ' +  RTRIM(@schemaname) + '.' + RTRIM(@tablename) + ' WHERE TERMINATIONDATE <> ''9999/12/31-23:59:59:999''';

    SELECT @SELECTSQL2 = 'DELETE FROM ' +  RTRIM(@schemaname) + '.' + RTRIM(@tablename) + ' WHERE TERMINATIONDATE <> ''''9999/12/31-23:59:59:999'''';';

    print(@SELECTSQL)

    INSERT INTO @rowcount EXEC(@SELECTSQL)

    SELECT @count = Value FROM @rowcount;

    print(@count)

    SELECT @IFEXISTSSQL = 'IF ( ' + @count + ' > 0 )' +
              ' BEGIN' +
                ' UPDATE #tableswithterminationDate' +
                   ' SET found = 1' +
                   '   , selectsql = ''' + @SELECTSQL2 + '''' +
                   ' , numberofrows = ' + @count +
                 ' WHERE id = ' + CONVERT(VARCHAR(16), @id)  +
              ' END'


    EXEC(@IFEXISTSSQL)

    SELECT @id=MAX(id) FROM #tableswithterminationDate WHERE id < @id

    END

    SELECT *
      FROM #tableswithterminationDate
     WHERE found = 1

  DROP table #tableswithterminationDate
  DROP table #vtlTempTables
END
```

### Find VTL Tables related to Jobs

For **MsSql**

```sql
SELECT USER_NAME(SO.UID)
      ,SO.NAME TABLE_NAME
    ,SO.XTYPE TYPE
    ,SCO.OBJNAME
    ,SCO.OBJDEFUID
  FROM SYSOBJECTS SO
--JOIN DATAOBJ SCO
  JOIN SCLBOBJ SCO
    on SO.name like '%' + SCO.OBID + '%'
   AND SO.XTYPE IN ('U','V')
   and sco.OBJDEFUID in ('SCLBSubmittal', 'SDALoader')
 --and SCO.OBJNAME = 'SUB_INC-A056_0005'
GROUP BY
       USER_NAME(SO.UID)
      ,SO.NAME
    ,SO.XTYPE
    ,SCO.OBJNAME
    ,SCO.OBJDEFUID
```

### Query Classification Tree

For **MsSql**

```sql
WITH rightTable(lobjname, robjname, lobjuid, robjuid, reldef, lvl) AS (
SELECT leftdata.objname, rightdata.objname, leftdata.objuid, rightdata.objuid, dr.defuid, 1 as lvl
  FROM dataobj leftdata
  JOIN datarel dr
    ON leftdata.objuid = dr.uid1
   and dr.defuid = 'SPFClassMember'
  JOIN dataobj rightdata
    ON rightdata.objuid = dr.uid2
 WHERE leftdata.objdefuid in ('SDADocumentClassification')
   AND leftdata.objuid = 'SDC_Document_classifications'
 UNION ALL
SELECT lt.robjname, rt.objname, lt.robjuid, rt.objuid, dr.defuid, lvl + 1 AS lvl
  FROM rightTable lt
  JOIN datarel dr
    ON lt.robjuid = dr.uid1
  JOIN dataobj rt
    ON rt.objuid = dr.uid2
   and dr.defuid = 'SPFClassMember'
  )
SELECT * FROM rightTable;
```

### Query EnumListTypes Tree

For **MsSql**

```sql
WITH rightTable(lobjname, robjname, lobjuid, robjuid, lobjdefuid, robjdefuid, reldef, lvl) AS (
SELECT leftdata.objname, rightdata.objname, leftdata.objuid, rightdata.objuid, leftdata.OBJDEFUID, rightdata.OBJDEFUID, dr.defuid, 1 as lvl
  FROM schemaobj leftdata
  JOIN schemarel dr
    ON leftdata.objuid = dr.uid1
   and dr.defuid like '%Contains%'
   and leftdata.TERMINATIONDATE = '9999/12/31-23:59:59:999'
   and dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  JOIN schemaobj rightdata
    ON rightdata.objuid = dr.uid2
   AND rightdata.TERMINATIONDATE = '9999/12/31-23:59:59:999'
 WHERE leftdata.objdefuid in ('EnumListType', 'EnumEnum')
   AND leftdata.objuid = 'e1SDADocCategories'
UNION ALL
SELECT lt.robjname, rt.objname, lt.robjuid, rt.objuid, lt.robjdefuid, rt.OBJDEFUID, dr.defuid, lvl + 1 AS lvl
  FROM rightTable lt
  JOIN schemarel dr
    ON lt.robjuid = dr.uid1
   AND dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  JOIN schemaobj rt
    ON rt.objuid = dr.uid2
   AND rt.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  )
SELECT * FROM rightTable;
```

### Query Classifications with Enums

For **MsSql**

```sql
WITH rightTable(lobjname, robjname, lobjuid, robjuid, reldef, lvl) AS (
SELECT leftdata.objname, rightdata.objname, leftdata.objuid, rightdata.objuid, dr.defuid, 1 as lvl
  FROM dataobj leftdata
  JOIN datarel dr
    ON leftdata.objuid = dr.uid1
   and dr.defuid = 'SPFClassMember'
   and leftdata.TERMINATIONDATE = '9999/12/31-23:59:59:999'
   and dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  JOIN dataobj rightdata
    ON rightdata.objuid = dr.uid2
   and rightdata.TERMINATIONDATE = '9999/12/31-23:59:59:999'
 WHERE leftdata.objdefuid in ('SDADocumentClassification')
   AND leftdata.objuid = 'SDC_Document_classifications'
 UNION ALL
SELECT lt.robjname, rt.objname, lt.robjuid, rt.objuid, dr.defuid, lvl + 1 AS lvl
  FROM rightTable lt
  JOIN datarel dr
    ON lt.robjuid = dr.uid1
   AND dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  JOIN dataobj rt
    ON rt.objuid = dr.uid2
   and dr.defuid = 'SPFClassMember'
   AND rt.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  )
SELECT lobjname, robjname, lobjuid, robjuid, reldef, lvl, dr.uid2 as EnumParent, dr2.uid2 EnumChild
  FROM rightTable
  Left join datarel dr
    ON rightTable.lobjuid = dr.uid1
   AND dr.defuid = 'SPFObjClassEnumEnum'
   AND dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
  Left join datarel dr2
    ON rightTable.robjuid = dr2.uid1
   AND dr2.defuid = 'SPFObjClassEnumEnum'
   AND dr2.TERMINATIONDATE = '9999/12/31-23:59:59:999'
 order by lvl, lobjname, robjname;
```

### Query Enums numbers

For **MsSql**

```sql
SELECT so.OBJNAME, sp.STRVALUE, so.OBJUID
  FROM schemaobj so
  JOIN schemaobjpr sp
    ON so.OBID = sp.OBJOBID
   AND so.TERMINATIONDATE = '9999/12/31-23:59:59:999'
   AND sp.TERMINATIONDATE = '9999/12/31-23:59:59:999'
 WHERE sp.propertydefuid = 'EnumNumber'
 --AND so.OBJUID like '%FDW%'
 ORDER BY ABS(strvalue) DESC
```

### Query Import Definition Order Values

For **MsSql**

```sql
 select so2.OBJNAME, sp.STRVALUE, sr.TERMINATIONDATE
  from schemaobj so
  JOin SCHEMAREL sr
    on so.OBJUID = sr.UID1
   and sr.DEFUID = 'VTLImportDefHeader'
  join SCHEMAOBJ so2
    on sr.UID2 = so2.OBJUID
   --and so2.OBJNAME in ('COMP_PlantCodeValue', 'COMP_PlantCode')
   and so.OBJNAME = 'Document Create Mapping'
  JOIN SCHEMAOBJPR sp
    on sp.OBJOBID = sr.OBID
   and sp.PROPERTYDEFUID = 'OrderValue'
   order by ABS(sp.strvalue)
```

### Query the missing files in the vault

For **MsSql**

```sql
-- Function to find if file exists
CREATE or ALTER FUNCTION dbo.fn_FileExists(@path varchar(512))
RETURNS BIT
AS
BEGIN
     DECLARE @result INT
     EXEC master.dbo.xp_fileexist @path, @result OUTPUT
     RETURN cast(@result as bit)
END;
GO

-- Query all files with FilePath and FileExists flag
select * from (
  select fd.OBJNAME as FileName
       , concat(vp.STRVALUE, '\', fp.STRVALUE) as FilePath
       , dbo.fn_FileExists(concat(vp.STRVALUE, '\', fp.STRVALUE)) as FileExists
    from DATAOBJ fd
    join DATAREL fr
      on fd.OBJUID = fr.UID1
     and fr.DEFUID = 'SPFFileVault'
     --and upper(fd.OBJDEFUID) like '%FILE%'
     and fd.TERMINATIONDATE = '9999/12/31-23:59:59:999'
     and fr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
    join DATAOBJPR fp
      on fp.OBJOBID = fd.OBID
     and fp.PROPERTYDEFUID = 'SPFRemoteFileName'
     and fp.TERMINATIONDATE = '9999/12/31-23:59:59:999'
    join SCHEMAOBJ vd
      on vd.OBJUID = fr.uid2
     and vd.TERMINATIONDATE = '9999/12/31-23:59:59:999'
    join SCHEMAOBJPR vp
      on vp.OBJOBID = vd.OBID
     and vp.PROPERTYDEFUID = 'SPFLocalPath'
     and vp.TERMINATIONDATE = '9999/12/31-23:59:59:999') FileData
 where FileExists = 0;
```

- The above FileExists may not evaluate properly if the user running SQL service does not have access to the file path (example: for network files etc).
- The work around is:

  - Copy the query output to a excel(.xlsm) file.
  - Go to Excel VBA code editor (Alt+F12)
  - Insert a module and add below vba code to the module
  - Add a column in the Excel and use the formula =FileExists

```vb
Function FileExists(sPath As String) As Boolean
    On Error GoTo HandleError
    FileExists = Dir(sPath) <> ""
HandleError:
    FileExists = False
End Function
```

### Password Reset

select dd.OBJNAME, dd.CONFIG, dd.OBJDEFUID, dr.DEFUID
from DOCOBJ dd
LEFT JOIN DOCREL dr
on dr.UID1 = dd.OBJUID
and dr.DEFUID = 'SDAItemSecurityCode'
and dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
WHERE dd.OBJDEFUID = 'FDWDocumentMaster'
and dr.uid2 is null
and dd.TERMINATIONDATE = '9999/12/31-23:59:59:999'

[Source link](https://www.ateam-oracle.com/avoiding-and-resetting-expired-passwords-in-oracle-databases)

**Run below commands as 'sys' user connected with 'SYSDBA' role.**

- Reset the password expiration policy to unlimitted

```sql
-- Find the exist password policy
select *
  from dba_profiles
 where resource_name = 'PASSWORD_LIFE_TIME';

-- Reset password policy. Replace the profile name with actual profile name
ALTER PROFILE <profile name> LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

- Unlock the user accounts

```sql
select 'ALTER USER '|| USERNAME || ' account unlock;'
  from dba_users
 where ACCOUNT_STATUS like '%LOCKED%';
```

- Reset the user password to same old password

```sql
-- The oracle 11g onwards the password is stored in the spare4 column of sys.user$ table. Below the dba_users and user$ tables are joined using username=name

select 'ALTER USER '|| USERNAME || ' identified by values ''' || spare4 || ''';'
  from  dba_users,user$
 where ACCOUNT_STATUS like '%EXPIRED%' and USERNAME=NAME;
```

### Find documents without security code

**For MsSql**

```sql
select dd.OBJNAME, dd.CONFIG, dd.OBJDEFUID, dr.DEFUID
  from DOCOBJ dd
  LEFT JOIN DOCREL dr
    on dr.UID1 = dd.OBJUID
   and dr.DEFUID = 'SDAItemSecurityCode'
   and dr.TERMINATIONDATE = '9999/12/31-23:59:59:999'
 WHERE dd.OBJDEFUID = 'FDWDocumentMaster'
   and dr.uid2 is null
   and dd.TERMINATIONDATE = '9999/12/31-23:59:59:999'
```

### Find interfaces for a object by name

**For MsSql**

```sql
select dd.OBJNAME, dd.OBJDEFUID, di.INTERFACEDEFUID
  from DATAOBJ dd
  JOIN DATAOBJIF di
    on di.OBJOBID = dd.OBID
   AND dd.OBJNAME = 'Document-Name';
```
