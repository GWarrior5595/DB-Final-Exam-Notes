# Normalization of Relations

 - Normal forms are based on functional dependencies amongst the attributes of a relation
 - Take a relation schema through a series of tests
    * Certify whether it satisfies a certain normal form
    * Proceed in a top-down fashion
  - Analyze a given relation schema based on their FD's and primary keys, to achive -
    * Minimum redundancy
    * Minimizing insertion/deletion/update anomalies
  - Filtering/purification process to achieve better quality design
  - **Definition**: The **normal form** of a relation refers to the highest normal form condition that it meets, and hence indicates the degree to which it has been normalized
  - **Objectives of Normalization**
    1. To free the collection of relations from undesirable insertion, update and deletion dependencies;
    2. To reduce the need to restructuring the collection of relations, as new types of data are introduced, and thus increase the life span of application programs.
    3. To make the relational model more informative to users
    4. To make the collection of relations neutral to the query statistics, where these statistics are liable to change as time goes by.
    > "Further Normalization of the Data Base Relational Model", - E.F. Codd
  - Properties that a normalized relational schema should have:
    * **Nonadditive/lossless join property**
      * Ensures spurious tuples generation problem does not occur with schemas created after decomposition
      * **Extremely critical and must be achieved at all costs**
    * **Dependency preservation property**
      * Each FD is represented in some indivigaual relation resulting after decomposition
      * **Desirable but sometimes sacrificed for other factors**

# Practical Use of Normal Forms

  - Normalization carried our in practice
    * Resulting designs are of high quality and meet the desirable properties/goals
    * Pays for particular attention to normalization only up to 3NF, BCNF (3.5NF), or at most 4NF
  - You do not need to normalize to the highest possible normal form

# Denomalization

  - **Definition**: is the process of storing the join of higher normal form relations as a base relation, which is in a lower normal form.

# Keys (REVIEW)

  - **Superkey**
    * A set of attributes
      * no two tuples in any legal relation state will have the same values for the superkey
      * {Ssn} is a key for employee; {Ssn}, {Ssn, Ename}, {Ssn,Ename,Bdate} are superkeys
  - **Canidate key**
    * If more than one key in a relation schema
      * One is a **primary key**
      * Others are **secondary keys**
  - **Prime attribute**
    * Attribute is a member of some **canidate key** of a given relation

# First Normal Form

  - Domain of an attribute must include only **atomic (simple, indivisible)** values
  - Value of any attribute in a tuple must be a single value
  - Disallows a set of values as an attribute value in a single tuple
    * **Disallows relations within relations**

  - <image src="./images/Slide 20.PNG" width=375></image>

## Techniques to achieve 1NF

  - Remove attributes that violate 1NF and place in separate relation
  - <image src="./images/Slide 21.PNG" width=400>
  - Expand the key
    * Introduces redundancy
    * <image src="./images/Slide 22.PNG" width=400></image>
    * Use several atomic attributes (DLocation1, Dlocation2, Dlocation3)
      * NULL values
      * Querying and ordering problems

## First Normal Form

Example:

<image src="./images/Slide 24.PNG" width=450/>

  - Does not allow **nested relations**
  - To change a relation to 1NF:
    * Remove nested relation attributes into a new relation
    * Propagate the primary key into it 
    * **Unset** relation into a set of 1NF relations

# Second Normal Form

  - **Full functional dependency**
    * For a FD, `X->Y` - if removal of any attribute *A* from *X* makes the FD not hold anymore
    * {Ssn, Pnumber} -> Hours
  - **Partial dependency**
    * {Ssn, Pnumber} -> Ename
  - **Prime Attribute**
    * Part of any canidate key will be considered as prime
  - **Definition**: A relation schema *R* is in **second normal form (2NF)** if every non prime attribute *A* in *R* is not partially dependent on *any* key of *R*.<sup>11</sup>
  - <image src="./images/Slide 27.PNG" width=450/>
  - Test for 2NF: Check FDs whose left-hand side attributes are part of the primary key
    * If the primary key contains a single attribute, no test needed
  - <image src="./images/Slide 29.PNG" width=450/>
  - Example of 2NF Normalization: 
    * <image src="./images/Slide 30.PNG" width=450/>
  - Another Example:
    * <image src="./images/Slide 31.PNG" width=450/>

