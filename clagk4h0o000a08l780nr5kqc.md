# What is SQL?

We used to store data on paper in big filing cabinets but this is not the case anymore and we need t store it somewhere online. This is what is called a **Database**! 

SQL is not a database it is a language that is used to communicate with it. 
SQL stands for **Structured Query Language**. People calling it S.Q.L or *sequel*.

If you want to pull, edit, or add information to the database, you will need to express what you want in the SQL language. 

SQL is what stands between your app and the actual database.
If SQL is not the database so what is the database?

**Database** is defined as 
> A database is an organized collection of structured information, or data, typically stored electronically in a computer system. 

A database is just software that is built using a programming language (C, C++, or Java) and its responsibility is to manage and store your data on the hard disk or in memory. Every database has its own way to store data and how to operate on it. But luckily there is a group of databases called *Relational Databases* that they are sharing the same interface when you are interacting with them from the outside (**API**). Database examples will be **Postgres, MySQL, Oracle, or SQL Server**. If there is something common between all of these databases, it will be that you can interact with all of them using the same language which is SQL (with some slight modifications). 

In simple words, you don't need to learn all of these databases! All you need to know is how to interact with them using SQL and the specifics for every database can be easily learned later. 

A stander full-stack app should look something like this

![Stander full stack app.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1668413917484/x9G7WMkGX.png align="left")

Notice how the server interacts with the database using the SQL language. 

## How relational database store data?
Relational databases use tables, rows, and columns to store the data with the ability to form links between these tables. You might think of a database as being a giant spreadsheet. 


![SQL Table example](https://cdn.hashnode.com/res/hashnode/image/upload/v1668415534081/IhEjn0Uai.png align="left")

Notice that we store all users in a *table* like structure in which every user represents a *row* and every column represents a **property** of that user.  Notice also that every column is designed to store a certain kind of data. For example, the `id` column will store a unique ID for each user as a number and both the `first_name` and `last_name` columns will store the data in a `string` format. And every column has its own way to deal with missing values and what type of data to store. All of this stuff is determined at the time of creating the table but it can be changed later. 

## How SQL looks like?

Let's say you have your own e-commerce website. In this case, you will need to store your products and users in a database like PostgreSQL or MySQL. 

> Notice that we are not learning how to write SQL at the moment, Try just to get a feel for what it looks like. 

If you want to display all of your products to users you will structure your query to look something like this.
```sql
SELECT id, name, price, discount, img_url FROM product_table
```

If you want to update the product price you will do something like this. 
```sql
UPDATE products_table
SET price = 99.99
WHERE id = 1;
```
If you want to publish a new product, ... 
```sql 
INSERT INTO products_table (id, name, price, discount, img_url)
VALUES (2, "iPhone 14 Pro Max", 1399.00, 1%, "apple.com/iphone_14.png")
```
if you want to delete a product,...

```sql
DELETE FROM products_table WHERE product_id = 1
```

In the upcoming blogs, we will dive deeper into how to write SQL and how to use it with other programming languages like NodeJS, Python, and Rust...
