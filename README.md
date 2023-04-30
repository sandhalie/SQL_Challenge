# SQL_Challenge

## SQL Challenge: Employee Database
This project involves designing and implementing a SQL database to store data on employees of Pewlett Hackard, a fictional company, who were employed during the 1980s and 1990s. The data is provided in six CSV files, which will be imported into the SQL database.

## Project Description
The project is divided into three parts: data modelling, data engineering, and data analysis.

### Data Modelling
Inspect the CSV files, and then sketch an ERD of the tables.

/EmployeeSQL/ERD_diagram.png
![Employees_ERD](EmployeeSQL/ERD_diagram.png)

### Data Engineering
Create tables in the database
```pgsql
CREATE TABLE titles (
	title_id varchar(5) PRIMARY KEY,
	title varchar(30)
);

CREATE TABLE employees (
	emp_no varchar(10) PRIMARY KEY,
	emp_title_id varchar(5) references titles(title_id),
	birth_date date,
	first_name varchar(45),
	last_name varchar(45),
	sex varchar(1),
	hire_date date
);

CREATE TABLE departments (
	dept_no varchar(5) PRIMARY KEY,
	dept_name varchar(30)
);

CREATE TABLE salaries (
	emp_no varchar(10) references employees(emp_no),
	salary int
);
	
CREATE TABLE dept_emp (
	emp_no varchar(10) references employees(emp_no),
	dept_no varchar(5) references departments(dept_no)
);

CREATE TABLE dept_manager (
	dept_no varchar(5) references departments(dept_no),
	emp_no varchar(10)
);
```

### Data Analysis
Perform the following queries on the SQL database:

List the employee number, last name, first name, sex, and salary of each employee.
```pgsql
select t1.emp_no,t1.last_name,t1.first_name,t1.sex,t2.salary
from employees as t1
left outer join salaries as t2 
on t1.emp_no = t2.emp_no;
```
List the first name, last name, and hire date for the employees who were hired in 1986.
```pgsql
select t1.emp_no,t1.last_name,t1.hire_date
from employees as t1
where hire_date between '1986-01-01' and '1986-12-31';
```
List the manager of each department along with their department number, department name, employee number, last name, and first name.
```pgsql
select t1.*, t2.emp_no,t3.last_name, t3.first_name 
from departments t1
left outer join dept_manager t2 on t1.dept_no = t2.dept_no
left outer join employees t3 on t2.emp_no = t3.emp_no;
```
List the department number for each employee along with that employee’s employee number, last name, first name, and department name.
```pgsql
select t1.dept_no, t2.emp_no,t3.last_name, t3.first_name, t1.dept_name 
from departments t1
left outer join dept_emp t2 on t1.dept_no = t2.dept_no
left outer join employees t3 on t2.emp_no = t3.emp_no;
```
List the first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.
```pgsql
select t1.first_name, t1.last_name, t1.sex
from employees t1
where t1.first_name = 'Hercules'
and t1.last_name like 'B%';
```
List each employee in the Sales department, including their employee number, last name, and first name.
```pgsql
select t2.emp_no,t3.last_name, t3.first_name 
from departments t1
left outer join dept_emp t2 on t1.dept_no = t2.dept_no
left outer join employees t3 on t2.emp_no = t3.emp_no
where t1.dept_name = 'Sales';
```
List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.
```pgsql
select t2.emp_no,t3.last_name, t3.first_name,t1.dept_name
from departments t1
left outer join dept_emp t2 on t1.dept_no = t2.dept_no
left outer join employees t3 on t2.emp_no = t3.emp_no
where t1.dept_name in ('Sales','Development');
```
List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).
```pgsql
select last_name, count(last_name)
from employees
group by last_name
order by 2 desc;
```
