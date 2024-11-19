# goit-rdb-hw-05

## Завдання 1.
Відображили таблицю order_details та поле customer_id з таблиці orders 


USE mydb;

SELECT 
    od.*, 
    (SELECT o.customer_id 
     FROM orders o 
     WHERE o.id = od.order_id) AS customer_id
FROM 
    order_details AS od;


## Завдання 2.
Запит, який відображає таблицю order_details. 

use mydb;

select 
    *
from 
    order_details
where 
    order_id in (
        select 
            id
        from 
            orders
        where 
            shipper_id = 3
    );


## Завдання 3.
Запит, вкладений в операторі FROM, який обирає рядки з умовою quantity>10 з таблиці order_details.


use mydb;

select 
    order_id, avg(quantity) as average_quantity
from 
    (select *
    from order_details
    where quantity > 10)
    as temporal_table
    
group by order_id;

## Завдання 4.
Розв’язання завдання 3, використовуючи оператор WITH для створення тимчасової таблиці temp.

use mydb;
with temporal_table as
(select *
    from order_details
    where quantity > 10)
select 
    order_id, avg(quantity) as average_quantity
from 
    temporal_table
group by order_id;


## Завдання 5.
Створення функції з двома параметрами, яке ділить перший параметр на другий.

use mydb;
drop function if exists quantity_divide

delimiter //
  create function quantity_divide(num_a float, num_b float)
  returns float
  deterministic
    begin
      if num_b = 0 then
        return null;
	end if;
    return num_a / num_b;
	end //
delimiter ;
select
  order_id,
  quantity,
  quantity_divide(quantity, 2.0) as result_q
from
  order_details;
