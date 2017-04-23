# Joined Tables in SQL

## Joined Table
  * Permits users to specify a table resulting a join operation in the `FROM` clause of a query
- The FROM clause in the query below contains a single join table
```sql
SELECT Fname,Lname,Address
FROM (EMPLOYEE Join DEPARTMENT ON Dno=Dnumber)
WHERE Dname='Research'
```

## Natural Joins on two relations R and S

- No join condition specified here
- Implicit EQUIJOIN condition for each pair of attributes with same name from R and S

```sql
SELECT Fname,Lname,Address
FROM (EMPLOYEE NATURAL JOIN DEPARTMENT AS DEPT (Dname, Dno, Mssn, Msdate))
WHERE Dname='Research';
#IMPLIED JOIN CONDITION IS EMPLOYEE.Dno = DEPT.Dno
```
## Inner Join

- Default type of join in a joined table
- Tuple is included in the result only if a matching tuple exists in the other relation

```sql
#Gets all the Employees Last Names and their supervisors last name if they have a supervisor
SELECT E.Lname AS Employee_name, S.Lname AS Supervisor_name
FROM EMPLOYEE AS E, EMPLOYEE AS S
WHERE E.Super_ssn=S.Ssn;
```

### Example:

**Department Table**

| DeptNo | DeptName | MgrNo |
|--------|----------|-------|
| A00    | Spiffy Computer Service Div. | 000010 |
| B01    | Planning | 000020|
| C01    | Information Center | 000030 |
| D01 | Development Center | N/A |

**Employee Table**

| EmpNo | LastName | WorkDept |
|--------|----------|-------|
| 000010 | Hass | A00 |
| 000020 | Thompson | B01|
| 000030 | Kwan | C01 |
| 000110 | Lucchessi | A00 |
| 000120 | O'Connell | A00 |
| 000130 | Quintana | C01 |

**Project Table**

| ProjNo | ProjName | DeptNo | RespEmp |
|--------|----------|-------|----------|
| AD3100 | Admin Services | D01 | 000010 |
| IF1000 | Query Services | C01 | 000030 |
| IF2000 | User Education | E01 | 000030 |
| MA2100 | Weld Line Automation | D01 | 000010 |
| PL2100 | Weld Line Planning | B01 | 000020 |

#### Query:

```sql
SELECT ProjNo, ProjName, P.DeptNo, D.DeptNo, DeptName
FROM PROJECT P INNER JOIN DEPARTMENT D
ON P.DEPTNO = D.DEPTNO
```

**Result:**

| ProjNo | ProjName | P.DeptNo | D.DeptNo | DeptName |
|--------|----------|----------|----------|----------|
| AD3100 | Admin Services | D01 | D01 | Development Center |
| IF1000 | Query Services | C01 | C01 | Information Center |
| MA2100 | Weld Line Automation | D01 | D01 | Development Center |
| PL2100 | Weld Line Planning | B01 | B01 | Planning |


## Left Outer Join

- Every tuple in the left table must appear in the result
- If no matching tuple
    * Padded with NULL values for attributes of right table
    * Example:
```sql
#Lists all employees last names along with their supervisors last names
SELECT E.Lname AS Employee_name,
       S.Lname AS Supervisor_name
FROM (EMPLOYEE AS E LEFT OUTER JOIN EMPLOYEE AS S ON E.Super_ssn=S.ssn);
```

### Example with Previous Database

**Department Table**

| DeptNo | DeptName | MgrNo |
|--------|----------|-------|
| A00    | Spiffy Computer Service Div. | 000010 |
| B01    | Planning | 000020|
| C01    | Information Center | 000030 |
| D01 | Development Center | N/A |

**Project Table**

| ProjNo | ProjName | DeptNo | RespEmp |
|--------|----------|-------|----------|
| AD3100 | Admin Services | D01 | 000010 |
| IF1000 | Query Services | C01 | 000030 |
| IF2000 | User Education | E01 | 000030 |
| MA2100 | Weld Line Automation | D01 | 000010 |
| PL2100 | Weld Line Planning | B01 | 000020 |


#### Query:

```sql
SELECT ProjNo, ProjName, P.DeptNo, D.DeptNo, DeptName
FROM PROJECT P LEFT OUTER JOIN DEPARTMENT D
ON P.DEPTNO = D.DEPTNO
```


#### Result:
**Notice the third tuple in this result**

| ProjNo | ProjName | P.DeptNo | D.DeptNo | DeptName |
|--------|----------|----------|----------|----------|
| AD3100 | Admin Services | D01 | D01 | Development Center |
| IF1000 | Query Services | C01 | C01 | Information Center |
| IF2000 | User Education | E01 | NULL | NULL |
| MA2100 | Weld Line Automation | D01 | D01 | Development Center |
| PL2100 | Weld Line Planning | B01 | B01 | Planning |


