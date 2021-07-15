# Get Oracle DB size
```sql
col pct_free format 999.9;  
col TABLESPACE format a29;  
col FrcLG_DB format a8;     
col FrcLG_TS format a8;     
col TOTAL_MB format 999999;  
col USED_MB  format 999999;  
col FREE_MB  format 99999;  
col PCT_FREE format 99999;  
col FRCLG_TS format a3;     
SET LINESIZE   999;         
SET PAGESIZE   999;         

select * from (
   SELECT df.tablespace_name TABLESPACE, df.total_space_mb TOTAL_MB, (df.total_space_mb - fs.free_space_mb) USED_MB,
  fs.free_space_mb FREE_MB, ROUND((100 * (fs.free_space / df.total_space)),1) PCT_FREE, db.FORCE_LOGGING FrcLG_DB, dts.FORCE_LOGGING FrcLG_TS
     FROM
    (
       SELECT tablespace_name, SUM(bytes) TOTAL_SPACE, ROUND(SUM(bytes) / 1048576) TOTAL_SPACE_MB
         FROM dba_data_files
     GROUP BY tablespace_name
    )
    df,
    (
       SELECT tablespace_name, SUM(bytes) FREE_SPACE, ROUND(SUM(bytes) / 1048576) FREE_SPACE_MB
         FROM dba_free_space
     GROUP BY tablespace_name
    )
    fs,
    (select FORCE_LOGGING, tablespace_name from dba_tablespaces) dts,
    (select FORCE_LOGGING from v$database) db
    WHERE df.tablespace_name = fs.tablespace_name(+)
      AND df.tablespace_name = dts.tablespace_name(+)
    UNION ALL
   SELECT TABLESPACE ||' <-Temp' TABLESPACE, TOTAL_SPACE_MB, USED_SPACE_MB,
  FREE_SPACE_MB, 100-100*USED_SPACE_MB/TOTAL_SPACE_MB asdf, db.FORCE_LOGGINg, dts.FORCE_LOGGING
     FROM
    (
       SELECT A.tablespace_name  TABLESPACE, D.mb_total TOTAL_SPACE_MB, SUM (A.used_blocks * D.block_size) / 1024 / 1024 USED_SPACE_MB,
        D.mb_total - SUM(A.used_blocks * D.block_size) / 1024 / 1024 FREE_SPACE_MB, (D.mb_total - SUM(A.used_blocks * D.block_size))/D.mb_total PCT_FREE
         FROM v$sort_segment A,
        (
           SELECT B.name, C.block_size, SUM (C.bytes) / 1024 / 1024 mb_total
             FROM v$tablespace B, v$tempfile C
            WHERE B.ts#= C.ts#
         GROUP BY B.name, C.block_size
        )
        D
        WHERE A.tablespace_name = D.name
     GROUP BY A.tablespace_name, D.mb_total
    ) t,
    (select FORCE_LOGGING, tablespace_name from dba_tablespaces) dts,
    (select FORCE_LOGGING from v$database) db
      where t.TABLESPACE = dts.tablespace_name(+)
  )  
  ORDER BY (case when instr(TABLESPACE, ' <-Temp')>0 then 2 else 1 end), TABLESPACE ; 
```
