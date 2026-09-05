# Employee & Projects SQL Practice

This repository contains two datasets — `Employee.csv` and `Projects.csv` —
created for practicing SQL joins, aggregations, filtering, and views in
PostgreSQL (pgAdmin 4).

## Datasets

### Employee.csv
| Column       | Type          | Description                  |
|--------------|---------------|-------------------------------|
| EmployeeID   | BIGINT (PK)   | Unique employee identifier    |
| Name         | VARCHAR(100)  | Employee full name            |
| Department   | VARCHAR(50)   | HR / IT / Finance             |
| Salary       | BIGINT        | Annual salary                 |
| JoinDate     | DATE          | Date employee joined          |

### Projects.csv
| Column       | Type          | Description                  |
|--------------|---------------|-------------------------------|
| ProjectID    | BIGINT (PK)   | Unique project identifier     |
| ProjectName  | VARCHAR(100)  | Name of the project           |
| Department   | VARCHAR(50)   | Department the project belongs to |
| Budget       | NUMERIC       | Allocated project budget      |

> Note: both tables use quoted, case-sensitive column names in PostgreSQL
> (e.g. `"Name"`, `"Department"`), since they were imported directly from CSV
> with their original header casing preserved.

---

## Queries

### 1. List all employees in the IT department
```sql
SELECT "Name"
FROM public."Employee"
WHERE "Department" = 'IT'
ORDER BY "Name";
```

### 2. List employees who joined after a given date
```sql
SELECT "Name"
FROM public."Employee"
WHERE "JoinDate" > '2020-01-01'
ORDER BY "Name";
```

### 3. Calculate the total project budget for each department
```sql
SELECT "Department", SUM("Budget") AS total_project_budget
FROM public."Projects"
GROUP BY "Department"
ORDER BY total_project_budget DESC;
```

### 4. Find employees earning above the company average salary
```sql
SELECT *
FROM public."Employee"
WHERE "Salary" > (SELECT AVG("Salary") FROM public."Employee");
```

### 5. Display employees and their project details (joined by department)
```sql
SELECT e."Name", e."Department", p."ProjectName", p."Budget"
FROM public."Employee" e
LEFT JOIN public."Projects" p ON e."Department" = p."Department"
ORDER BY e."Name";
```

### 6. Create a view of high-earning employees (salary > 60000)
```sql
CREATE VIEW "HighEarners" AS
SELECT * FROM public."Employee" WHERE "Salary" > 60000;
```

To query the view:
```sql
SELECT * FROM "HighEarners" ORDER BY "Salary" DESC;
```

---

## Tools Used
- PostgreSQL 18
- pgAdmin 4
