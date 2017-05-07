### problem : SQL Server怎样求得两组时间段之间，重叠了的时长

例如：
时间段1：2013-06-13 08：00 至 2013-06-13 18：00
时间段2：2013-06-13 15：00 至 2013-06-13 20：00

```
Create Table #t
(startdate1 datetime,
enddate1 datetime,
startdate2 datetime,
enddate2 datetime)

SELECT    
Case When startdate1 > enddate2 Then 0 
         When startdate2 > enddate1 Then 0
         When startdate1 > startdate2 and enddate1 > enddate2 Then DateDiff(Hour,startdate1,enddate2)
         When startdate1 > startdate2 and enddate1 <= enddate2 Then DateDiff(Hour,startdate1,enddate1)
         When startdate1 <= startdate2 and enddate1 > enddate2 Then DateDiff(Hour,startdate2,enddate2)
         When startdate1 <= startdate2 and enddate1 <= enddate2 Then DateDiff(Hour,startdate2,enddate1)
         Else 0 End

FROM #t
```
