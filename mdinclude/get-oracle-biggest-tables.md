## Get Oracle biggest tables

```sql
SET VERIFY     OFF
SET LINESIZE   999
SET PAGESIZE   999
SET LONG       999
SET COLSEP     ' | '
SET UNDERLINE  = 
COLUMN size_mb       FORMAT 99999
COLUMN owner         FORMAT A35
COLUMN segment_name  FORMAT A35

select
   *
from (
   select
      owner,
      segment_name,
      bytes/1024/1024 size_mb
   from
      dba_segments
   where
      segment_type = 'TABLE'
   order by
      bytes/1024/1024 desc)
where
   rownum <= 10;
```
