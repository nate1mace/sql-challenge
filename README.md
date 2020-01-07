# sql-challenge
```
create table Employee (
  emp_no Serial primary key,
  birth_date date,
  first_name varchar,
  last_name varchar,
  gender varchar,
  hire_date date
);

create table Salaries (
	emp_no integer REFERENCES Employee(emp_no),
	salary money,
	from_date date,
	to_date date);

create table Departments (
	dept_no varchar primary key,
	dept_name varchar
);

create table Dept_mang (
	dept_no varchar references Departments(dept_no),
	emp_no int references Employee(emp_no),
	from_date date,
	to_date date
);

create table Dept_emp (
	emp_no int references Employee(emp_no),
	dept_no varchar references Departments(dept_no),
	from_date date,
	to_date date
);

create table Titles (
	emp_no int references Employee(emp_no),
	title varchar,
	from_date date,
	to_date date
);

--List the following details of each employee: employee number, last name, first name, gender, and salary.

select e.emp_no, e.last_name, e.first_name, e.gender, s.salary
from Employee e
left join Salaries s
on e.emp_no = s.emp_no

--List employees who were hired in 1986.
select first_name, last_name
from Employee
where hire_date >= '1986-01-01' and hire_date <= '1986-12-31';

---List the manager of each department with the following information:
---department number, department name, the manager's employee number, last name, first name, and start and end employment dates.
select dm.dept_no, d.dept_name, dm.from_date, dm.to_date, e.last_name, e.first_name
from Dept_mang dm
left join Departments d
on dm.dept_no = d.dept_no
left join Employee e
on e.emp_no = dm.emp_no

---List the department of each employee with the following information:
---employee number, last name, first name, and department name.
select d.dept_name, e.emp_no, e.last_name, e.first_name
from Employee e
left join Dept_emp de
on e.emp_no = de.emp_no
left join Departments d
on d.dept_no = de.dept_no

---List all employees whose first name is "Hercules" and last names begin with "B."
select first_name, last_name
from Employee
where first_name = 'Hercules' and last_name like 'B%'
---List all employees in the Sales department
--including their employee number, last name, first name, and department name.
select d.dept_name, e.emp_no, e.last_name, e.first_name
from Employee e
left join Dept_emp de
on e.emp_no = de.emp_no
left join Departments d
on d.dept_no = de.dept_no
where dept_name = 'Sales'

---List all employees in the Sales and Development departments,
--including their employee number, last name, first name, and department name.
select d.dept_name, e.emp_no, e.last_name, e.first_name
from Employee e
left join Dept_emp de
on e.emp_no = de.emp_no
left join Departments d
on d.dept_no = de.dept_no
where dept_name = 'Sales' or dept_name = 'Development'

---In descending order, list the frequency count of employee last names
--i.e., how many employees share each last name.
select last_name, count(last_name) as "Num_employees_with_last_name"
from employee
group by last_name
order by "Num_employees_with_last_name" desc;

```
