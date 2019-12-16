# P/L SQL Cheatsheet

Hi! Welcome to my Cheatsheet of P/L SQL. This is still work in progress but I hope you will find something usefull here ;-)

ToDo:
- [X] Procedures
- [ ] Functions
- [ ] Packages
- [ ] Everything else?

## Procedures
A Procedure is a group of PL/SQL statements that are stored on the database server. The use of the procedure requires calling it by the user:
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
```
CREATE OR REPLACE PROCEDURE AddOne 
(
vTest IN NUMBER DEFAULT 1,
vReturn OUT NUMBER
) IS
vOne NUMBER(1) := 1;
BEGIN
    vReturn := vTest + vOne;
END AddOne;
```


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
