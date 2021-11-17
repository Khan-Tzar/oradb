
## Get Oracle sessions in 
```bash
select sample_time, count(*)
  from pt_session ash
 where ash.sample_time between to_date('2021/11/17 13:50:00', 'yyyy/mm/dd hh24:mi:ss') and to_date('2021/11/17 23:10:00', 'yyyy/mm/dd hh24:mi:ss')
 group by sample_time
 order by sample_time;
 ```
