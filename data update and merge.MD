## data manipulation：update and merge, using join to implement merge.

There are some simplified representations of typical merge operations between a source and target table. 
Notice that the source and the target table are the same in all scenarios. 
In each case, the result columns show the intended state of the target table after the merge operation has been performed.
Let's do it without using UPSERT statements and the MERGE syntax.

### For each merge scenario, create code that uses the source table to transform the target into the desired result. 

source tables:
```
Create table target(ID int not null, Value Char(5));
Create table source(ID int not null, Value Char(5));
 insert into  tb_target
 values(1,'A'),(2,'A'),(3, null),(5,'A'),(8,'A'),(9,null)
 insert into  tb_source
 values(1,null),(2,'B'),(4,'B'),(8,'B'),(9,'B')
 Create table tb_result(ID int not null, Value Char(5));
 select * from tb_source
 select * from tb_target
```
```
重置target表
truncate table tb_target
insert into  tb_target values(1,'A'),(2,'A'),(3, null),(5,'A'),(8,'A'),(9,null)
```

## 1. update: souce table change, applied to target table if target has same id
```
 update tb_target
 set tb_target.value=tb_source.value
 from tb_target
 join tb_source
 on tb_source.id=tb_target.id
```

## 2.merge:souce table changed, applied to target table let target has update data and new data
```
 update tb_target
 set tb_target.value=tb_source.value
 from tb_target
 join tb_source
 on tb_source.id=tb_target.id
 insert into tb_target
 select tb_source.id,tb_source.value
 from tb_target
 right join tb_source
 on tb_source.id =tb_target.id
 where  tb_target.id is null and tb_source.id is not null
 ```
## 3.append:
 insert into tb_target
 select tb_source.id,tb_source.value
 from tb_source

 

## 4.update_null_fill
```
update tb_target
set tb_target.value=tb_source.value
from
tb_target
join tb_source
on tb_target.id=tb_source.id and tb_target.value is null
 ```

## 5.merge_null_fill 
```
update tb_target
 set tb_target.value=tb_source.value
 from tb_target
 join tb_source
 on tb_source.id=tb_target.id and tb_target.value is null
 insert into tb_target
 select tb_source.id,tb_source.value
 from tb_target
 right join tb_source
 on tb_source.id=tb_target.id
 where  tb_target.id is null
 ```

## 6. update_override
```
update tb_target
 set tb_target.value=tb_source.value
 from tb_target
 join tb_source
 on tb_source.id=tb_target.id and tb_source.value is not null
 ```

## 7. merge_override
```
update tb_target
 set tb_target.value=tb_source.value
 from tb_target
 join tb_source
 on tb_source.id=tb_target.id and tb_source.value is not null
 insert into tb_target
 select tb_source.id,tb_source.value
 from tb_target
 right join tb_source
 on tb_source.id=tb_target.id
 where  tb_target.id is null
 select * from tb_target order by ID
```


