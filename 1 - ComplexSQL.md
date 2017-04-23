Complex SQL (March 20th - 22nd)

# Comparisons Involving NULL and Three-Valued Logic

- Meanings of NULL
  * **Unknown value**
    * Date of Birth is Unknown
  * **Unavailable or withheld value**
    * Home phone number does not want it listed
  * **Not applicable attribute**
    * Attribute LastCollegeDegree is NULL for someone without a college degree
- Each individual NULL value is considered to be different from every other NULL value
- SQL uses a three-valued logic:
  * `TRUE`, `FALSE`, and `UNKNOWN`
  
- SQL allows queries that check whether an attribute value is NULL
  * `IS` or `IS NOT NULL`

### Example

*Q18: Retrieve the names of all employees who do not have supervisors*

```sql
SELECT Fname, Lname
FROM EMPLOYEE
WHERE Super_ssn IS NULL;
```

# Complex SQL Retrieval Queries

- Additional features allow users to specify more complex retrievals from the database:
  * Nested Queries
  * Joined Tables
  * Outer Joins
  * Aggregate functions
  * Grouping


# Nested Queries

- **Nested queries**
  * Complete select-from-where blocks within `WHERE` clause of another query
  * Also known as an **Outer query**
- Comparison operator `IN`
  * Compares value *v* with a set(or multiset) of values *V*
  * Evaluates `TRUE` if *v* is one of the elements in *V*

### Example

*Explanation: Gets all the project numbers in which are under the department managed by an employee with the last name of Smith or the project number in which employees with the last name Smith are working on.*
```sql
SELECT DISTINCT Pnumber
FROM PROJECT
WHERE Pnumber IN
      (SELECT Pnumber
       FROM PROJECT, DEPARTMENT, EMPLOYEE
       WHERE Dnum=Dnumber AND Mgr_ssn=Ssn
       AND Lname='Smith')
OR
Pnumber IN 
(
  SELECT Pno
  FROM WORKS_ON,EMPLOYEE
  WHERE Essn=Ssn AND Lname='Smith');
);
```

- You can use tuple values in parentheses along with Nested Queries

### Example

*Explanation: If there is a tuple that has the SSN of the employee within the WORKS_ON table it will retrieve an SSN of 123456789*
```sql
SELECT DISTINCT Essn
FROM WORKS_ON
WHERE (Pno,Hours) IN (SELECT Pno,Hours
                      FROM WORKS_ON
                      WHERE Essn='123456789');
```

## Other Comparison Operators

- Other Comparison Operators to compare a single value v
  * `ANY` (or = `SOME`) operator
    *  Returns `TRUE` if the value *v* is equal to some value in set *V*, hence it is equivalent to `IN`
  * Other operators can be combined with `ANY`(or `SOME`): `>`(Greater Than), `>=`(Greater than or equal), `<`(Less Than), `<=`(Less than or equal), and `<>`(not equal)
  * The Keyword `ALL` can be combined with these operators
      * `ALL` returns `TRUE` if `ALL` of the sub-query values meet this condition

### Example

*Explanation: Selects the Employees who's salary is greater than the salaries of the employees in Department 5*

```sql
SELECT Lname, Fname
FROM EMPLOYEE
WHERE Salary > ALL (SELECT SALARY FROM EMPLOYEE WHERE Dno=5)
```

## Creating Tuple Variables

- To avoid potential errors and ambiguities
  * Create tuple variables (aliases) for all tables referenced in SQL query

### Example

```sql
SELECT E.Fname, E.Lname
FROM EMPLOYEE AS E
WHERE E.Ssn IN (SELECT Essn
                FROM Dependent AS D
                WHERE E.Fname = D.Dependent_name AND E.Sex=D.Sex);
```

## Correlated Nested Queries

- Two queries are said to be correlated
    *  Whenever a condition in the WHERE clause of a nested query references some attribute of a relation declared in the outer query
    * Evaluated once for each tuple in the outer query
    * Like in the example about with tuple variables: For each EMPLOYEE tuple, evaluate the nested query; If the SSN value of the EMPLOYEE tuple is in the result of the nested query, then SELECT that EMPLOYEE tuple


## EXISTS Function in SQL

- `EXISTS` function
    * Check whether the result of a correlated nested query is empty or not

## NOT EXISTS Function in SQL

- `NOT EXISTS`
    * Typically used in conjunction with any correlated nested query

## UNIQUE Function in SQL

- SQL function `UNIQUE(Q)`
    * Returns `TRUE` if there are no duplicate tuples in the result of query *Q*
    * Use it to test whether the result of a nested query is a set or a multi-set


## Explicit Sets & Renaming of Attributes

- Can use explicit set of values in `WHERE` clause instead of a nested query

```sql
SELECT DISTINCT Essn
FROM WORKS_ON
WHERE Pno IN (1,2,3)
```

- Use quantifier `AS` followed by the desired new name
    * Rename any attribute that appears in the result of a query

### Example

```sql
SELECT E.Lname AS Employee_name, S.Lname AS Supervisor_name
FROM EMPLOYEE AS E, EMPLOYEE AS S
WHERE E.Super_ssn=S.Ssn
```


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

- `CROSS JOIN`
  * To Retrieve a list of names of each female employee's dependents
  * EMP_DEPENDENTS = EMPNAMES X Depenent

```sql
SELECT * FROM EMPNAME CROSS JOIN DEPENDENT
```


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

# Grouping: GROUP BY and Having Clauses

