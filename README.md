create database if not exists COREX;
USE COREX;

CREATE TABLE Categories (
category_id int NOT NULL,
category_name varchar(60),
category_description varchar(255),
CONSTRAINT PK_Catagories PRIMARY KEY (category_id)
);

CREATE TABLE Products (
product_id int NOT NULL,
product_name varchar(255),
product_description varchar(255),
product_price decimal,
product_status varchar(25),
CONSTRAINT PK_Products PRIMARY KEY (product_id)
);

CREATE TABLE Product_Categories (
product_id int NOT NULL,
category_id int NOT NULL,
CONSTRAINT Product_Categories PRIMARY KEY (product_id, category_id),
CONSTRAINT FK_Product_Categories_Products FOREIGN KEY (product_id) REFERENCES Products(product_id),
CONSTRAINT FK_Product_Categoriesr_Categories FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);

CREATE TABLE Option_Groups (
Option_group_id int NOT NULL,
Option_group_name varchar(60),
CONSTRAINT PK_Option_Group PRIMARY KEY (Option_group_id)
);

CREATE TABLE Option_Values (
option_value_id int NOT NULL,
value_name varchar(60),
CONSTRAINT PK_Option_Values PRIMARY KEY (option_value_id)
);

CREATE TABLE Product_options (
product_id int NOT NULL,
option_group_id int NOT NULL,
option_group_type varchar(60),
CONSTRAINT PK_Product_options PRIMARY KEY (product_id, option_group_id),
CONSTRAINT FK_Product_options_Products FOREIGN KEY (product_id) REFERENCES Products(product_id),
CONSTRAINT FK_Product_options_Option_Groups FOREIGN KEY (option_group_id) REFERENCES Option_Groups(option_group_id)
);
CREATE TABLE Product_Values (
product_id integer NOT NULL,
option_group_id integer NOT NULL,
option_value_id integer not null,
CONSTRAINT PK_Product_Values PRIMARY KEY (product_id, option_group_id,option_value_id),
CONSTRAINT FK_Product_Values_Products FOREIGN KEY (product_id) REFERENCES Products(product_id),
CONSTRAINT FK_Product_Values_Option_Groups FOREIGN KEY (option_group_id) REFERENCES Option_Groups(option_group_id),
CONSTRAINT FK_Product_Values_Option_Values FOREIGN KEY (option_value_id) REFERENCES Option_Values(option_value_id)
);
CREATE TABLE Users (
user_id int NOT NULL,
user_role varchar(60),
user_name varchar(60),
user_email varchar(60),
user_password varchar(255),
CONSTRAINT PK_Users PRIMARY KEY (user_id)
);

CREATE TABLE Shopping_Cart (
product_id int NOT NULL,
user_id int NOT NULL,
CONSTRAINT Shopping_Cart PRIMARY KEY (product_id, user_id),
CONSTRAINT FK_Shopping_Cart_Products FOREIGN KEY (product_id) REFERENCES Products(product_id),
CONSTRAINT FK_Shopping_Cart_Users FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Orders (
order_id int NOT NULL,
user_id int NOT NULL,
user_name varchar(60),
user_email varchar(60),
constraint pk_Orders primary key(order_id),
constraint fk_Orders foreign key(user_id) references Users (user_id)
);

CREATE TABLE Order_History (
order_id int NOT NULL,
user_id int NOT NULL,
history_time datetime,
history_status varchar(60),
history_notes varcharacter(255),
constraint pk_Order_History primary key(order_id,history_time),
constraint fk_Order_History_Users foreign key(user_id) references Users (user_id),
constraint fk_Order_History_Orders foreign key(order_id) references Orders (order_id)
);

CREATE TABLE Order_Totals (
total_id integer not null,
order_id integer not null,
total_name varchar(60),
total_amount decimal,
constraint pk_Order_Totals primary key (total_id),
constraint fk_Order_Totals foreign key (order_id) references Orders (order_id)
);

CREATE TABLE Order_Products (
item_id integer not null,
order_id integer not null,
item_name varchar(60),
item_price decimal,
item_quantity integer,
constraint pk_Order_Products primary key (item_id),
constraint fk_Order_Products foreign key (order_id) references Orders (order_id)
);
 
CREATE TABLE Order_Product_Options (
item_option_id integer not null,
item_id integer not null,
option_group varchar(60),
option_value varchar(60),
constraint pk_Order_Product_Options primary key (item_option_id),
constraint fk_Order_Product_Options foreign key (item_id) references Order_Products (item_id)
);
