# Introduction to Database Security Issues

  - Types of Security
    * Legal and ethical issues
    * Policy issues
      * Governmental, institutional, corporate levels
    * System-related issues
      * Physical hardware, OS or DBMS levels
    * The need to identify multiple security levels

# Database Security Issues

## Database Security Issues
  
  - Threats to databases
    * Loss of **integrity**
      * Information needs to be protected from improper modification
      * Unauthorized changes
    * Loss of **availability**
      * Database needs to be available to legitimate, authorized users
    * Loss of **confidentiality**
      * Protecting data from unauthorized disclosure
      * Unauthorized, unanticipated, or unintentional disclosure

  - To protect databases against these types of threats four kinds of counter measures can be implemented:
    * Access control
    * Inference control
    * Flow control
    * Encryption

 - Provisions for restricting access to the database as a whole
   * Called **access control**
   * Handled by creating user accounts and passwords to control login process by the DBMS

## Statistical Database Security

  - **Statistical Databases** are used mainly to produce statistics on various populations
  - The database may contain **confidential data** on individuals, which should be protected from user access.
  - Users are permitted to retrieve **statistical information** on the populations, such as **averages, sums, counts, maximums, minimums, and standard deviations**
  - May want to retrieve the *number* of individuals in a **population** or the **average income** in the population
    * However, statistical users are not allowed to retrieve individual data, such as the income of a specific person.
  - Statistical database security techniques must prohibit the retrieval of individual data.
  - Prohibiting queries that retrieve attribute values and by allowing only queries that involve statistical aggregate functions such as COUNT, SUM, MIN, MAX, AVERAGE, and STANDARD DEVIATION
    * **Statistical queries**
  - Controlling the access to **statistical databases**
    * used to provide statistical information or summaries of values based on various criteria
    * The countermeasures to **Statistical database security** problem is called **inference control measures**
      * Disallow statistical queries when the number of tuples in a population is below a threshold
      * Introduce *<b>noise</b>* into the results
      * Partitioning the database - records stored in groups of minimum size
        * Allow queries only on complete groups or sets of groups; never on subsets within a group.

## Database Security Issues

  - **Flow Control**
    * prevents information from flowing in such a way that it reaches unauthorized users
  - **Covert Channels**
    * Channels that are pathways for information to flow implicitly in ways that violate the security policy of an organization

## Covert Channels

  - **Covert channels** are classified into two broad categories:
    * **Storage Channel**: do not require any temporal synchronization
      * Information is conveyed by accessing system information or what is otherwise inaccessible to the user
    * **Timing Channel**
      * Allow the information to be conveyed by the timing of events or processes
  - How to avoid covert channels?
    * programmers do not have access to sensitive data that a program is supposed to process after the program has been put into operation

## Database Security Issues

  - **Data encryption**
    * to protect sensitive data that is being transmitted via communication network
  - Data **encoded** using some **encoding algorithm**
    * An unauthorized user who access encoded data will have difficulty deciphering it, but authorized users are given decoding or decrypting algorithms(or keys) to decipher data

## Database Security and the DBA

  - The database administration (**DBA**) is the cnetral authority for managing a database system
    * The DBA's responsibilities include
      * granting privileges to users who need to use the system
      * classifying users and data in accordance with the policy of the organization
  - The DBA is responsible for the overall security of the database system

## Access Protection, User Accounts, Database Access

  - The database system must also keep **track of all operations** on the database
    * applied by a certain user throughout each **login session**
  - If any tampering with the database is suspected, a **database audit** is performed
  - A database log used mainly for security purposes - **audit trail**

## Database Security Issues

  - A DBMS typically includes a database security and authorization subsystem
    * Responsible for ensuring the security portions of the database against unauthorized access
  - Two types of database security mechanisms:
    * Discretionary
    * Mandatory

