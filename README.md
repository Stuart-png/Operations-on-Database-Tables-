drop table if exists books;
create table books(
	Book_ID SERIAL primary key,
	Titile varchar(100),
	Author varchar(100),
	Genre varchar(70),
	Published_Year int,
	price numeric(10,3),
	Stock int
);

drop table if exists customers;
create table customers(
	Customer_ID int primary key,
	Name varchar(50),
	Email varchar(70),
	Phone int,
	City varchar(50),
	Country varchar(70)
);


drop table if exists orders;
create table orders(
	Order_ID int,
	Customer_ID int references customers(Customer_ID),
	Book_ID int references books(Book_ID),
	Order_date date,
	Quantity int,
	Total_Amount numeric(10,3)
);

---- 1) retrive all book in the 'friction' table----

select * 
from books 
where genre = 'Fiction';

----- 2) find books published after the year 1950:
SELECT *
FROM BOOKS
WHERE published_year>1950;

----- 3) List all customer from canada:
select *
from customers
where country = 'Canada';

---- 4) Retrive total stock of book Available:

select sum(stock) as total_stock
from books;

----- 5) show orders placed in november 2023:

select *
from orders
where order_date between '2023-11-01' and '2023-11-30';


----- 6) find the details of the most expensive book:

select *
from books
order by price desc
limit 1;


----- 7) show all customer who have ordered more than 1 quantity of book:


select * 
from orders
where quantity>1
order by quantity;


----- 8) retrives all orders where the total amount exceeds 20:

SELECT *
FROM ORDERS
WHERE total_amount>20;


----- 9) List all genre available in book table:

SELECT DISTINCT(GENRE)
FROM BOOKS;


------ 10) FIND THE BOOK WITH THE LOWEST STOCK:

select *
from books
order by price 
limit 1;


------ 11) Calculate total revenue generated from all orders:


SELECT SUM(TOTAL_AMOUNT)
FROM ORDERS;


--ADVANCE QUESTION---

------ 12) RETRIVE TOTAL NUMBER OF BOOKS SOLD FOR EACH GENRE:

SELECT b.genre, SUM(o.quantity)
from books b
join orders o
on b.book_id = o.book_id
GROUP BY GENRE;


------ 13) find the average price of book in 'Fantasy' genre:
SELECT AVG(PRICE)
FROM BOOKS
WHERE GENRE = 'Fantasy';

------ 14) list customers who have placed atleast 2 orders:

SELECT Customer_id, count(order_id)
from orders
group by customer_id
having  count(order_id)>=2;


------ 15) Find the most frequent ordered book:

SELECT o.book_id, b.titile, COUNT(order_id) 
FROM orders o
join books b on b.book_id = o.book_id
GROUP BY book_id, b.titile
ORDER BY COUNT(order_id) DESC
LIMIT 1;



------- 16) show the top 3 most expensive book of 'fantasy' genre:

select *
from books 
where genre='Fantasy'
order by price desc
limit 3;


------- 17) Retrive total quantity of book sold by each author:



select sum(o.quantity), b.author
from orders o
join books b on o.book_id=b.book_id
group by author;


------ 18) List the cities where customers who spent over 30$ are located:


SELECT DISTINCT(c.city), o.total_amount as total_spent
from customers c
join orders o on c.cuStomer_id=o.customer_id
WHERE o.total_amount>110;


------- 19) Find the customers who spent most on orders:

SELECT c.name, SUM(o.total_amount)
from customers c
join orders o on c.customer_id=o.customer_id
GROUP BY  c.customer_ID
ORDER BY SUM(o.total_amount) DESC ;
