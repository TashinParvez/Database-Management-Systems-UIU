# Database creation_practice [SEC - A]


![](https://github.com/TashinParvez/Database-Management-Systems-UIU/blob/main/DBMS%20LAB%20-%20Class%20Performance/Sec%20A%20%5BDatabase%20Creation%20Practice%5D/DATABASE_creation_practice.png)

## Solution

###
```sql
CREATE TABLE department(
    department_id INT PRIMARY KEY
);


CREATE TABLE student(
    student_id INT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    major VARCHAR(100)
);


CREATE TABLE PROFESSOR_id(
    prof_id INT PRIMARY KEY,
    prof_name VARCHAR(100),
    dept_id INT,
    
    CONSTRAINT fk_dept_id 
    FOREIGN key(dept_id) REFERENCES department(department_id)
);


CREATE TABLE course(
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100),
    professor_id INT,
    
    CONSTRAINT fk_prof_ID FOREIGN KEY(professor_id) REFERENCES professor(prof_id)
);


CREATE TABLE enrollment(
    stu INT,
    cou INT,
    Ga VARCHAR(2),
    
    CONSTRAINT fk_stu_id FOREIGN KEY(stu) REFERENCES student(student_id),
    CONSTRAINT fk_course_Code FOREIGN KEY(cou) REFERENCES course(course_id)
);
```