## Discretionary Access Control

  - **Discretionary access control techniques**
    * **Granting** and **revoking** privileges on relations
    * Traditionally the main security mechanism for a relational database system
    * All-or-nothing method
  - **Account Level**:
    * At this level, the DBA specifies the particular privileges that each account holds independently of the relations in the database
  - **Relation level** ( or **table level**):
    * At this level, the DBA can control the privilege to access each individual relation or view in the database

## Mandatory Access Control

  - **Additional security policy** classifies data and users based on security classes
    * **Mandatory access control**, typically can be **combined** with the discretionary access control mechanisms
  - Typical **security classes**:
    * Top Secret (TS)
    * Secret (S)
    * Confidential (C)
    * Unclassified (U)
  - where TS is the highest level and U is the lowest: TS >= S >= C => U

## Role-Based Access Control

  - **Role-based access control (RBAC)** emerged rapidly in the 1990s
    * proven technology for managing the enforcing security in large-scale enterprise-wise systems
  - Permissions are associated with rols, and users are assigned to appropriate roles
  - Roles can be created using the **CREATE ROLE** and **DESTROY ROLE** commands
    * The **GRANT** and **REVOKE** commands can be used to assign and revoke privileges from roles
  - **RBAC** systems have the possible temporal constraints on roles, such as time and duration of role activiations, and timed triggering of the role by an activiation of another role.
  - Using an **RBAC** model is highly desirable goal for addressing the key security requirements of Web-based applications
  - Discretionary access control (**DAC**) and mandatory access control (**MAC**) models **lack capabilities** needed to support the security requirements of emerging enterprises and Web-based applications

# Attacks on the Database

## SQL Injection

  - Attacker injects a string input through the web application
  - Changes/manipulates the application generated SQL statement to the attacker's advantage

## SQL Manipulation
  
  - Change the SQL command in the application
    * Adding conditions to the WHERE clause of a query

### Example
```sql
SELECT * FROM users WHERE username = 'jake' and PASSWORD = 'jakepassword';
```

Now we do **SQL Manipulation** to do this:

```sql
SELECT * FROM users WHERE username = 'jake' AND (PASSWORD = 'jakepasswd' OR 'x'='x')
```

## Code Injection

  - Add additional SQL statements or commands to existing SQL statement by exploiting bugs
    * Patch systems in a timely manner

## Function Call Injection

  - A database or OS function call inserted into a vulnerable SQL statement

### Example

```sql
SELECT TRANSLATE ('user input', 'from_string', 'to_string') FROM dual;
```

```sql
SELECT TRANSLATE ("|| UTL_HTTP.REQUEST ('http://129.107.2.1) ||", '98765432', '9876') 
FROM dual;
```

## Risks associated with SQL Injection

  - Database finger printing
  - Denial of Service
  - Bypass authentication
  - Identify injectable parameters
  - Execute remote commands
  - Privilege escalation

## Protection Techniques

 - Bind Values
   * parameterize statements

   ```
   PreparedStatement stmt = conn.prepareStatement(
       "SELECT * FROM EMPLOYEE WHERE EMPLOYEE_ID=? AND PASSWORD=?"
   );
   stmt.setString(1, employee_id)
   stmt.setString(2, password)
   ```
  - Input Validation/Sanitize the input
    * By filtering input, remove escape characters from input strings with *<b>Replace</b>* function
    * Single quote delimiter ' replaced by "
    * Define good data for input
      * Strip out bad stuff - quotes, semicolons, escapes
  - Function Security
    * Restrict access for both standard and custom functions
  - Limit database permissions and segregate users
    * Web applications must use a database connection with very limited rights
    * Only logged-in users have required rights to work with the database
  - Isolate the webserver
    * Keep the database server on a different host from the webserver
    * Keep them on seperate network locations

## Summary

  - Major Database Security Issues
    * Privilege abuse
    * Weak authentication
    * Backup data exposure
    * SQL injection
    * DB platform vulnerabilities
  - Fixes
    * Encryption
    * Levels of access control (Query, Content)
    * Strong Authentication
    * Firewall/IDS
    * Patch management