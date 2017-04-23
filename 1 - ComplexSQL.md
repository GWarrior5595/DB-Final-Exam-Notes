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
