# Lemo-Programming-Language and Interpreter

Lemo is a programming language and interpreter, which I developed during the Summer Holidays of 2021<br/>
Motivation for developing was pure interest/curiosity in learning how programming languages work under the hood<br/>
The design of the language is heavily inspired by BASIC and the language it is written in is TypeScript<br/>

**Author**: Artem Moshnin <br/>
**Resources used**: [Gabriele Tomassetti's blog entries](https://tomassetti.me/resources-create-programming-languages/) with numerous useful resources (Books & Articles)

## Overview

### General information

Programming language supports various fundamental programming concepts such as variable-declaration,
function calling, conditional statements, for and while loops, proper order of operations, recursion and much more.

> The language also includes the following built-in datatypes: Integers, Floats, Strings, Lists.

> Each datatype also has its own methods implemented that can be called on instances of these datatypes.

> The language also includes the following built-in global functions: print(), input(), clear().

> The language also includes a type-checking system.

> Multi-line support is integrated, as well as the ability to run external files of '.lemo' format.

Below is the language's EBNF-based grammar, and following that are some examples of programs that the language can run. Even further down is a link to a repl.it where the programs can be run.

### Parser

Parser builds up a syntax tree of the program from the tokens created by the Lexer.
The parses has to figure out if the tokens match out language grammar. If it does, generate a tree accordingly.
For example: "200 200 +" makes no sense in our language. While "123 + 456" makes perfect sense.

In other words, order of operations are being followed and the parses construct the syntax tree accordingly to meet its requirements, as illustrated below:

<p align="center">
<img src="./img/parser.png" alt="Parser image (of mathematical expression)" style="width:500px;" />
</p>

To understand the source code of the parser, I below illustrated the grammar of a mathematical expression that is being analyzed while parsing to ensure that the order of operations are followed:

<p align="center">
<img src="./img/parser_order_of_operations.jpeg" alt="Parser image (of mathematical expression)" style="width:500px;" />
</p>

### Interpreter

The role of the interpreter is to traverse through the AST (Abstract Syntax Tree) that the parser builds, look for different node types and determine what code should be executed. For example, if it comes across a binary operation node with a '+' operator - it will add its left and right child nodes together. The AST (Abstract Syntax Tree) will allow the interpreter to follow the order of operations correctly.

### Steps for implementing new operators

### Variables declaration

<details>
  <summary style="fontWeight:bold;">Comparison, Logical Operators and Booleans</summary>

    >> Comparison Operators: =, !=, <, >, <=, >=

    >> Logical Operators: and, or, not
    - Example of Logical Operators:
    - 5 == 5 and 6 == 6 => 1
    - 1 + 1 == 2 or 2 + 2 == 5 => 1

    >> Booleans: 0 = FALSE, 1 = TRUE

</details>

## Grammar

<details>
  <summary style="fontWeight:bold;">Grammar (EBNF-based) of the Lemo-Programming Language</summary>

    -> Grammar (EBNF-based) of the Lemo-Programming Language <-

    statements : NEWLINE* statement (NEWLINE+ statement)* NEWLINE\*

    statement : KEYWORD:RETURN expr?
    : KEYWORD:CONTINUE
    : KEYWORD:BREAK
    : expr

    expr : KEYWORD:VAR IDENTIFIER EQ expr
    : comp-expr ((KEYWORD:AND|KEYWORD:OR) comp-expr)\*

    comp-expr : NOT comp-expr
    : arith-expr ((EE|LT|GT|LTE|GTE) arith-expr)\*

    arith-expr : term ((PLUS|MINUS) term)\*

    term : factor ((MUL|DIV) factor)\*

    factor : (PLUS|MINUS) factor
    : power

    power : call (POW factor)\*

    call : atom (LPAREN (expr (COMMA expr)\*)? RPAREN)?

    atom : INT|FLOAT|STRING|IDENTIFIER
    : LPAREN expr RPAREN
    : list-expr
    : if-expr
    : for-expr
    : while-expr
    : func-def

    list-expr : LSQUARE (expr (COMMA expr)\*)? RSQUARE

    if-expr : KEYWORD:IF expr KEYWORD:THEN
    (statement if-expr-b|if-expr-c?)
    | (NEWLINE statements KEYWORD:END|if-expr-b|if-expr-c)

    if-expr-b : KEYWORD:ELIF expr KEYWORD:THEN
    (statement if-expr-b|if-expr-c?)
    | (NEWLINE statements KEYWORD:END|if-expr-b|if-expr-c)

    if-expr-c : KEYWORD:ELSE
    statement
    | (NEWLINE statements KEYWORD:END)

    for-expr : KEYWORD:FOR IDENTIFIER EQ expr KEYWORD:TO expr
    (KEYWORD:STEP expr)? KEYWORD:THEN
    statement
    | (NEWLINE statements KEYWORD:END)

    while-expr : KEYWORD:WHILE expr KEYWORD:THEN
    statement
    | (NEWLINE statements KEYWORD:END)

    func-def : KEYWORD:FUN IDENTIFIER?
    LPAREN (IDENTIFIER (COMMA IDENTIFIER)\*)? RPAREN
    (ARROW expr)
    | (NEWLINE statements KEYWORD:END)

</details>

## Examples of Fundamental Concepts of Programming Languages

### Built-In Constants

```javascript
PRINT(NULL) // 0
PRINT(FALSE) // 0
PRINT(TRUE) // 1
PRINT(MATH_PI) // 3.141592653589793
```

### Built-In Functions

```javascript
// > General functins < //
'PRINT' =>
PRINT("Hello World") // Hello World

// > Check if is corresponding type < //
'IS_NUMBER' =>
IS_NUMBER(123) // 1
IS_NUMBER("Hello") // 0

'IS_STRING' =>
IS_STRING("Hello") // 1
IS_STRING(123) // 0

'IS_LIST' =>
IS_LIST([1,2,3]) // 1
IS_LIST("Hello") // 0

'IS_FUNCTION' =>
VAR x = FUN test(a) -> a * 2 // <function test>
IS_FUNCTION(x) // 1

// > List functions (mutable) < //
'APPEND' =>
VAR list = [1,2,3] // [1, 2, 3]
APPEND(list, 4)
PRINT([1, 2, 3]) // [1, 2, 3, 4]

'POP' =>
VAR list = [1,2,3] // [1, 2, 3]
POP(list, 0) // 1
PRINT(list) // [2, 3]

// > List functions (immutable) < //
'EXTEND' =>
VAR list = [1, 2, 3]
VAR extendedList = EXTEND(list, [1,2,3])
PRINT(extendedList) // [1, 2, 3, 1, 2, 3]
PRINT(list) // [1, 2, 3]
```

### Basic Variable Declaration

### Basic Function Calling

```javascript
FUN add (a, b) -> a + b // <function add>
add(5, 6) // 11
```

### Re-assigning function to a variable

```javascript
FUN add (a, b) -> a + b // <function add>
VAR someFunc = add // <function add>
someFunc(1, 2) // 3
```

### Anonymous functions

```javascript
FUN (a) -> a + 6 // function<anonymous>
VAR someFunc = FUN(a) -> a + 6
someFunc(12) // 12 + 6 = 18
```

### Structured Error Traceback when calling functions which throw some error

```javascript
FUN test(a) -> a / 0 // function<test>
test(12345)
// Error output:
// Traceback (most recent call last):
//    File <stdin>, line: 1, in <program>
//    File <stdin>, line: 1, in test
// Runtime Error: Division by zero
//
// FUN test(a) -> a / 0
//                    ^
```

### Basic Conditional Statements

### For Loop (positive step values)

```javascript
VAR result = 1
FOR i = 1 TO 6 THEN VAR result = result * i
result // result => 120
```

### For Loop (negative step values)

```javascript
VAR result = 1
FOR i = 5 TO 0 STEP -1 THEN VAR result = result * i
result // result => 120
```

### While Loop

```javascript
VAR result = 0
WHILE result < 100000 THEN VAR result = result + 1
result // result => 100000
```

### Order of Operations

```javascript
VAR result1 = (2 + 5) * 2 - (10 + 5) // -1
VAR result2 = 5 == 5 OR 3 == 2 // 1 (TRUE)
```

## Examples of Advanced Concepts of Programming Languages (Multi-Line Statements & Reading from files)

### Multi-Line Statements (if statements)

```javascript
IF <expr> THEN <expr>

IF <expr> THEN
  <expr1>
  <expr2>
  <expr3>
END
```

### Multi-Line Statements (for loops)

```javascript
FOR i = 0 to 10 THEN <expr>

FOR i = 0 TO 10 THEN
  <expr1>
  <expr2>
  <expr3>
END
```

### Multi-Line Statements (functions)

```javascript
FUN <name>() -> <expr>

FUN <name>()
  <expr1>
  <expr2>
  <expr3>
END
```

## DataTypes

### DataType: Integer (INT)

```javascript
VAR int = 25
```

### DataType: Floating number (FLOAT)

```javascript
VAR float = 23.2
```

### DataType: String

```javascript
'Text'
"Text with \"quotes\""
'Text with \\ backslashes \\'
'Text \nwith \nnewlines'
```

### DataType: Lists

```javascript
[] // List syntax
[1, 2, 3] + 4 => [1, 2, 3, 4] // Add an element to the list
[1, 2, 3] * [3, 4, 5] => [1, 2, 3, 3, 4, 5] // Concatenate two lists

[1, 2, 3] - 1 => [1, 3] // Remove element from the list at index 1
[1, 2, 3] - 0 => [2, 3] // Remove element from the list at index 0

[1, 2, 3] - -1 => [1, 2] // Remove the last element from the list
[1, 2, 3] - -2 => [1, 3] // Remove the before-last element from the list

[1, 2, 3] / 0 => 1 // Retrieve an element from the array at index 0
[1, 2, 3] / 1 => 2 // Retrieve an element from the array at index 1
```
