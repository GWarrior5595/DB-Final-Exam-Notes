# Introduction

- Approaches to database design:
    * Bottom-up (synthesis)
        * Basic relationships among individual attributes to start with
        * Collecting large number of binary relationships among 
    * Top-down (analysis)
        * Group attributes into relations as they exist together naturally, in real-life situations
        * Analyze relations to figure out the details

# Implicit Goals of Design Activity

  - Information preservation
    * Maintain all concepts -> attribute types, entity types, relationship types
    * Everything from the conceptual design
  - Minimum redundancy
    * Minimize redundant storage of same information
    * Reducing multiple updates to ensure consistency

# Informal Design Guidelines for Relation Schemas

  - Measures of quality
    * Making sure attribute semantics are clear
    * Reducing redundant information in tuples
    * Reducing NULL values in tuples
    * Disallowing possibility of generating spurious tuples

# Guideline 1

  - Design relation schema so that it easy to explain its real-world meaning
  - Do not combine attributes from multiple entity types and relationship types into a single relation
  - See slide 8 and 9 of March 29th slides for company schema
  - Mixing attributes from multiple entities(See slide 10)

# Guideline 2

  - Design base relation schemas so that no update anomalies are present in the relations
  - If any anomalies are present:
    * Note them clearly
    * Make sure that the programs that update the database will operate correctly
    * Use triggers or stored proc for automatic updates

## Redundant Information in Tuples and Update Anomalies

  - Grouping attributes into relation schemas
    * Significantly effect on storage space
  - Storing natural joins of base relations leads to **update anomalies**
  - Types of update anomalies:
    * Insertion
    * Deletion
    * Modification

## Redundant Information in Tuples and Insertion Anomalies(See slide 14)

  - To insert new employees, all attributes related to the department have to be correctly entered in EMP_DEPT
  - Difficult to enter a new department with no employees - violation of entity integrity as SSN is a primary key

## Redundant Information in Tuples and Deletion Anomalies

  - If you delete an employee who happens to be the last employee in a department - that department is lost.

## Redundant Information in Tuples and Update Anomalies

  - Updating the manager of a department in EMP_DEPT
    * Update all employees who work in that department

# Guideline 3

  - Avoid placing attributes in a base relation whose values may be frequently be NULL
  - If NULLs are unavoidable:
    * Make sure that they apply in exceptional cases only, not to a majority of tuples

## Null Value in Tuples
  - Group many attributes together into a "fat" relation
    * Can end up with many NULLs
  - Problem with NULLs
    * Wasted storage space
    * Avoiding joins with NULL values
    * Problems understanding the meaning of attributes
  - If only a small number of employees have individual offices
    * Attribute Office_Number in EMPLOYEE relation - not justified
    * New relation EMP_OFFICES(Essn, Office_number)

# Guideline 4
  
  - Design relation schemas to be joined with equality conditions on attributes that are appropriately related
    * Guarantees that no spurious tuples are generated
  - Avoid relations that contain matching attributes that are not (foreign key, primary key) combinations

## Generation of Spurious Tuples
  - See slides 24 and 25 for images on March 29th slides
  - Natural Join
    * Result produces many more tuples than the original set of tuples in EMP_PROJ
    * These are called **spurious tuples**
    * Represent **spurious** information that is not valid
  - See slide 27 for image

# Functional Dependencies

## Definition of a Functional Dependency

  - Constraint between two sets of attributes from the database
  - **Formal Definition**: A **functional depenency**, denoted by *X* --> *Y*, between two sets of attributes X and Y are the subsets of *R* specifies a *constraint* on the possible tuples that can form a relation state *r* of *R*. The constraint is that, for any two tuples *t<sub>1</sub>* and *t<sub>2</sub>* in *r* that have *t<sub>1</sub>[X]* = *t<sub>2</sub>[X]*, they must also have *t<sub>1</sub>[Y]* = *t<sub>2</sub>[Y]*
  - Property of the sematincs or meaning of the attributes

## Functional Dependencies

  - *Y* component of a tuple depends on <span style="text-decoration: underline;">or</span> is determined by values of *X* component
  - Values of *X* component of a tuple uniquely/functionally determine the values of *Y* component
  - *<b>Y is functionally dependent on X</b>*
  - `X -> Y`
--------------------------------------------------------------------
  - If *X* is a *<b>candidate key</b>* of relation, *R*
    * Cannot be more than one tuple with given *X* value
    * `X -> Y` for any subset of attributes *Y* in *R*
    * With X as a canidate key, `X -> R`
    * `X -> Y` in R does not mean `Y->X` in R
 ------------------------------------------------------------------

 (INSERT FUNCTIONAL DEPENDENCY IMAGE HERE Slide 8 of April 3rd)
 
 ------------------------------------------------------------------

  - Given a populated relation
    * Cannot determine which FD's hold and which do not
    * Unless meaning of and relationships among attributes known
    * Can state that FD does not hold if there are tuples that show violation of such an FD

-------------------------------------------------------------------

![alt text](https://github.com/Toltar/DB-Final-Exam-Notes/blob/master/images/Slide_10_April3rd.PNG)