- **Partition** relation into subsets of tuples
  * Based on **grouping attribute(s)**
  * Apply function to each such group independently
- **GROUP BY** clause
  * Specifies grouping attributes
- If NULLs exist in grouping attribute
  * Separate group created for all tuples with a NULL value in grouping attribute
- **HAVING** clause
  * Provides a condition on the summary information

#### Example:

*Q28: For each department that has more than five employees, retrieve the department number and the number of its employees who are making more the $40,000.*
```sql
SELECT Dnumber, COUNT(*)
FROM DEPARTMENT, EMPLOYEE
WHERE Dnumber=Dno AND SALARY>40000 AND
      (SELECT Dno
       FROM EMPLOYEE
       GROUP BY Dno
       HAVING COUNT(*) > 5);
```


# SQL Query Format Summary

```sql
SELECT <attribute function list>
FROM <table list>
WHERE <condition>
GROUP BY <grouping attribute(s)>
HAVING <group condition>
ORDER BY <attribute list>;
```


# Constraints as Assertions

- **CREATE ASSERTION**
  * Specify additional types of constraints outside scope of built-in relational model constraints

## Specifying General Constraints as Assertions

- *CREATE ASSERTION*
  * Specify a query that selects any tuples that violate the desired condition
  * Use only in cases where its not possible to use *CHECK* on attributes and domains

### Example

*This creates and assertion where it checks if there does not exist an employee who's salary is greater than a managers salary*
```sql
#THIS WILL NOT WORK IN MYSQL, Maria DB has a CREATE ASSERTIONS function
CREATE ASSERTION SALARY_CONSTRAINT
CHECK (NOT EXISTS (SELECT *
                   FROM EMPLOYEE E, EMPLOYEE M,
                        DEPARTMENT D
                   WHERE E.Salary>M.Salary
                         AND E.Dno=D.Dnumber
                         AND D.Mgr_ssn=M.Ssn));
```

# Actions as Triggers

- **CREATE TRIGGER**
  * Specify automatic actions that database system will perform when certain events occur

## Triggers in SQL

- *CREATE TRIGGER* statement
  * Used to maintain database consistency, monitor database updates, auto-update derived data
- Triggers Consist of three parts:
  * **Event(s)**
  * **Condition**
  * **Action**

### Example

```sql
CREATE TRIGGER SALARY_VIOLATION
  BEFORE INSERT OR UPDATE OF SALARY, SUPERVISOR_SSN
    ON EMPLOYEE
FOR EACH ROW
  WHEN(NEW.SALARY > (SELECT SALARY FROM EMPLOYEE
                     WHERE SSN=NEW.SUPERVISOR_SSN))
                     INFORM_SUPERVISOR(NEW.Supervisor_ssn, NEW.Ssn);
```

# Views (Virtual Tables) in SQL

- Concept of a view in SQL
  * Single table derived from other tables
  * Considered to be a virtual table

## Specification of Views in SQL

- **CREATE VIEW** command
  * Give table name, list of attribute names, and a query to specify the contents of the view

### Example

*V1:Creates a view in which retrieves all employee's first names, last names, the project names in which they are working on, and the amount of hours they have worked on that project*

```sql
CREATE VIEW WORKS_ON1
AS SELECT E.Fname, E.Lname, P.Pname, W.Hours
   FROM EMPLOYEE E, PROJECT P, WORKS_ON W
   WHERE E.Ssn=W.Essn AND W.Pno=P.Pnumber;
```


*V2:It creates a view where for each department, it retrieves the departments name, the number of employees it has, and the sum of the salary's each employee has in each department.*

```sql
CREATE VIEW DEPT_INFO(Dept_name, No_of_emps, Total_sal)
AS SELECT D.Dname, COUNT(*), SUM(Salary)
   FROM DEPARTMENT D, EMPLOYEE E
   WHERE D.Dnumber=E.Dno
   GROUP BY Dname;
```

## Specifications of a View in SQL

- Specify the queries on a view
- View will always up-to-date
  * Responsibility of the DBMS and not the user
- **DROP VIEW** command
  * Dispose of a view


## View Implementation

- Complex problem of efficiently implementing a view for querying
- **Query Modification** approach
  * Modify view query into a query on underlying base tables
  * Disadvantage: inefficient for views defined via complex queries that are time-consuming to execute
- **View Materialization Approach**
  * Physically create a temporary view table when the view is first queried
  * Keep that table on the assumption that other queries on the view will follow
  * Requires efficient strategy for automatically updating the view table when the base tables are updated
- **Incremental Update Strategies**
  *DBMS determines what new tuples must be inserted, deleted, or modified in a materialized view table

# Schema Change Statements

- Can be done while the database is operational
- Does not require recompilation of the database schema

## DROP Command

- DROP command
  * Used to drop named schema elements, such as tables, domains, or constraint
- Drop behavior options:
  * `CASCADE` and `RESTRICT`
- **Example:** `DROP SCHEMA COMPANY CASCADE;`

## ALTER Command

- **Alter table** actions include:
  * Adding or dropping a column (attribute)
  * Changing a column definition
  * Adding or dropping table constraints
- **Example:** `ALTER TABLE COMPANY.EMPLOYEE ADD COLUMN Job VARCHAR(12);`
- To drop a column
  * Choose either `CASCADE` or `RESTRICT`
- To change constraints specified on a table
  * Add or drop a named constraint
  ```sql
  ALTER TABLE Company.Employee
  DROP CONSTRAINT EMPSUPERFK CASCADE;
  ```