# CP 1 Spring 24 DBMS Lab Sec A

1. Insert this [(Link)](https://github.com/TashinParvez/MySQL_From_Zero/blob/Tashin/Files/hr_schema.sql) database on your system.
2. Perform the operations below 

   > 1. Find out the 4th maximum paid salary if we increment the saalary to 5%. [employees]
   > 2. Find out the departments details where there is no manager assigned. [departments]
   > 3. Find out the location id, postal code and street address where the state province is not available and street address is not ending with any even number. [locations]
   > 4. Find out the country names where country id is not having any vowel in it and region id is an even number. [countries]
   > 5.  Find out the employee id who has a previous work experience of more than 3 years but not accounts department. [job id having "AC" in the begining and not having any vowel in the rest of the job id name is considered as accounts department] [use job hitory table]


## Solution

### Q1
```sql
# Find out the 4th maximum paid salary if we increment the saalary to 5%. [employees]

SELECT DISTINCT salary*(5/100) AS Inc_salary
FROM employees
ORDER BY Inc_salary DESC
LIMIT  1
OFFSET 3;

```
 


### Q2
```sql 
# Find out the departments details where there is no manager assigned. 

SELECT *
FROM departments
WHERE manager_id IS NULL;

```


### Q3
```sql 

# Find out the location id, postal code and street address 
# where the state province is not available and 
# street address is not ending with any even number. [locations] 


SELECT location_id, postal_code, street_address
FROM locations
WHERE state_province IS NULL AND 
      RIGHT(street_address,1) NOT IN ('2','4','6','8','0');

```



### Q4
```sql 

# Find out the country names 
# where country id is not having any vowel in it and 
# region id is an even number. [countries]


SELECT country_name

FROM countries
WHERE   country_id NOT LIKE '%a%' AND
		country_id NOT LIKE '%e%' AND
        country_id NOT LIKE '%i%' AND
        country_id NOT LIKE '%o%' AND
        country_id NOT LIKE '%u%' 
        AND
        region_id%2 = 0

ORDER BY country_name ASC;
```


### Q5
```sql
# Find out the employee id 
# who has a previous work experience of more than 3 years 
# but not in accounts department.


SELECT ntb.employee_id

FROM (
SELECT employee_id, DATEDIFF(CURDATE(), hire_date) / 365 AS experience
    FROM employees as e 
    INNER JOIN
    departments as dept 
    ON dept.department_id = e.department_id
    
    WHERE dept.department_name != 'Accounting'
) AS ntb

WHERE ntb.experience >3; 

```

