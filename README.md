# -Banking-System-Graduation-project
BankSystemDesign
First we Received a Required Document from the client
![277184062-921f5c42-373c-4302-9adc-ba5e8257950f](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/7cdf445d-2c54-4d38-93b5-23bdfd05771c)

 <h2>Second We Start to Draw The ERD</h2>
 
![ERD](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/6cbd1729-56d8-43b6-bf3d-4ecc4aadb499)

<h2>Thrid We Start to Execute The Mapping</h2>

![Mapping](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/fab4c0b0-4ebd-4264-9ab5-95519117fab4)

<h2>Fourth implementation of OLTP schema</h2>
![Screenshot (5)](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/d03ca8cb-78c4-4c6f-ab0f-ac6c1eb1200b)

<h2>Let's go for structure</h2>
create database Union_Bank
go


USE Union_Bank
go


--2)Q2.Create all the tables mentioned in the database diagram.
--create tables & add constraint Section
--3)Q3. Create all the constraints based on the database diagram.


--Creating table named Customers


create table Customers
(
CustomerId int primary key identity(1,1),
FirstName varchar(50) not null,
LastName varchar(50) not null,
Gender varchar(1) not null,
Email varchar(50) not null,
BirthDate date not null,
Age as year(getdate())-year (BirthDate),
City varchar(50) not null,
State varchar(50) not null,
--constraint ch1 check(Gender in ('m','f'))
);
go


--Creating table named Customer_phones


create table Customer_phones
(
CustomerId int,
phone varchar(50) not null,
constraint Customer_phones_IDS primary key (CustomerId, phone),
constraint Customer_phones_Customers foreign key (CustomerId)
references Customers (CustomerId)
);
go


--Creating table named Branch


create table Branch
(
BranchId int primary key,
BranchName varchar(50) not null,
Branch_Location varchar(50) not null,
);
go


--Creating table named Account


create table Account
(
AccountNumber int primary key,
AccountType varchar(50) ,
Balance float not null,
OpenDate date default getdate(),
Account_Status varchar(30) not null,
Branchid INT,
CustomerId int,
constraint customer_Account foreign key (CustomerId)
references Customers (CustomerId),
constraint Branch_Account foreign key (Branchid)
references Branch (BranchId),
--constraint ch2 check(Account_status in ('active','not active')),
);
go


---we can check on account type through this constraint to check if
--- the account saving or current ---
alter table account add constraint check_status check(accounttype in ('saving','current'))


--Creating table named Transactions


create table Transactions
(
Transaction_Type_Id int primary key identity (1,1),
TransactionType varchar(30) not null,
);
go


--Creating table named Card


create table Card (
CardNumber int primary key,
CardType varchar(30) not null,
card_Status varchar (30) not null,
ExpiryDate date not null,
AccountNumber int,
constraint Card_Account foreign key (AccountNumber)
references Account (AccountNumber)
);
go


--Creating table named Employee


create table Employee
(
EmployeeId int primary key,
FirstName varchar(30) NOT NULL,
LastName varchar(30) NOT null,
Salary DECIMAL NOT NULL,
Position varchar(50) NOT NULL,
SupervisorID int,
branchid INT,
Dno INT,
constraint super_Employee foreign key(SupervisorID)
REFERENCES Employee (EmployeeId),
CONSTRAINT branchid_Employee foreign key(branchid)
references branch(branchid) ,
);
go


--create table named Department


CREATE TABLE Department
(
Dnumber INT PRIMARY KEY,
Dname VARCHAR(50) NOT NULL,
MgriD INT,
constraint manager_Employee foreign key(MgriD)
REFERENCES Employee (EmployeeId)
)


ALTER TABLE employee
ADD constraint Dno_Employee foreign key(Dno)references Department (DNumber)


--Creating table named Loan


create table Loan
(
LoanId int primary key,
Amount decimal not null,
loan_months_terms int not null,
LoanType varchar(30) not null,
StartDate date not null,
EndDate date not null,
InterestRate varchar(20) not null,
customerid int,
BranchId int,
constraint Loan_customer foreign key (customerid)
references Customers (customerid),
constraint Loan_Branch foreign key (BranchId)
references Branch (BranchId)
);
go


--Creating table named ATM


create table ATM
(
AtmId int primary key,
Atm_location varchar(50) NOT NULL,
Atm_status varchar(30) NOT NULL ,
BranchId int,
constraint ATM_Branch foreign key (BranchId)
references Branch(BranchId)
);
GO


--create table Account_Atm_transcation


CREATE TABLE Account_Atm_transcation
(
TransactionDate DATE DEFAULT GETDATE() ,
AccountNumber INT,
transactionid INT identity(1,1) primary key,
Transaction_Type_Id int,
Atmid INT,
Amount FLOAT NOT NULL,
CONSTRAINT Atm_account_transcation foreign key (AccountNumber) references Account(AccountNumber),
CONSTRAINT transcation_Account_Atm foreign key (Transaction_Type_Id) references Transactions(Transaction_Type_Id)
);
go

<h2>DATAWAREHOUSE</h2>
<h2>Denormalized Data Base star Schema</h2>
Business Process "Transactions "
Grain "Low Level"
Dimensions -----> Account - Branch - Customer -ATM - Date - card
Fact Table: Transaction Table

<h2>Star OLTP schema</h2>
![Screenshot (7)](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/7355d837-425f-443b-8108-fbe1d9823180)

