# SQL Queries

- [SQL Queries](#sql-queries)
	- [Utility Queries](#utility-queries)
		- [Setup new daabase and user](#setup-new-daabase-and-user)
		- [Search for tables and columns](#search-for-tables-and-columns)
		- [Reindex tables](#reindex-tables)
		- [Find row count in all tables](#find-row-count-in-all-tables)
	- [SPF Queries](#spf-queries)
		- [Check the documents count](#check-the-documents-count)
		- [Clean up terminated records in SPF](#clean-up-terminated-records-in-spf)
		- [Find VTL Tables related to Jobs](#find-vtl-tables-related-to-jobs)

## Utility Queries

### Setup new daabase and user

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

### Reindex tables

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
  ORDER BY tableschema desc	;



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