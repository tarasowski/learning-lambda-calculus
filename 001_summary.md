# An Introduction To Functional Programming Through Lambda Calculus

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

* From another book (Learn Haskell Book): The lambda calculus has three basic components, or lambda terms: expressions, variables, and abstractions. The word expression refers to a superset of all those things: an expression can be a varible name, an abstraction, or a combination of those things. 
    * **The simplest expression is a single variable.** Variables here have no meaning or value; they are only names for potential inputs to functions.
    * **An abstraction is a function.** It is a lambda term that has a head (a lambda) and a body and is applied to an argument. An argument is an input value. Abstractions consist of two parts: the *head* and the *body* The head of the function is a `𝜆`(lambda) followed by a variable name. The body of the function is another expression. So, a simple function might look like this: `𝜆𝑥.𝑥`. The variable named in the head is the **parameter** and **binds** all instances of that same variable in the body of the function. That means, when we apply this function to an argument, each `x`in the body of the function will have the value of that argument.
    * **The act of applying a lambda function to an argument is called application** and application is the lynchpin of the lambda calculus.

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
* The order in which A(D), B(D), C(D) carried out does not matter, beacuse functions A, B and C cannot change it's common actual parameter D. 

> In functional languages there is no necessary exexution order. 

**Note:** Of course, functional programs must be executed in some order  - all programs are - but the order does not affect the final result. This execution independence in one of the strengths of functional programming. **Probably in distributed systems, where each function can run in parallel and independent**

### Repetition in imperative and functional languages

* In imperative languages command may change the values associated with a names by previous commands so a new name is not necessarily introduced for each new command. In order to carry out several commands several times, those commands need to be duplicated. Instead, the same commands are repeated. Each name may be and usually will be associated with different values while a program is running.

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

// The sum is found through successive recursive calls to SUM:
B[1] + SUM(B, 2, M) = B[1] + B[2] + SUM(B, 3, M)
B[1] + B[2] + ... + B[M] + SUM(B, M+1, M)
B[1] + B[2] + ... + B[M] + 0
```

* **Note:** Each recursive call to SUM creates new local versions of A, I and N, and the previous versions become inaccessible. At the end of each recursive call, the new local variable are lost, the partial sum is returned to the previos call and previous local variable come back to use. 

* In functional languages, new values are associated with new names through recursive function call nesting.

### Data structures in functional languages

* In imperative languages, array elements and record fields are changes by succesive assignments. In functional languages, because here is no assignemnt, sub-structures (an underlying or supporting structure) in data structures cannot be changes one at a time. Instead, it is necessary to write down a whole structure with explicit changes to the appropriate sub-structure. 

**Note:** Functional languages provide explicit representations for data structures. 

* Functional languages do not provide arrays because without assignment there is no easy way to access an arbitrary element. Writing out an entire array with a change to one element would be ludicrously unwiedly. Instead nested data structures like lists are provided. 
r.
### Functions as values

* In many imperative languages, sub-programs may be passed as actual parameters to other sub-programs but it is rare for an imperative language to allow sub-programs to be passed back as result. In functional languages, functions may construct new functions and pass them on to other functions. **Functional languages allow functions to be treated as values. Functions are first class citizens**

### Origins of functional languages

* An important difference between Turing machines, and recursive functions and lambda calculus is that the Turing machine approach concentrated on computation as mechanised symbol maniputalion based on assignment and time ordered evaluation. Resurive function theory and lambda calculus emphasised computation as structured function application. They both are evaluation order independent. 

### Lambda Calculus

* The lambda calculus is a suprisingly simple yet powerful system. It is based on function **abstraction**, to generalise expressions through the introduction of names, and function **application**, to evaluate generalised expressions by giving names particular values. 

* Abstraction and application are all that are needed to develop representations for arbitrary programming language construct. Thus, lambda calculus can be treate as a universal machine code for programming languages.

**Note:** In this book we are going to use lambda calculus to explore functional programming. Pure lambda calculus does not look much like a programming language. Indeed, all it provides are names, function abstraction and function application. **However, it is straightforward to develop new language constructs from this basis.** Here we will use lambda calculus to construct step by step a compact, general purpose functional programming notation such as pairs of object, boolean values and operations, numbers, conditional expressions, recursive functions, types and typed representations for boolean values, numbers and characters, representations of lists, linear list processing...

* Greek letters are used in naming lambda calculus concepts:

```
α - alpha 
β - beta
λ - lambda 
η - eta
```
* Syntatic construcs are defined using BNF **rules.** Each rule has a **rule name** consisting of one or more words within angel brackets `< and >.` A rule associates its name with a **rule body** consisting of a sequence of symbols and rule names. If there are different possible rule bodies for the same rule then they are separated by | s.

* For example, binary numbers are based on the digits 1 and 0:

```
<digit> ::= 1 | 0
```
* and a binary number may be either a single digit (any of the numerals from 0 to 9) or a digit followed by a number:

```
<binary> ::= <digit> | <digit> <binary>
```

## Lambda Calculus

### Abstraction

* Abstraction is central to problem solving and programming. It involves generalisation from concrete instances of a problem so that a general solution may be formulated. A general, abstract, solution may then be used in turn to solve particular, concrete instances (a particular case) of the problem. 

* The simplest way to specify an instance of a problem is in terms of particular concrete operations on particular concrete object. Abstraction is based on the use of names to stand for concrete objects and operations to generalise the instances. A generalised instance may subsequently be truned into particular instance by replacing the names with new concrete objects and operations.

* Consider buying 9 items at 10 pence each: The total cost is: 10 * 9. Here we are carrying out the concrete operation of multiplication on the concrete values 10 and 9. Now consider buying 11 items at 10 pence each. The total cost is: 10 * 11. Here we are carrying out the concrete operation of multiplication on the concrete values 10 and 11. 

* We can see that as the number of items changes so the formula for the total cost changes at the place where the number of items appear. We can **abstract over** the number of items in the formula by introducing a name to stand for a general number of items, say `items`:

```
10 * items
```

* We might make this abstraction explicit by preceding the formula with the name used for abstraction:

```
REPLACE items IN 10 * items
```
* Here we have abstracted over an operand in the formula. To evaluate this abstraction we need to supply a value for the name. For example, for 84 items:

```
REPLACE items WITH 84 IN 10 * items
```
* which gives 10 * 84. We made a **function** from a formula by replacing an object with a name and identifying the name that we used. We have then **evaluated** the function by replacing the name in the formula with a new object and evaluation the resulting formula. 

* Lets use abstraction again to generalise our example further. Suppose the cost of items goes up to 11 pence. Now the total cost of items is:

```
REPLACE items IN 11 * items
```

* Suppose the cost of items drops to 9 pence. Now the total cost of items is:

```
REPLACE items IN 9 * items
```

* Becasue the cost changes, we could also introduce a name to stand for the cost in general, say `cost`:

```
REPLACE cost IN
    REPLACE items IN cost * items
