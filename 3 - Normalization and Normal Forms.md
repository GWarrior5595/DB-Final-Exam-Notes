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
  - <image src="./images/Slide 21.PNG" width=400
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

# Third Normal Form

# Boyce-Codd Normal Form

# 3ND and BCNF

# Multivalued Dependency & 4NF

# Join Dependency & 5NF