# Third Normal Form

  - **Definition**: A relation schema *R* is in **third normal form(3NF)** if, whenever a *nontrivial* functional dependency *X --> A* holds in *R*, either (a) X is a superkey of *R*, or (b) *A* is a prime attribute of *R*
  - **Alternative Definition**: A relation schema *R* is in 3NF if every nonprime attribute of *R* meets both of the following conditions:
    * It is fully functionally dependent on every key of *R*
    * It is nontransitiviely depenent on every key of *R*
  - **<i>Transitive dependency</i>**
    * `X-->Y` in a relation *R* is a transitive dependency, if
      * There exists a set of attributes Z in R which is neither a canidate key
    * Despondency **Ssn -> Dmgr_ssn** is transitive through **Dnumber** in EMP_DEPT
    * <image src="./images/Slide 8.PNG" width=450/>
    * **Ssn -> Dnumber** and **Dnumber -> Dmgr_ssn** hold
    * **Dnumber** is neither a key itself; not a subset of the key of EMP_DEPT
  - Example of 3NF normalization:
    * <image src="./images/Slide 10(2).PNG" width=450/>
  - Example to 2NF to 3NF:
    * <image src="./images/Slide 11.PNG" width=450/>
    * <image src="./images/Part D.PNG" width=450/>

# Overview of First, Second, and Third Normal Forms

<image src="./images/Slide 12.PNG" width=450/>

# Boyce-Codd Normal Form(3.5NF)

  - **Definition of 3NF**: relation schema *R* is in **third normal form(3NF)** if, whenever a *nontrivial* functional dependency *X --> A* holds in *R*, either (a) X is a superkey of *R*, or (b) *A* is a prime attribute of *R*
  - **Definition for BCNF**:  A relation schema *R* is in **BCNF** if whenever a *nontrivial* functional dependency *X->A* holds in *R*, then *X* is a superkey of *R*.
  - Difference from 3NF
    * FD's having the RHS as a prime attribute[clause (b)] is absent
    * Makes BCNF a stronger form than 3NF
  - Meant to be a simpler 3NF, but found to be stricter than 3NF
  - Every relation in BCNF is also in 3NF
    * Relation in 3NF is not necessarily in BCNF
    * Most relation schemas are in 3NF are also in BCNF 
---------------------------------------------------

<image width=450 src="./images/Slide 16.PNG"></image>

  - Thousands of lots
  - Lots from only two countries
  - Lot sizes are different in the 2 countries
    - additional dependency
    - FD5: Area -> Country_Name

---------------------------------------------------

<image width=450 src="./images/Slide 17.PNG"></image>

-----------------------------------------------------
  - <image width=200 src="./images/Slide 18.PNG"></image>
  - R is in 3NF, but not BCNF due to FD2
  - Such a FD results in potential redundancy of data
  - Ideally you want to avhieve BCNF or 3NF for every relation
------------------------------------------------------

<image width=450 src="./images/Slide 19.PNG"></image>

------------------------------------------------------

<image width=450 src="./images/Slide 20(2).PNG"></image>

------------------------------------------------------


# 3ND and BCNF

<image width=450 src="./images/Slide 21(2).PNG"></image>

# Multivalued Dependency & 4NF

  - Multivalued dependency (MVD)
    - Consequence of first normal form(1NF)
    - Two or more multivalued independent attributes in the same relation
    - Repeat every value of one of the attribute to maintain data consistency
    and data independence

<image width=450 src="./images/Slide 23.PNG"></image>

  - When two independent 1:N relationships are mixed in the same relation, an MVD may occur.

# Forth Normal Form

  - **Definition**: A relation schema *R* is in 4NF with respect to a set of dependencies *F* (that includes functional dependencies and multivalued dependencies) if, for every *nontrivial* multivalued dependency *X->->Y* in *F*, *X* is a superkey for *R*

<image width=450 src="./images/Slide 24(2).PNG"></image>

# Join Dependency & 5NF

  - **Join dependency**
    - Whenever a supplier **<i>s</i>** supplies part **<i>p</i>**, and a project **<i>j</i>** uses part **<i>p</i>**, and the supplier **<i>s</i>** supplies at least one part to project **<i>j</i>**, then supplier **<i>s</i>** will also be supplying part **<i>p</i>** to project **<i>j</i>**.
  - Very particular semantic constraint
    * Normalization into 5NF is very rarely done in practice

<image width=450 src="./images/Slide 26.PNG"></image>