``` 
* Here we have abstracted over two operands in the formula. To evaluate the abstraction we need to supply two values. For example, 12 items at 32 pence will have total cost:

```
REPLACE cost WITH 32 IN
    REPLACE items WITH 12 IN cost * items

// which is
REPLACE items WITH 12 IN 32 * items

// which is 
32 * 12
```

* Suppose we now want to solve a different problem. We are given the total cost and the number of items and we want to find out how much each item costs. For example, if 12 items cost 144 pence then each item costs:

```
144/23
```

* In general, if `items`items `cost` pence then each item costs:

```
REPLACE cost IN
    REPLACE items IN cost/items
```

* Now, compare this with the formula for finding a total cost:

```
REPLACE cost IN
    REPLACE items IN cost * items
```

* They are the same except for the opration `/` in finding the cost of each item and `*`in finding the cost of all items. We have two instances of a problem involving applying an operation to two operands (the quantity on which an operation is to be done). We could generalize the instances by introducing a name, say `op`, where the operation is ued:

```
REPLACE op IN
    REPLACE cost IN
        REPLACE items IN cost op items
```
* Now, finding the total cost will require the replacement of the opration name with the conceret `multiplication`operation:

```
REPLACE op WITH * IN
    REPLACE cost IN
        REPLACE items IN cost op items

// which is

REPLACE cost IN
    REPLACE items IN cost * items
```

* Similarly, finding the cost of each item will require the replacement of the operation name with the concrete `division` operation:

```
REPLACE op WITH / IN
    REPLACE cost IN
        REPLACE items IN cost op items

// which is

REPLACE cost IN
    REPLACE items IN cost / items
