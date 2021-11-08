# Check Undo Usage
```sql
col STATUS format a10;
col     CN format 999999;  
col     MB format 999999;
col   COMM format a70;

select status, count(1) as CN, sum(bytes)/1024/1024 MB
, decode(status, 'ACTIVE','Undo data that is part of the active transaction.','EXPIRED','Undo data whose age is greater than the undo retention period.','UNEXPIRED','Undo data whose age is less than the undo retention period.') comm
   from dba_undo_extents
   group by status
   ;
```

## Example output
```sql
STATUS          CN      MB COMM
---------- ------- ------- ----------------------------------------------------------------------
UNEXPIRED     3543    2985 Undo data whose age is less than the undo retention period.
EXPIRED       1345     975 Undo data whose age is greater than the undo retention period.
ACTIVE           1       1 Undo data that is part of the active transaction.
```
