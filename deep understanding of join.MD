## problem of join
```
create table a (Id int)
create table b (Id int)
insert into a values(1),(1),(1),(1),(1)
insert into b values(1),(1),(1)
```

## guess the result for each query below:
```
select count(*) from a inner join b on a.id =b. id
select count(*) from a left join b on a.id =b. id
select count(*) from a full join b on a.id =b. id
select count(*) from a cross join b 
```