```

* **Important:** Abstraction is based on generalisation through the introduction of a name to replace a value and specialisation through the replacement of a name with another value. 

**Note:** Note that care must be taken with generalisation and replacement to ensure that names are replaced by object of appropriate types. For example, in the above examples, the operand names must be replaced by numbers and the operator name must be replaced by an operation with two number arguments.

### Abstraction in progamming languages

* Abstraction lies at the heart of all programming languages. In imperative languages, variables as name/value associations are abstractions for computer memory locations based on specific address/value associations. 

* Abstractions my be subject to further abstraction. This is the basis of hierarchical program design methodologies and mudularity. 

* For example, Pascal procedures are abstractions for sequences of statements, named by **procedure** declarations, and functions are abstractions for expressions, named by function declarations. Procedures and functions, declare formal parameters which identify the names used to abstract in statement sequences and expressions. Actual parameters specialise procedures and functions. Procedure calls with actual parameters invoke sequence of statements with formal parameters replaced by actual parameters. Similarly, function calls with actual parameters evaluate expressions with formal parameters replaced by actual parameters. 

* Arays are abstractions for sequence of variables of the same type. 

### Lambda Calculus

* Lambda calculus is a model for computability and has subsequently been central to contemporary computer science. **It is a very simple but very powerful language based on pure abstraction.** It can be used to formalise all aspects of programming languages and programming and is particularly suited for use as a `machine code` for functional languages and functional programming. 

### Lambda Expressions

* The lambda calculus is a system for manipulating lambda expressions. A lambda expression my be a **name** to identify an abstraction point, a **function** to introduce an abstraction or a **function application**

1) A **name** when instead of values we use names such as `items, const`. Just a simple name without any meaning or value.
2) A **function** when we generalise over a specific problem such as total costs `item * cost``
3) A **function application** when we replace the names with their actual values `32 * 12`

```
// rule name ::= rule body
<expression> ::= <name> | <function> | <application>
```

* A name may be any sequence of non-blak characters, for example:

```
fred legs-11 19th_nervous_breakdown 33 + -->
```

* A lambda function is an abstraction over a lambda expression and has the form:

```
<function> ::= λ<name>.<body>
```

* where 

```
<body> ::= <expression>
```
* for example:

```
λx.x λfirst.λsecond.first λf.λa.(f a)
```
* The `λ`precedes and introduces a name used for abstraction. The name is called the function's `bound variable` and is like a formal parameter. The `.`separates the name from the expression in which abstraction with that name takes place. This expression is called the function's `body`. 

**Note:** The body expression may be any `λ`expression including another function. Note that function do not have names! For example, in Pascal, the function name is always used to refer to the function's definition.

* A function application has the form:

```
<application> ::= (<function expression> <argument expression>)
```
* where

```
<function expression> ::= <expression>
<argument expression> ::= <expression>
```

* for example

```
(λ.x.x λa.λb.b)
```

* A function application specialises an abstraction by providing a value for the name. The function expression contains the abstraction to be specialied with the argument expression. 

* In a function application, also known as a **bound pair**, the function epxression is said to be **applied to** the argument expression. This is like a function call in Pascal where the argument expression corresponds to the actual parameter. The crucial difference is that in Pascal the function name is used in the function call and the implementation picks up the corresponding definition. The λ calculus is far more general and allows function defintions to appear directly in function calls. 

* There are two approaches to evaluationg function applications. For both, the function expression is evaluated to return a function. Next, all occurences of the function's bound variable in the function's body expresion are preplaces by *either*: the value of the argument expression *or* the unevaluated argument expression. Finally, the function body expression is then evaluated. 

* The first approach is called **applicative order** and is like Pascal 'call by value': the actual parameter expression is evaluated before being passed to the formal parameter.

* The second approach is called **normal order** and is like 'call by name' in ALGOL 60: the actual parameter expression is not evaluated before being passed to the formal parameter.

**Note:** Normal order is more powerful than applicative order but may be less efficient. For the moment, all function applications will be evaluated in normal order. The syntax allows a single name as a `λ`expression but in general we will restrict single names to the bodies fo functions. This is so that we can avoid having to consider names as objects in their own right, like LISP or Prolog literals as it complicates the presentation.

### Identitfy function

```
λx.x
```
* This is the identity function which returns whatever argument it is applied to. Its bound variable (parameter) is: `x` and its body expression is the name `x`. When it is used as the function expression in a function application the bound variable `x` will be replaced by the argument expression in the body expression `x` giving the original argument expression.

* Suppose the identity function is applied to itself. This is a function application with: `λx.x` as the function expressin and `λx.x` as the argument expression.

```
(λx.x λx.x)
```

