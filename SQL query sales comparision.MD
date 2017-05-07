### senari orequirement: compare same time period this year and previous year 计算同比

solutions: 伪代码，未验证
```
with cte as
(
select Y=year(orderdate),sum(sales)
from Tb_orders
where month(orderdate)(取orderdate的月和日)>(取startdate的月和日) and (取orderdate的月和日)<(取enddate的月和日)
group by year
)
select 
from CTE thisyear
left join CTE preyear
on thisyear.Y=preyear.Y+1
```
