
### `schema.sql`
```sql
-- Drop existing database and create a new one
DROP DATABASE IF EXISTS Medical_mart;
CREATE DATABASE Medical_mart;
USE Medical_mart;

-- Create Cart table
CREATE TABLE Cart  
(
    cart_id VARCHAR(7) PRIMARY KEY,
    user_id VARCHAR(7) NOT NULL,
    prod_id VARCHAR(7) NOT NULL,
    prod_qnty INT DEFAULT(1) NOT NULL
);

-- Insert data into Cart table
INSERT INTO Cart VALUES
('cart101','cust101','prod101','50'),
('cart102','cust102','prod102','100'),
('cart103','cust103','prod103','60'),
('cart104','cust104','prod104','500'),
('cart105','cust105','prod105','150');

-- Create Admin table
CREATE TABLE Admin
(
    admin_id VARCHAR(7) PRIMARY KEY,
    admin_username VARCHAR(15) NOT NULL,
    admin_password VARCHAR(5) NOT NULL,
    admin_first_name VARCHAR(10) NOT NULL,
    admin_last_name VARCHAR (10) NOT NULL,
    admin_phonenumber VARCHAR(10) NOT NULL
);

-- Insert data into Admin table
INSERT INTO Admin VALUES 
('88276','RAI MEDICO','44556','RAJESH','RAI','8827622890');

-- Create Customer_details table
CREATE TABLE Customer_details
(
    cust_id VARCHAR(7) PRIMARY KEY,
    cust_first_name VARCHAR(20) NOT NULL,
    cust_last_name VARCHAR(20) NOT NULL,
    cust_address VARCHAR (30) NOT NULL,
    cust_password VARCHAR (5) NOT NULL,
    cust_number VARCHAR(10) NOT NULL,
    cust_pincode VARCHAR(6) NOT NULL,
    cart_id VARCHAR (7) NOT NULL,
    FOREIGN KEY (cart_id) REFERENCES Cart(cart_id)
);

-- Insert data into Customer_details table
INSERT INTO Customer_details 
(cust_id, cust_first_name, cust_last_name, cust_address, cust_password, cust_number, cust_pincode, cart_id)
VALUES
('cust101','ADITYA','RAI','NEW MARKET','11111','9306877829','473444','cart101'),
('cust102','MANSI','RAI','KAMLAGANJ','22222','9306887638','473477','cart102'),
('cust103','AISHWARYA','RAI','NEW BLOCK','33333','9306877987','473488','cart103'),
('cust104','LOVISH','RAI','PHYSICAL ROAD','44444','9306877456','473551','cart104'),
('cust105','RAJESH','RAI','MADHAV CHOWK','55555','9306877985','473496','cart105');

-- Create Seller_details table
CREATE TABLE Seller_details   
(
    seller_id VARCHAR(7) PRIMARY KEY,
    seller_comp VARCHAR(20) NOT NULL,
    seller_password VARCHAR(5) NOT NULL,
    seller_address VARCHAR(30) NOT NULL,
    seller_phone_num VARCHAR(10) NOT NULL
);

-- Insert data into Seller_details table
INSERT INTO Seller_details VALUES
('sell101','CIPLA','67890','GANDHI NAGAR','7974156789'),
('sell102','MANFORCE','67890','SIDDHIVINAYAK','7974158888'),
('sell103','ABBOTT','67890','POHARI ROAD','7974159999'),
('sell104','DABUR','67890','MADHAV CHOWK','7974155678'),
('sell105','MANKIND','67890','KAMLAGANJ','7974154563');

-- Create Manufacturer_details table
CREATE TABLE Manufacturer_details
(
    man_id VARCHAR (7) NOT NULL,
    man_comp VARCHAR (30) NOT NULL,
    man_prod_name VARCHAR(30) NOT NULL,
    man_date VARCHAR (20) NOT NULL,
    exp_date VARCHAR (20) NOT NULL,
    FOREIGN KEY (man_id) REFERENCES Seller_details(seller_id)
);

-- Insert data into Manufacturer_details table
INSERT INTO Manufacturer_details VALUES 
('sell101','CIPLA','KIMDROX 200 mg','2022-08-25','2026-09-30'),
('sell102','MANFORCE','ACITO SP','2022-07-14','2026-03-15'),
('sell103','ABBOTT','ACILOC 150 mg','2022-12-25','2026-05-07'),
('sell104','DABUR','BECOSULES','2022-05-02','2026-08-20'),
('sell105','MANKIND','LEVOCETIRIZINE','2022-03-13','2026-11-01');

-- Create Product_details table
CREATE TABLE Product_details
(
    product_id VARCHAR(7) PRIMARY KEY,
    product_name VARCHAR(30) NOT NULL,
    product_company VARCHAR(30) NOT NULL,
    prod_use VARCHAR(300) NOT NULL,
    prod_mg VARCHAR(10) NOT NULL,
    prod_Commission VARCHAR(10) NOT NULL,
    prod_Cost VARCHAR(5) NOT NULL,
    prod_qty VARCHAR(15) NOT NULL,
    seller_id VARCHAR(7),
    seller_comp VARCHAR (20),
    FOREIGN KEY (seller_id) REFERENCES Seller_details (seller_id)
);

-- Insert data into Product_details table
INSERT INTO Product_details VALUES
('prod101','KIMDROX 200 mg','KIM LAB','Kimdrox 200mg Tablet DT is an antibiotic medicine used to treat bacterial infections in your body.','200mg','10 %','180', '10 tabtet','sell101','CIPLA'), 
('prod102','ACITO SP','SANTO','Acito-SP Tablet is a combination medicine used for short-term relief of pain, inflammation, and swelling in conditions affecting joints and muscles.','350 mg', '25%','250',' 10 tablet','sell102','MANFORCE'),
('prod103','ACILOC 150 mg ', 'CADILA', 'Aciloc 150 Tablet is a Tablet manufactured by Cadila Pharmaceuticals Ltd. It is commonly used for the diagnosis or treatment of Ulcers in stomach, Zollinger-Ellison syndrome, gastroesophageal reflux disease, heartburn.','150 mg', '30 %','70', ' 15 tablet','sell103','ABBOTT'),
('prod104','BECOSULES','PFIZER LTD.', 'Becosules capsule is a multivitamin that improve overall health and wellness by improving metabolism and repairing tissues.','25 mg','30 %','90','20 tablet','sell104','DABUR'),
('prod105','LEVOCETIRIZINE','ALKEM','Levocetirizine is used to relieve the symptoms of hay fever and hives of the skin.','10 mg', '50 %','100','10 tablet','sell105','MANKIND');

-- Create Order_details table
CREATE TABLE Order_details
(
    order_id VARCHAR (7) PRIMARY KEY,
    cust_id VARCHAR(7) NOT NULL,
    product_id VARCHAR(7) NOT NULL,
    seller_id VARCHAR(7) NOT NULL,
    cart_id VARCHAR(7) NOT NULL,
    admin_id VARCHAR(7) NOT NULL,
    ordertime DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    man_date VARCHAR(20) NOT NULL,
    exp_date VARCHAR(20) NOT NULL,
    FOREIGN KEY (cust_id) REFERENCES Customer_details (cust_id),
    FOREIGN KEY (product_id) REFERENCES Product_details (product_id),
    FOREIGN KEY (seller_id) REFERENCES Seller_details (seller_id),
    FOREIGN KEY (cart_id) REFERENCES Cart(cart_id),
    FOREIGN KEY (admin_id) REFERENCES Admin(admin_id)
);

-- Insert data into Order_details table
INSERT INTO Order_details VALUES
('order01','cust101','prod101','sell101','cart101','88276','2023-05-25 00:00:00','2022-08-25','2026-09-30'),
('order02','cust102','prod102','sell102','cart102','88276','2023-05-07 00:00:00','2022-07-14','2026-03-15'),
('order03','cust103','prod103','
