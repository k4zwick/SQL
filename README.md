184. Department Highest Salary

The Employee table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
The Department table holds all departments of the company.

+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, Max has the highest salary in the IT department and Henry has the highest salary in the Sales department.

+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+


# Write your MySQL query statement below


Method 1:
SELECT Department.Name AS Department, Employee.Name AS Employee, Max(Salary) OVER (PARTITION BY Department ORDER BY Salary) AS Salary
FROM Employee JOIN Department ON Employee.DepartmentId = Department.Id

Method 2:
SELECT L1.Department, L1.Employee, L1.Salary
FROM
(SELECT Department.Name AS Department, Employee.Name AS Employee, Salary
FROM Employee JOIN Department ON Employee.DepartmentId = Department.Id) AS L1,
(SELECT Department.Name AS Department, Employee.Name AS Employee, Salary
FROM Employee JOIN Department ON Employee.DepartmentId = Department.Id) AS L2
WHERE L1.Salary > L2.Salary 
AND L1.Department = L2.Department

Method 3:
SELECT Department.Name AS Department, e1.Name AS Employee, e1.Salary AS Salary
FROM Department JOIN Employee e1 ON Department.Id = e1.DepartmentId
WHERE e1.Salary = (SELECT Max(Salary) FROM Employee e2 WHERE e2.DepartmentId = e1.DepartmentId)
