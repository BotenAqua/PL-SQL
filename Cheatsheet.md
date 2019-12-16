# PL/SQL Cheatsheet
Hi! Welcome to my Cheatsheet of P/L SQL. This is still work in progress but I hope you will find something usefull here ;-)

ToDo:
- [X] [Procedures](#Procedures)
- [ ] Functions
- [ ] Packages
- [ ] Everything else?


## Procedures
A Procedure is a group of PL/SQL statements that are stored on the database server. The use of the procedure requires calling it by the user:
```SQL
BEGIN
  ProcedureName;
END;
```
or
```SQL
EXECUTE ProcedureName;
```
### Procedure syntax
```SQL
CREATE [OR REPLACE] PROCEDURE <ProcedureName> 
[ (
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [DEFAULT <DefaultValue>] [,
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [DEFAULT <DefaultValue>] ...]
) ] IS
[Other Declarations]
BEGIN
  <ProcedureBody>;
END <ProcedureName>;
```
### Procedure parameters
```
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [DEFAULT <DefaultValue>]
```
#### Parameters modes:
- **IN** - default mode; It lets you pass the value to the procedure. It's the read-only parameter. Inside the procedure it acts like a constant
- **OUT** - Returns value to the caller. You can reference and use its value inside the procedure.
- **IN OUT** - Combination of **IN** and **OUT** ~~(duh)~~. It passes the value to the procedure, allows modification and returnes the value to the caller.
<!-- NO COPY -->
#### Defining datatype

While specyfying datatype you can not specify length or precision. For example use **VARCHAR** instead of **VARCHAR(20)** or **NUMBER** instead of **NUMBER(6,2)**.

#### Default value
Use **DEFAULT** to specyfy the default value of the argument. You can use := instead of **DEFAULT**.

### Examples
#### AddOne

```SQL
CREATE OR REPLACE PROCEDURE AddOne
(
pNumber IN NUMBER,
pReturn OUT NUMBER
) IS
vOne NUMBER(1) := 1;
BEGIN
    pReturn := pNumber + vOne;
END AddOne;
/
DECLARE
a NUMBER(1);
b NUMBER(1);
BEGIN
a := 1;
AddOne(a, b);
DBMS_OUTPUT.PUT_LINE('Value of a: ' || a);
DBMS_OUTPUT.PUT_LINE('Value of b: ' || b);
END;
```
This procedure is one big overkill but it shows basic concepts.
We define two parameters: **pNumber** and **pReturn** and the variable vOne.
Next we **do complicated operations on the data** and assign recived value to **pReturn**.

**pReturn** is in the **OUT** mode ([what?](#Parameters_modes)) and that's why **b** is equal to 2.

### BetterAddOne
```SQL
CREATE OR REPLACE PROCEDURE BetterAddOne
(
pNumber IN OUT NUMBER
) IS
BEGIN
    pNumber := pNumber + 1;
END BetterAddOne;

/

DECLARE
a NUMBER(1);
BEGIN
a := 1;
BetterAddOne(a);
DBMS_OUTPUT.PUT_LINE('Value of a: ' || a);
END;
```
Better? Better! Cleaner? Sure!
All redundant code parts have been cut out and we use **IN OUT** parameter mode.


## Functions
```SQL
CREATE [OR REPLACE] FUNCTION <FunctionName>
[ (
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [,
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> ...]
) ] RETURN <DataType>
[Other Declarations]
BEGIN
  <FunctionBody>;
RETURN <ReturnedValue>;
END <FuncionName>;
```
![](https://docs.oracle.com/cd/B19306_01/server.102/b14200/img/create_function.gif)

Illustration from [Oracle Database Online Documentation](https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_5009.htm)
### Wyjasnienia tego wszystkiego

### Examples
#### ReturnOne
```
CREATE OR REPLACE FUNCTION ReturnOne
RETURN NUMBER IS
BEGIN
RETURN 1;
END ReturnOne;

```
#### Recursively factorial
```
CREATE OR REPLACE FUNCTION RecFactorial
    (
    pN IN NUMBER
    ) RETURN NUMBER IS
BEGIN
    IF pN = 0 THEN
        RETURN 1;
    ELSE
        RETURN pN * RecFactorial(pN -1);
    END IF;
END RecFactorial;
```
