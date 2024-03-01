# CP 2 Spring 24 DBMS Lab Sec A

1. Insert this [(Link)](https://github.com/TashinParvez/MySQL_From_Zero/blob/Tashin/Files/hr_schema.sql) database on your system.
2. Perform the operations below 

   > 1. Find out the country names which is having “ae” or “ea” as last character and located in “Europe” region.
   > 2. Find out the employee’s name who are having job history experience of more than 8 years but not more than 10.
   > 3. Find out the location id and departments name under that location where the state province is not null and location id is in between 1200 to 1400.
   > 4. Count the number of employees along with their total paid amount considering salary as yearly salary who are having salary greater than 5000 and working under a manager.



## Solution

### Q1

```sql
# Find out the country names 
# which is having “ae” or “ea” as last character 
# and located in “Europe” region.

SELECT ntb.country_name 
FROM 
(
    SELECT c.country_name 
    
    FROM countries as c
    INNER JOIN
    regions as r
    ON c.region_id = r.region_id 
    
    WHERE r.region_name = 'EUROPE'
) AS ntb

WHERE   ntb.country_name LIKE '%ae' OR
		ntb.country_name LIKE '%ea';

```


### Q2
```sql
# Find out the employee’s name 
# who are having job history experience of more than 8 years but not more than 10. 

SELECT first_name , DATEDIFF(CURRENT_DATE(), hire_date)/365 AS exp
FROM employees
WHERE DATEDIFF(CURRENT_DATE(), hire_date)/365 > 8 AND DATEDIFF(CURRENT_DATE(), hire_date)/365 < 10;



```


### Q3
```sql
# Find out the location id and departments name under that location 
# where the state province is not null and 
# location id is in between 1200 to 1400. 
 
    SELECT l.location_id, dept.department_name, l.street_address
    
    FROM locations as l 
    INNER JOIN 
    departments as dept 
    ON l.location_id = dept.location_id
    
    WHERE l.state_province is NOT NULL AND
    	  l.location_id BETWEEN 1200 AND   1400;

```
 


### Q4
```sql
# Count the number of employees 
# along with their total paid amount considering salary as yearly salary 
# who are having salary greater than 5000 and working under a manager. 

SELECT COUNT(*) AS num_of_emp, SUM(salary*12) AS Total_paid
FROM employees
WHERE salary > 5000 AND manager_id IS NOT NULL;

```
 