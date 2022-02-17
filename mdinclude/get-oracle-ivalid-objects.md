## Get Oracle invalid objects

```sql
---------------------------------------
-- Set Profile parameters.
--
-- SET TIME       ON
-- SET TIMING     ON
SET VERIFY     OFF
SET LINESIZE   999
SET PAGESIZE   999
SET LONG       999
SET COLSEP     ' | '
SET UNDERLINE  =
--SET RECSEPCHAR "-"
TTITLE        OFF
---------------------------------------
-- Set COLUMN FORMAT and HEADINGs.
COLUMN OWNER                 FORMAT A8         HEADING "OWNER"
COLUMN OBJECT_NAME           FORMAT A35        HEADING "OBJECT_NAME"
COLUMN OBJECT_TYPE           FORMAT A20        HEADING "OBJECT_TYPE"
COLUMN LAST_DDL_TIME         FORMAT DATE       HEADING "LAST_DDL_TIME"
COLUMN CREATED               FORMAT DATE       HEADING "CREATED"
COLUMN STATUS                FORMAT A15        HEADING "STATUS"
SET FEEDBACK OFF
--ALTER SESSION SET NLS_DATE_FORMAT = 'DD.MM.YYYY HH24:MI:SS';
---------------------------------------
-- Select
select OBJECT_NAME, 
       OBJECT_TYPE, 
       LAST_DDL_TIME, 
       CREATED, 
       STATUS 
from   user_objects 
where STATUS != 'VALID' 
order by   LAST_DDL_TIME DESC ;
---------------------------------------
```
