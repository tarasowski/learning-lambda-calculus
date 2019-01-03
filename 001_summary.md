# AN INTRODUCTION TO FUNCTIONAL PROGRAMMING THROUGH LAMBDA CALCULUS

## Introduction

* Functional programming is an approach to programming based on function calls as the primary programming construct.

* Some of the simplest computing machines are electronic calculators. These are used to carry out mathematic calculations. The main disadvantage is that there is no way of generalising a calculation so it can be used again and again with different values. 

* Programming languages provide names to stand for arbitrary values. We write program using names to stand for values in general. We then run the programm with the names taking on on particular values from input. The program does not have to change to be used with different values: **we simply change the values in the input and the computer system makes sure that they are associated with the right names in the program**

> The main difference between imperative programming languages like Pascal, FORTRAN and COBOL and functional programming languages, like SML and Miranda lies in the rules governing the association of names and values. 

### Names and values in imperative and functional languages

* Traditional imperative languages are based around the idea of a variable as a changable association between a name and value. **These languages are said to be imperative because they consists of sequence of commands.**

```
<command1>;
<command2>;
<command3>;
<command4>
```

* Typically each command consists of **assignment** which changes a variable values. This involves working out the value of an expression and associating the result with the name:

```
<name> := <expression>
```

* Functional languages are based on structured function calls. A functional program is an expression consisting of a function call which calls other functions in turn:

```
<function1>(<function2>(<function3>( ...) ...))
```

* Each function receives values from and passes new values back to the calling function. This is knows as **function composition** or **nesting.**

> In imperative languages, commands may change the values associated with a name by a previous command so each name may be and usually will be associated with different values while program is running. 

**Note:** In imperative, the same name may be associated with a different values. 

* In functional language, names are only introduces as formal parameters of functions and given by function calls with actual parameters. Once is formal parameter associated with the actual parameter value there is no way for it to be associated with a new value. There is no concept of command which changes the value associated with name through assignment. There is no concept of command repetition or sequence to enable successive changes to values associated with names. 

**Note:** In functional language, a name is only ever associated with one value.

### Execution order in imperative and functional languages

* In imperative languages, the order in which the commands gets executed is usually crucial. Values are passed from command to command by references to common variables and one command may change the variable's values before that variable is used in the next command. 

* For example, in the program fragment to swap X and Y:

```
T := X;
X := Y;
Y := T
```
* T's values depend on X's values, X values depend on Y's values and Y's values depend on T's values. Any change in the sequence completely changes what happens:

```
X := Y
T := X
Y := T
```
* sets X to Y and:

```
T := X;
Y := T;
X := Y
```
* sets Y to X

> Imperative languages have fixed execution orders.

* In functional languates, function calls cannot change the values associated with the common names. Hence the order in which nested function calls are carried out does not matter because function calls cannot interact with each other. 

```
F(A(D), B(D), C(D))
```
* The order in which A(D), B(D), C(D) carried out does not matter, beacuse functions A, B and C cannot change it's thier common actual paramter D. 

> In functional languages there is no necessary exexution order. 

**Note:** Of course, functional programs must be executed in some order  - all programs are - but the order does not affect the final result. This execution independence in one of the strengths of functional programming. **Probably in distributed systems, where each function can run in parallel and independent**

### Repetition in imperative and functional languages

* In imperative languages command may change the values associated with a names by previous commands so a new name is not necessarily introduced for each new command. In order to carry out several commands several times, those commands need to be duplicated. Instead, the same commands are preated. Each name may be and usually will be associated with different values while a program is running.

* For example, in order to find the sum of the N elements of array A we do not write:

```
SUM1 := A[1];
SUM2 := SUM1 + A[2];
SUM3 := SUM2 + A[3];
...
```
* Instead of creating N new SUMs and refering to each element of A explicitly we write a loop that reuses one name for the sum, say SUM and another that indicates successive array elements, say I:

```
I := 0;
SUM := 0;
WHILE I < N DO
BEGIN
    I := I + 1;
    SUM := SUM + A[I]
END
```

* In imperative languages, new values may be associated with the same name through command repetiton. 

* In functional languages, because the same names cannot be resued with different values, nested function calls are used to create new version of the names for new values. Because command repetition cannot be used to change the values associated with names, **recursive** function calls are used to repeatedly create new versions of names associated with new values. 

* For example, we might write a function in Pascalish style to sum an array:

```
FUNCTION SUM(A:Array [1...N] of Integer; I, N:Integer):Integer;
BEGIN
    IF I > N THEN
        SUM := 0
    ELSE
        SUM := A[1] + SUM(A, I+1, N)
    END

// Thus, for the function call:
SUM(B, 1, M)

// The sum is found through successive resucrive calls to SUM:
B[1] + SUM(B, 2, M) = B[1] + B[2] + SUM(B, 3, M)
B[1] + B[2] + ... + B[M] + SUM(B, M+1, M)
B[1] + B[2] + ... + B[M] + 0
```

* **Note:** Each recursive call to SUM creates new local versions of A, I and N, and the previous versions become inaccessible. At the end of each recursive call, the new local variable are lost, the partial sum is returned to the previos call and previous local variable come back to use. 

* In functional languages, new values are associated with new names through recursive function call nesting.

### Data structures in functional languages

* In imperative languages, array elements and record fields are changes by succesive assignments. In functional languages, becauset here is no assignemnt, sub-structures in data structures cannot be changes one at a time. Instead, it is necessary to write down a whole structure with explicit changes to the appropriate sub-structure. 

**Note:** Functional languages provide explicit representations for data structures. 

* Functional languages do not provide arrays because without assignment there is no easy way to access an arbitrary element. Writing out an entire array with a change to one element would be ludicrously unwiedly. Instead nested data structures like lists are provided. 

### Functions as values

* In many imperative languages, sub-programs may be passed as actual parameters to other sub-programs but it is rare for an imperative language to allow sub-programs to be passed back as result. In functional languages, functions may construct new functions and pass them on to other functions. **Functional languages allow functions to be treated as values. Functions are first class citizens**
