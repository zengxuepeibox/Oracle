# 实验4：对象管理
## 实验目的：

了解Oracle表和视图的概念，学习使用SQL语句Create Table创建表，学习Select语句插入，修改，删除以及查询数据，学习使用SQL语句创建视图，学习部分存储过程和触发器的使用。
## 查询数据：

1.查询某个员工的信息
```
 select * from employees where employee_id=11;
```
2.递归查询某个员工及其所有下属，子下属员工。
```
select a.*,b.Name AS "下属"
from employees a,employees b
where a.EMPLOYEE_ID=b.MANAGER_ID;
```
3.查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。
```
select a.*,(select sum(b.product_num*b.product_price)
from order_details b
where b.order_id=c.order_id
group by b.order_id)-a.discount 
from orders a,order_details c
where a.order_id=c.order_id;
```
4.查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。
```
select a.customer_name,a.customer_tel,c.product_type as "产品名称"
from orders a,order_details b,products c
where a.order_id=b.order_id and b.product_name=c.product_name;
```
5.查询出所有空订单，即没有订单详单的订单。
```
select a.*
from orders a left join order_details b
on a.order_id=b.order_id
where b.order_id is null
```
6.查询部门表，同时显示部门的负责人姓名。
```
select departments.*,employees.name as "负责人"
from departments,employees
where departments.department_id=employees.department_id
```
7.查询部门表，统计每个部门的销售总金额。
```
 select a.department_name,(select sum(c.Trade_Receivable)from orders c  where c.employee_id=d.employee_id group by c.employee_id)as "销售总额"
from departments a,employees b,orders d
where a.department_id=b.department_id
and d.employee_id=b.employee_id;
```
