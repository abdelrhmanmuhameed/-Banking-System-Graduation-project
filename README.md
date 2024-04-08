# -Banking-System-Graduation-project
BankSystemDesign
First we Received a Required Document from the client

![Document](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/43403726-1c2c-4552-b5e9-0f5d206cb983)


 <h2>Second We Start to Draw The ERD</h2>
 
![ERD](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/6cbd1729-56d8-43b6-bf3d-4ecc4aadb499)

<h2>Thrid We Start to Execute The Mapping</h2>

![Mapping](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/fab4c0b0-4ebd-4264-9ab5-95519117fab4)

<h2>Fourth implementation of OLTP schema</h2>

![oltp](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/7d1ef211-bcf8-49c4-ad64-f64763ebf116)

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

![DWH Diagram](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/a03d6004-8b58-46d0-b6ba-1db533820d1f)

<h2>Business Intelligence Using SSIS SSAS SSRS</h2>
<h2>First Integration using SSIS</h2>

![ETL](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/0035c7e9-65e5-4310-a196-eba769b9f6ef)

<h2>Secound Cube "The cube is Genarlized About Business and Specified for the CEO And CMO Should closely track financial performance, set clear goals, and analyse key metrics to measure profits. they must identify and manage risks by conducting assessments and creating strategies to mitigate potential threats."
</h2>

![SSAS1](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/b05887c1-8568-4066-93a2-9296223f8319)

<h2>Thrid Reporting using SSRS</h2>


![SSRS2](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/1178f8bc-5a24-49d3-9ab3-dc309190319b)
![SSRS](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/3a841f4d-e08b-4ff4-8fc0-8e1a12e775fc)
![SSRS](https://github.com/abdelrhmanmuhameed/-Banking-System-Graduation-project/assets/81581455/8034737e-931a-4c0a-80a3-683342840489)

<h2>Visualzation</h2>
<hr>
<h2>Using Power BI</h2>
<hr>
<h2>The analysis of the Bank application history data of sales, customer, product and operations departments to figure out the company's performance to get an overview insight of both departments will empower the company to make data-driven decisions that can lead to increased revenue, cost savings, improved customer satisfaction, and enhanced overall business performance.</h2>
<hr>
<h2>It allows businesses to respond to market dynamics, customer preferences, and operational challenges more effectively and provides valuable insights to the operations, marketing, and sales teams for informed decision-making.</h2>





