### 1.Given a table with 2 columns: colA & colB, write a SQL to output the duplicate rows. 
Table has values such as: a, 1; a, 2; a, 3; b, 1; b, 2; b, 3; a, 1; a, 2; a, 1
 
My answer:
 '''
select colA, colB
From table
group by colA, colB
having count(*)>1
 '''
------------------------------------  
### 2.Given 2 tables: Order & OrderDetail. Design schema as needed to answer the below questions. Send the schema design.
 
My answer:

CREATE TABLE [dbo].[tb_Order](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NULL,
    [CustomerID] [int] NULL,
    [Amount] [money] NULL,
 CONSTRAINT [PK_tb_Order] PRIMARY KEY CLUSTERED
(
    [OrderID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]
 
 
CREATE TABLE [dbo].[tb_OrderDetail](
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [OrderId] [int] NOT NULL,
    [ProductID] [int] NOT NULL,
    [ProductName] [varchar](50) NOT NULL,
    [OrderQty] [int] NOT NULL,
    [UnitPrice] [money] NOT NULL,
 CONSTRAINT [PK_tb_OrderDetail] PRIMARY KEY CLUSTERED
(
    [OrderDetailID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]
 
------------------------------------  
###  a. Write a SQL statement to find the number of orders that contained ice cream and either bread or milk and were made in last month?
 
My answer:
select count(*) from tb_Order
where OrderID in(
Select A.OrderId
from tb_OrderDetail A
join tb_OrderDetail B
on A.OrderID=B.OrderID
and A.ProductName='ice cream' and (B.productname ='bread' or B.ProductName='milk' )
)  and datediff(m,OrderDate ,getdate())=1
------------------------------------  
###  b. How many customers made orders that were more than $100 in Jan’16?
 
My answer:
select count(CustomerID)
from tb_Order
where Amount>100 and year(OrderDate)='2016' and month(OrderDate)=1
 
------------------------------------  
###  c. How many orders contained just one Order Item?
 
My answer:
select count(*)
from
(
select OrderID from tb_OrderDetail
group by OrderID
having count(OrderDetailID)=1
)as tmp
------------------------------------ 
###  d. How many customers came in last month but did NOT come in last-last month. Please answer using a ‘join’ without using ‘except’.
 
My answer:
 
select count(distinct lm.CustomerID)
from (select * from tb_Order 
where datediff(m, OrderDate ,getdate())=1) lm
left join
(select * from tb_Order 
where datediff(m, OrderDate ,getdate())=2)  llm
on lm.CustomerID=llm.CustomerID
where llm.OrderID is null