* When this application is evaluated, the bound variable `x` for the function expression `λx.x`is replaced by the argument expression: `λx.x`in the body expression `x` giving `λx.x`

* An identity operation always leaves its argument unchanged. In arithmetic, adding or subtracting 0 are identity operations. For any `<number>`:

```
<number> + 0 = <number>
<number> - 0 = <number>
```

* Multiplying or diving by 1 are also identity operations:

```
<number> * 1 = <number>
<number> / 1 = <number>
```
* The identity function is an identity operation for `λ` functions.

* We could equally well have used different names for the bound variable, for example: `λa.a` or `λyibble.yibble` to define other versions of the identity function. 

### Self application function

* Consider the rather odd function:

```
λs.(s s)
```
* which applies its argument to its argument. The bound variable is `s` and the body expression is the function application: `(s s)`, which has the name `s` as function expression and the same name `s` as argument expression. Here is the application of the identity function to it:

```
(λx.x λs.(s s))
```

* In this application, the function expression is `λx.x` and the argument expression is `λs.(s s)`, when this application is evaluated, the function expression bound variable: `x` is replaced by the argument: `λs.(s s) in the function expression body: `x`giving `λs.(s s)` which is the original argument.

* Application of self application function to the identity function: 

```
(λs.(s s) λx.x)
```

* Here the function expression is `λs.(s s)` and teh argument expression is `λx.x`. When this application is evaluated, the function expression bound variable: `s` is the function expression body: `(s s)` giving a new application: `(λx.x λx.x)`with function expression: `λx.x`and argument expression: `λx.x`. This is now evaluated as above giving the final value: `λx.x`

* Consider the application of the self application function to itself:

```
(λs.(s s) λs.(s s))
```

* This application has function expression `λs.(s s)` and argument expression: `λs.(s s)` To evaluate it, the function expression bound variable: `s` is replaced by the argument: `λs.(s s)`in the function expression body `(s s)`giving a new application `(λs.(s s) λs.(s s))`whith function expression: `λs.(s s)`and argument expression `λs.(s s)`which is then evaluated. The function expression bound variable: `s`is replaced by teh argument: `λs.(s s)`in the function expression body: `(s s)`giving the new application: `λs.(s s) λs.(s s)` which is then evaluated... **Each application evaluates to the original application so this application never terminates!** 

### Function application function

```
λfunc.λarg.(func arg)
```
* This function has bound variable `func`and the body expression is another function: `λarg.(func arg)` which has bound variable `arg`and the a function application `(func arg)` as body expression. This in turn has the name `func` as function expression and the name `arg`as argument expression.

* When used, the whole function returns a second function which then applys the first function's argument to the second function's argument. For example, lets apply the identity functon to the self application function:

```
((λfunc.λarg.(func arg) λx.x) λs.(s s))
```
* In this example of an application, the function expression is itself an application: `(λ.func.λarg.(func arg) λx.x)` which must be evaluated first. The bound variable `func` is replaced by the argument `λx.x` in the body `λarg.(func arg)` giving `λarg.(λx.x arg)` which is a new function which applies the identity function to its argument. The original expression is now: `(λarg.(λx.x arg) λs.(s s))` and so the bound variable: `arg` is replaced by the argument: `λs.(s s)` in the body: `(λx.x arg)` giving: `(λx.x λs.(s s))`which is now evaluated. The bound variable: `x` is replaced by the argument `λs.(s s)` in the body `x` giving: `λs.(s s)`

### Introducing new syntax

* As our λ expression became more elaborae they become harder to work with. To simplify working with λ expressions and to construct a higher level functional language we will allow the use of more concise notations. For example, in this and subsequent chapeters we will introduce named function definitions, infix operations, an IF style conditional expression and so on. This additional of higher level layers to a language is known as `syntactic sugaring` because the representation of the language is changed but the underlying meaning stays the same.

* The new syntax for commonly used constructs will be introduced through substitution (the action of replacing someone or something with another person or thing) rules. The application of these rules won't involve making choices. Their use will lead to pure λ expressions after a finite number of steps involving simple substitutions. this is to ensure that we can always 'compile' completely a higher level representation into λ calculus before evaluation. Then we only need to refer to our original simple λ calculus rules for evaluation. 

### Notation for naming functions and application reduction

* It is a bit tedious writing functons out over and over again. We will now name functions using: `def <name> = <function>` to define a name/function association. For example, we could name the functions we looked at in the previous sections:

```
def identity = λx.x
def self_apply = λs.(s s)
def appy = λfunc.λarg.(func arg)
```
**Note:** Now we can just use the `<name>` in expressions to stand for the `<function>`. Strictly speaking, all defined names in an expression should be replaced by their defintions before the expression is evaluated. However, for now we will only replace a nmae by its associated function when the name is the function expression of an application. We will use the notation s. below to indicate the replacement of a `<name>` by its associated `<function>

