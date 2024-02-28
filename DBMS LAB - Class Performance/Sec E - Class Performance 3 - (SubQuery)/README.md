# Class Performance 03 (Spring 24 DBMS Lab Sec E CP 3) - Subqueries 

1. Insert this [(Link)](https://github.com/TashinParvez/MySQL_From_Zero/blob/Tashin/Files/hr_schema.sql) database on your system.
2. Perform the operations below
   
    Use Sub-query to answer

   > 1. Find out the name of the employees who are not having minimum or maximum salary and the salary is also not equal to the average salary and not having experience less than 5 years.
   > 2. Find out the country names along with their available region names where the location id is not the minimum one.
   > 3. Find out the employee average salary who is assigned to a manager position and salary is not less than the avg salary of all employees. [employees having “manager” at the last part of their job title name are considered as an employee of manager position.
   
	Answer this using any method of your choice. You can combine join and subquery as well.

  	 > 4. Sort the locations for each department in desc order and show the department names along with the minimal salary allocated for that department where the average of salaries are not starting with "2". 


## Solution

### Q1
```sql

# Find out the name of the employees who are not having minimum or maximum salary
# and the salary is also not equal to the average salary 
# and not having experience less than 5 years

SELECT first_name 
FROM (
    SELECT first_name, (YEAR(jh.start_date) - YEAR(jh.end_date)) AS EXPERIENCE , e.salary
    FROM employees as e  
    INNER JOIN 
    job_history as jh
    ON e.employee_id = jh.employee_id
    
    ) AS e 

WHERE salary != (  # AVG
    	SELECT AVG(salary)
    	FROM employees
      )
      AND 
      salary != (  # max
    	SELECT MAX(salary)
    	FROM employees
      )
      AND
      salary != (  # MIN
    	SELECT MIN(salary)
    	FROM employees
      )
      AND 
      e.EXPERIENCE < 5 ;
 
```


### Q2
```sql
 
# Find out the country names along with their available region names 
# where the location id is not the minimum one.

SELECT NC.country_name, r.region_name
FROM ( 
    SELECT c. country_name, c.region_id, l.location_id
    FROM countries as c
         INNER JOIN
         locations as l 
         ON c.country_id = l.country_id 
    WHERE l.location_id != (
   		 SELECT MIN(locations.location_id)
       FROM locations
    )
		) AS NC
    INNER JOIN 
    regions as r
    ON NC.region_id = r.region_id
     ;

```


### Q3
```sql
# employee average salary who is assigned to a
# manager position and salary is not less than the avg salary of all employees. 
# employees having “manager” at the last part of their
# job title name are considered as an employee of manager position.

SELECT AVG(salary)
FROM employees
WHERE salary >= (
    SELECT AVG(salary)
    FROM employees
    ) AND manager_id IS NOT NULL;


```



### Q4
```sql
# Sort the locations for each department in desc order and 
# show the department names along with the minimal salary allocated for that  department 
# where the average of salaries are not starting with "2"

SELECT d.location_id, d.department_name , NC.mnSalary 
FROM ( 
        SELECT AVG(salary), min(salary)  as mnSalary
        FROM employees as e 
        GROUP BY department_id
        HAVING AVG(salary) NOT LIKE '2%'  
    )AS NC
    INNER JOIN 
    departments as d 
ORDER BY d.location_id DESC;
```

