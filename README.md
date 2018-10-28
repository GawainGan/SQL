# SQL
-- --------copy data from a table to another table--------------


create table orders(
	primary key (order_id),
    order_id int(11) not null auto_increment,
    order_no varchar(100) default null,
    user_name varchar(100) not null,
    product_name varchar(100) not null,
    order_status enum('CART',  'DRAFT',  'NEW',  'IN PROCESS',  ' COMPLETED',  'FAILED' ) DEFAULT 'CART',
    carete_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间'
    
);

select * from orders;
describe orders;


alter table orders 
add column unit_price decimal(9,2) null default 0.0 
after product_name;


-- 本来老师写错了，正好复习‘修改function’alter
-- -----------alter column unite_price to unit_price-------------
-- alter table orders
-- change column unite_price unit_price decimal(9,2), null default 0.0;
 
alter table orders 
add column quantity int not null default 0
after unit_price;


select * from orders;


INSERT INTO orders
	(order_no, user_name, product_name, unit_price, quantity)
VALUES
('KFC123','Gawain','香辣鸡腿煲套餐',36,1),
('KFC321','eric', '12345超级套餐',37,2),
('KFC631','goutian','全家智障桶无敌套餐',88,1),
('KFC465','lancy','新奥尔良大盘鸡鸡腿',85,3),
('KFC875', 'xiuyu','超级肯塔基鸡堡人气套餐',36,2),
('KFC990','wanmen','夏日缤纷欢乐时光桶',29,1);

select * from orders;


-- 这里提到一个重点
-- 当产品上线后时不可以重来的
-- 只能从一个版本变成下一个版本
-- eg：我可以将上一个版本的一个table拆分成3个tables
-- table1(order)  ------> table2(users/orders/products)
-- 要保证数据不能丢失
-- sol1 基于原来的表单再创建表单

-- ---------------------克隆/复制表单-----------------------
-- 方式1
-- CREATE TABLE  XXXX   [AS]  XXXX(updated名字可填可不填)  
-- SELECT * FROM XXX;


CREATE TABLE  users (
		primary key (user_id),
        user_id int(11) not null auto_increment,
        user_name varchar(255) default null
)select distinct user_name from orders;
-- 上面位第一步：创建用户


create table user_info 
as ( select user_name from orders);
describe users;
-- 上面是把用户放进user_info里面


create table products (
	primary key (product_id),
    product_id int(11) not null auto_increment,
    product_name varchar(100) default null,
    image_url varchar(225) default null,
    sales_status enum('NEW',  'DEPRECATED') DEFAULT 'NEW',
    price decimal(9,2) default null, -- 市场推荐价格（base price）
    carete_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
) SELECT DISTINCT product_name ,unit_price, AS price FROM orders;  -- ERROR HERE, UNKNOW 
