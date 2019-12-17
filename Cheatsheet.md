# PL/SQL Cheatsheet
Hi! Welcome to my Cheatsheet of PL/SQL. This is still work in progress but I hope you will find something usefull here ;-)

ToDo:
- [X] [Procedures](#Procedures)
- [ ] Functions
- [ ] Packages
- [ ] Everything else?


## Procedures
A procedure is a group of PL/SQL statements that are stored on the database server. The use of the procedure requires calling it by the user:
```
BEGIN
  ProcedureName;
END;
```
or
```
EXECUTE ProcedureName;
```
### Procedure syntax
```
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
#### Parameter modes:
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

```
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

**pReturn** is in the **OUT** mode ([what?](#Parameter-modes)) and that's why **b** is equal to 2.

#### BetterAddOne
```
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
All redundant code parts have been cut out and we use **IN OUT** parameter mode([?](#Parameter-modes)).
#### "Real" example
```
CREATE OR REPLACE PROCEDURE NewIntern
    (
    pLastName IN Employees.LastName%type,
    pTeam IN Teams.TeamName%type,
    pBoss IN Employees.LastName%type,
    pSalary IN Employees.Salary%type
    ) IS
    vTeamID Teams.TeamID%type;
    vBossID pracownicy.EmployeeID%type;
BEGIN
    --Team
    SELECT Teams.TeamID
    INTO vTeamID
    FROM Teams
    WHERE Teams.TeamName = pTeam;

    --Boss
    SELECT Employees.EmployeeID
    INTO vBossID
    FROM Employees
    WHERE Employees.LastName = pBoss;

    --Insert
    INSERT INTO Employees(EmployeeID, LastName, Position, BossID, EmploymentDate, Salary,  TeamID)
    VALUES((SELECT MAX(Employees.EmployeeID) + 1 FROM Employees), pLastName, 'Intern', vBossID, current_date, pSalary, vTeamID);
END NewIntern;
```
That's what more complicated but also more real use of procedures looks like.

## Functions
A function is a group of PL/SQL statements that are stored on the database server. Functions and procedures are very similar but functions returns a value to the caller.
### Function syntax
```
CREATE [OR REPLACE] FUNCTION <FunctionName>
[ (
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [,
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> ...]
) ] RETURN <DataType> IS
[Other Declarations]
BEGIN
  <FunctionBody>;
RETURN <ReturnedValue>;
END <FuncionName>;
```
<!--![](https://docs.oracle.com/cd/B19306_01/server.102/b14200/img/create_function.gif)

Illustration from [Oracle Database Online Documentation](https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_5009.htm)-->
Function syntax is the same as the [procedure syntax](#procedure-syntax) with the exception of defining **RETURN**:
```
RETURN <DataType>
```
and returning value to the caller:
```
RETURN <ReturnedValue>;
```
Function **MUST** return a value to the caller (error 06503) even if the value is she same every time for example **NULL**.
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