```
(<name> <argument>) == (<function> <argument>)
```

* Formally, the replacement of a bound variable with an argument in a function body is called `β reduction (beta reduction)`. In future, instead of spelling out each `β` reduction below by blow we will introduce the notaton: `(<function> <argument>) => <expression>` to mean that the `<expression>` results from the application of the `<function>`to the `<argument>`

* When we have seen a sequence of reduction before or we are familiar with the functions involved we will omit the reduction and write: `=> ... =>` to show where they should be.

### Functions from functions

* We can use the self application function to build version of other functions. For example, lets define a function with the same effect as the identity function `def identity2 = λx.((apply identity) x). Let us apply this to the identity function: 

```
(identity2 identity) == (λ.x.((apply identity) x) identity) =>
((apply identity) identity) ==
((λfunc.λarg.(func arg) identity) identity) =>
(λarg.(identity arg) identity) =>
(identity identity) => ... =>
identity
```

* Let us show that `identity` and `identity2`are equivalent. Suppose: `<argument>` stands for any expression. Then: 

```
(identity2 <argument>) ==
(λx.((apply identity) x) argument) =>
((apply identity) <argument>) => ... =>
(identity <argument>) => ... =>
<argument>
```
* so `identity` and `identity2`have the same effect.

* We can use the function application function to define a function with the same effect as the function application function itself. Suppose: `<function>` is any function. Then:

```
(apply <function>) ==
(λf.λa.(f a) <function>) =>
λa.(<function> a)
```
* Applying this to any argument: `<argument>` we get: 

```
(λa.(<function> a) <argument>) =>
(<function> <argument>)
```

* which is the application of the original functon to the argument. Using apply adds a layer of β reduction to an application. We can also use the function application function slightly differently to define a function with the same effect as the self application function: `def self_apply2 = λs.((apply s) s)` 

* Lets apply this to the identity function:

```
(self_apply2 identity) ==
(λs.((apply s) s) identity) =>
((apply identity) identity) =>
(identity identity) => ... =>
identity
```

In general, applying `self-apply2` to any argument: `<argument>` gives: 

```
(self_apply2 <argument>) ==
(λs.((apply s) s) <argument>) =>
((apply <argument>) <argument>) => ... =>
(<argument> <argument>)
```

so `self_apply` and `self_apply2` have the same effect.

### Argument selection and argument pairing functions

* We are now going to look at functions for selecting arguments in nested function applications. We will use these functions a great deal later on to model boolean logic, integer artithmetic and list data structures. 

### Selecting the first of two arguments

* Consider the function `def select_first = λ.first.λsecond.first`. This function has bound variable: `first` and body: `λsecond.first` When applied to an argument, it returns a new function which when applied to another argument return the first argument. For example:

```
((select_first identity) apply) ==
((λfirst.λsecond.first identity) apply) =>
(λ.second.identity apply) =>
identity
``` 
* In general, applying `select_first`to arbitrary arguments: `<argument1>` and `<argument2>` returns the first argument: 

```
((select_first <argument1>) <argument2>) ==
((λfirst.λsecond.first <argument1>) <argument2>) =>
(λsecond.<argument1> <argument2>) =>
<argument1>
```

### Selecting the second of two arguments

Consider the function `def select_second = λ.first.λsecond.second`. This function has bound variable: `first`and body `λsecond.second` which is another version of identity function. When applied to an argument `select_second` returns a new function wich when applied to another argument returns the other argument. For example:

```
((select_second identity) apply) ==
((λfirst.λsecond.second identity) apply) =>
(λsecond.second apply) =>
apply
``` 
* The first argument `identity` was lost because the bound variable `first` does not apper in the body `λsecond.second`. In general, applying `select_second` to arbitrary arguments: `<argument1>`and: `<argument2>`returns the second argument:

```
((select_second <argument1>) <argument2>) ==
((λfirst.λsecond.second <argument1>) <argument2>) =>
((λsecond.second <argument2>) =>
<argument2>
```

* We can show that `select_second` applied to anything returns a version of `identity`
