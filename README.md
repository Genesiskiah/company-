# company-
company database 
drop database company; 
drop database bakery;
drop database employee;


CREATE DATABASE company; 
USE company;


-- first table 
drop table if exists employee; 


CREATE TABLE employee  
(employee_id INT NULL PRIMARY KEY, 
first_name CHAR(12) NULL,  
last_name CHAR(12) NULL, 
gender CHAR(10) NULL, 
date_of_birth DATE NULL);

select * from employee;

INSERT INTO employee (employee_id, first_name, last_name, gender, date_of_birth)
 
 VALUES
 (1,jack,jones,male,2000-03-30),
 (2, samantha, dale, female, 1987-05-11),
 (3, richard, kenos, male, 1999-09-27),
 (4, abbey, capelle, female, 1995,01,01),
 (5, sarah, rogers, female, 1990-07-11),
 (6, dami, hutchinson, male, 2003-11-06),
 (7, tola, reid, male, 2001-10-30),
 (8, temi, beker, male, 1980-02-22),
 (9, sally, harry, female, 1975-08-17),
 (10, kate, ebenezer, female, 1988-04-05),
 (11, eve, brown, female, 2002-06-29),
 (12, nathan, green, male, 2000-07-25),
 (991, claire, fallows, female, 1971-06-06),
 (992, alam, tom, male, 1969-03-16),
 (993, saj, david, male, 1997-04-15),
 (994, beverly, walker, female, 1989-05-20),
 (995, ryan, samuels, male, 1982-12-10),
 (996, mustafa, gates, male, 2001-11-08);
 
 
 
 -- second table 
 
 USE company; 
 CREATE TABLE Industry
 (industry_id INT NOT NULL PRIMARY KEY, 
 First_name VARCHAR(20) NOT NULL,
 team_industry_name VARCHAR(20) NOT NULL, 
 team_manager_id INT NOT NULL);

 INSERT INTO Industry
  (industry_id, First_name, team_industry_name, team_manager_id)
 VALUES 
 (20,jack, drivers, 991),
 (21, samantha, backofhouse, 993),
 (22, richard, retail, 995),
 (23, abbey, industrial, 996),
 (24,sarah, frontofhouse, 994),
 (25,dami, frontofhouse, 994),
 (26, tola, drivers, 991),
 (27, temi, facilitiesmanagement, 992),
 (28,sally, backofhouse, 993),
 (29, kate, industrial, 996),
 (30, eve, retail, 995),
 (31, nathan, facilitiesmanagement, 992);
 
 -- third table 
 
 USE company; 
 CREATE TABLE inteviews 
 (interview_id INT NOT NULL PRIMARY KEY,
Team_indsutry_name VARCHAR(20)NOT NULL, 
temp_or_perm VARCHAR(20) NOT NULL,
number_of_interviews_per_week INT NOT NULL, 
interview_targert_per_week INT NOT NULL);


INSERT INTO interviews 
(interview_id, first_name, team_industry_name, temp_or_perm, 
number_of_interviews_per_week, interview_target_per_week)
 VALUES
 (1, drivers, perm, 23, 50),
 (2, backofhouse, temp, 49,70),
 (3, retail, perm, 69, 70),
 (4, industrial, temp, 97, 100),
 (5, frontofhouse, perm, 120, 100),
 (6, frontofhouse, temp, 87,100),
 (7, drivers, perm, 44, 50),
 (8, facilitiesmanagement, temp, 55, 70),
 (9, backofhouse, perm, 11, 70),
 (10, industrial, temp, 112, 100),
 (11,  retail, perm, 33, 70),
 (12, facilitiesmanagement, temp, 88, 70);
 
 -- fourth table 

USE company; 
CREATE TABLE Location 
(city VARCHAR(20) NOT NULL PRIMARY KEY, 
first_name VARCHAR(20) NOT NULL, 
manager_id INT NOT NULL, 
salary INT NOT NULL); 

INSERT INTO location 
(city, first_name, manager_id, salary)
VALUES
(london, jack, 991 , 25000),
(manchester, samantha, 993 , 22000),
(nottingham, richard, 995 , 25000),
(luton, abbey, 996 , 22000),
(liverpool, sarah, 994, 25000),
(birmingham,dami, 994, 22000),
(sheffield, tola, 991, 25000),
(glasgow, temi, 992, 22000),
(bristol, sally, 993 , 250000),
(london, kate, 996, 22000),
(cardiff, eve, 995 , 25000),
(middlesborough,nathan, 992 , 22000);

-- fifth table 


USE company;
CREATE TABLE management 
(manager_id INT NOT NULL,  
First_name VARCHAR(20) NOT NULL,  
team_industry_name VARCHAR(20) NOT NULL,  
Number_in_team INT NOT NULL);

INSERT INTO management 
(manager_id, first_name, team_industry_name, number_in_team)
VALUES
(991, claire, drivers, 2),
(992, alam, facilitiesmanagement, 2),
(993, saj, backofhouse, 2),
(994, beverly, frontofhouse,2),
(995, ryan, retail, 2),
(996, mustafa, industrial, 2);

-- Using any type of the joins create a view that combines multiple tables in a logical way
-- I WANT TO JOIN 2 TABLES TO FIND OUT WHO IS THE MANAGER IS FOR EACH INTERVIEWER


USE company; 

SELECT employee.employee_id, employee.employee_first_name, management.manager_id
FROM employee 
INNER JOIN management 
ON employee.employee_id = management.manager_id;

-- Creating a view with a inner join 

CREATE VIEW employee_view AS
SELECT e.employee._id, employee.employee_first_name, management.manager_id 
FROM employee
INNER JOIN management 
ON employee.employee_id = management.manager_id;

-- my view and inner joins are now created 


-- prepare a example query with a sub query , demonstrate how to extract data for analysis 
-- find namees of all employees who have interviewed more than 
-- 100 people per week 
--  
USE company;

SELECT employee.first_name , employee.last_name
FROM employee
WHERE employee.employee_id IN ( 
  SELECT interview.interviewer_id 
  FROM interview 
  WHERE number_of_interviews_per_week >100);
  
-- this allows me to see the employees first name and last name who is 
-- doing more than 100 interviews even though this information is on different tables 



-- stored procedure and function which selects the first name and gender of each employee 
-- this stored procedure returns all the employees who are female 
-- and also all the employees born after 1990-01-01

USE company; 
DELIMITER //
CREATE PROCEDURE employees ()
BEGIN 
SELECT first_name , gender 
FROM employee 
WHERE gender = 'female' AND employee.date_of_birth >1990-01-01;

END //

DELIMITER ; 

CALL employees;


-- Find out how many males and females there are in the company 
USE company; 
Select COUNT(gender), gender
FROM employee 
GROUP BY gender; 

-- Prepare an example query with group by and having clause 


SELECT employee_id , count(number_of_interviews_per_week)
FROM interview 
GROUP BY employee_id 
HAVING COUNT(number_of_interviews_per_week) >50
ORDER BY  count(number_interviews_per_week) DESC;


-- table used 



SELECT * FROM employee; 
SELECT * FROM industry; 
SELECT * FROM interviews;
SELECT * FROM location;
SELECT * FROM management 
