# Get Oracle Version
```sql
col VERSION format a10;
col VERSION_FULL format a13;
col EDITION   format a3; 
col FAMILY    format a5; 
col DATABASE_TYPE   format a7; 
SET LINESIZE   999;         
SET PAGESIZE   999;  
 
select INSTANCE_NAME, VERSION_FULL, EDITION, FAMILY, DATABASE_TYPE from v$instance;
```
