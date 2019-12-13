# P/L SQL Cheatsheet

Hi! Welcome to my Cheatsheet of P/L SQL. This is still work in progress but I hope you will find something usefull here ;-)

ToDo:
- [ ] Procedures
- [x] Functions
- [ ] Packages
- [ ] Everything else?

## Procedures
```
CREATE [OR REPLACE] PROCEDURE <ProcedureName> IS
[ (
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [DEFAULT <DefaultValue>] [,
Argument [IN | OUT | IN OUT] [NOCOPY] <DataType> [DEFAULT <DefaultValue>] ...]
) ]
[Other Declarations]
BEGIN
  <ProcedureBody>;
END <ProcedureName>;
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
![TestŁobrazka](https://docs.oracle.com/cd/B19306_01/server.102/b14200/img/create_function.gif)
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
