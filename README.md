# SQLite Views

This example demonstrates the use of views on the following database:
* department(<ins>dept_id</ins>, dept_name)
* employee(<ins>emp_id</ins>, first_name, last_name, salary, dept_id)

The data set is in [the script file](script.txt) and the code examples can be run on https://sqliteonline.com/

* create an employee view that hides salary information
```sql
CREATE VIEW employee_view
AS
SELECT emp_id, first_name, last_name
FROM employee
ORDER BY first_name,last_name;
```

* insert an employee through the employee view using triggers
```sql
CREATE TRIGGER trigger_view_insert
INSTEAD OF INSERT ON employee_view 
BEGIN
  INSERT INTO employee(emp_id,first_name,last_name) 
  SELECT new.employee_id, new.first_name,new.last_name; 
END;

INSERT INTO employee_view(emp_id,first_name,last_name) 
VALUES (10,'Andrai','Marku');
```

* delete an employee through the employee view using triggers
```sql
CREATE TRIGGER trigger_view_delete
INSTEAD OF DELETE ON employee_view
BEGIN 
    DELETE FROM employee
    WHERE emp_id = old.emp_id; 
END;

DELETE FROM employee_view 
WHERE emp_id = 9; 
```