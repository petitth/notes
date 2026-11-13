# SQL notes

## Oracle

### Dummy

```sql
select 1 from DUAL ;
select * from DUAL ;
```

### Time

```sql
-- current time
select CURRENT_TIMESTAMP as TS from DUAL;
select CURRENT_TIMESTAMP(3) as TS from DUAL;
select CURRENT_DATE as TS from DUAL;
select systimestamp as TS from DUAL ;
select sysdate as current_date from DUAL ;

-- time difference
select TO_DATE('2023-11-30 10:10:25','YYYY-MM-DD HH24:MI:SS') - TO_DATE('2023-11-28 10:06:11','YYYY-MM-DD HH24:MI:SS') DIFF_IN_DAYS from DUAL ;
select TO_TIMESTAMP('2023-11-30 10:10:25','YYYY-MM-DD HH24:MI:SS') - TO_TIMESTAMP('2023-11-30 10:06:11','YYYY-MM-DD HH24:MI:SS') DIFF from DUAL ;
select ((TO_DATE('2023-11-30 10:10:25','YYYY-MM-DD HH24:MI:SS') - TO_DATE('2023-11-30 10:06:11','YYYY-MM-DD HH24:MI:SS')) * 24 * 60 * 60) DIFF_IN_SECONDS from DUAL ; -- 00:04:14
```

### Text

```sql
UPPER(some-field)
LOWER(some-field)
```

### Conversion

```sql
select TO_CHAR(sysdate,'YYYY-MM-DD HH24:MI:SS') CURRENT_TIME from DUAL ;
select to_char(sysdate, 'yyyy-mm-dd') current_date, to_char(sysdate, 'hh24:mm:ss') current_time from DUAL;
```

### System

```sql
-- DB version
select * from V$VERSION ; 

-- get tables names from a schema
select object_name as table_name from all_objects where object_type = 'TABLE' order by object_name

-- check PDBs (use system user)
SELECT pdb_name, status FROM cdb_pdbs

```

### Export schema to a picture

right click on schema / print diagram / to image

### Change date format in SQL Developer

preferences \ database \ NLS

## Sqlite

install it (linux) : ```apt-get install sqlite3``` 

```bash
# open db / create it if it does not exist
sqlite3 northwind.db

# run sql from a file (wait until prompt comes back)
.read file.sql

# see db definition
.fullschema

# activate query header or not (show fields'name)
.header on
.header off

# run query : just enter it normally (don't forget ";")
select * from my-table ;

# quit connecion
.exit
```