## Right Outer Join

- Same as Left Outer Join Except Every Tuple in the right table must appear in the result
- Again, if there is no matching tuple, the result will be padded with NULL values for the attributes of the left table


### Example with Previous Database

**Department Table**

| DeptNo | DeptName | MgrNo |
|--------|----------|-------|
| A00    | Spiffy Computer Service Div. | 000010 |
| B01    | Planning | 000020|
| C01    | Information Center | 000030 |
| D01 | Development Center | N/A |

**Project Table**

| ProjNo | ProjName | DeptNo | RespEmp |
|--------|----------|-------|----------|
| AD3100 | Admin Services | D01 | 000010 |
| IF1000 | Query Services | C01 | 000030 |
| IF2000 | User Education | E01 | 000030 |
| MA2100 | Weld Line Automation | D01 | 000010 |
| PL2100 | Weld Line Planning | B01 | 000020 |


#### Query:

```sql
SELECT ProjNo, ProjName, P.DeptNo, D.DeptNo, DeptName
FROM PROJECT P RIGHT OUTER JOIN DEPARTMENT D
ON P.DEPTNO = D.DEPTNO
```


#### Result:
**Notice the First tuple in this result**

| ProjNo | ProjName | P.DeptNo | D.DeptNo | DeptName |
|--------|----------|----------|----------|----------|
| NULL | NULL | NULL | A00 | Spiffy Computer Service Div. |
| PL2100 | Weld Line Planning | B01 | B01 | Planning |
| IF1000 | Query Services | C01 | C01 | Information Center |
| AD3100 | Admin Services | D01 | D01 | Development Center |
| MA2100 | Weld Line Automation | D01 | D01 | Development Center |

## Full Outer Join

- Combines the effect of applying both left and right joins
- NULL values in every column of the table without a matching row
  * Example with Company Database:

  ```sql
  SELECT *
  FROM employee FULL OUTER JOIN department
    ON employee.Dno = department.Dnumber
  ```

### Example with other database

#### Query:

```sql
SELECT ProjNo, ProjName, P.DeptNo, D.DeptNo, DeptName
FROM Project P FULL OUTER JOIN DEPARTMENT D
ON P.DeptNo=D.DeptNo
```

#### Result:

| ProjNo | ProjName | P.DeptNo | D.DeptNo | DeptName |
|--------|----------|----------|----------|----------|
| NULL | NULL | NULL | A00 | Spiffy Computer Service Div. |
| PL2100 | Weld Line Planning | B01 | B01 | Planning |
| IF1000 | Query Services | C01 | C01 | Information Center |
| AD3100 | Admin Services | D01 | D01 | Development Center |
| MA2100 | Weld Line Automation | D01 | D01 | Development Center |
| IF2000 | User Education | E01 | NULL | NULL|

## Cross Join (Cartesian Product)

- TO FILL OUT


## Nested Joins

- Joins can be nested as well

Gets the project number, department number, the last name of the Manager who's department is on those projects, the address of the manager, and the birth date of the manager who's department is in charge of a project located in Stafford.

```sql
SELECT Pnumber, Dnum, Lname, Address, Bdate
FROM ((Project JOIN Department ON Dnum=Dnumber)
      JOIN EMPLOYEE ON Mgr_ssn=Ssn)
WHERE Plocation = 'Stafford';
```


# Aggregate Functions in SQL

- Used to summerize information from multiple tuples into a single-tuple summary
- **Grouping**
  * Create subgroups of tuples before summarizing
- Built in Aggregate Functions
  * **Count**, **SUM**, **MAX**, **MIN**, and **AVG**
- Functions can be used in the *SELECT* clause  or in a *HAVING* clause
- NULL values are discarded when aggregate function are applied
- **Examples:**

*Q20: Gets the sum, max, min, and average salary for employees in the research department*

```sql
SELECT SUM(Salary), MAX(Salary), MIN(Salary), AVG(Salary)
FROM (EMPLOYEE JOIN DEPARTMENT ON Dno=Dnumber)
WHERE Dname='Research';
```

*Q21: Gets the amount of employees in total*

```sql
SELECT COUNT(*)
FROM EMPLOYEE;
```

*Q22: Same as Q20 except it gets the total number of employees in the research department*

```sql
SELECT COUNT(*)
FROM EMPLOYEE, DEPARTMENT
WHERE Dno=Dnumber AND Dname='Research';
```

# Grouping

