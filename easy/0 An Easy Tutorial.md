# Welcome to the nJexl Easy Tutorial

## Contents 

* [Overview](#overview) 
    * [About nJexl](#about-njexl)
    * [nJexl Vs. Java](#njexl-vs-java)
* [Environment Setup](#environment-setup)
* [Basic Syntax](#basic-syntax) 
    * [First Program](#first-program)
* [Data Types](#data-types)
* [Variables](#variables) 
* [Operators](#operators)  
* [If/Elses](#if-and-else) 
* [Loops](#loops) 
    * [The Infamous Goto](#goto)
    * [While](#while)
    * [For](#for)
    * [Continue](#continue)
    * [Break](#break)
* [Functions](#functions)
     * [Definition](#function-definition) 
     * [Anonymous Functions](#defined-as-variables)
* [Closures](#closures) 
    * [Partial Function](#partial-function)
* [I/O](#input-output)
     * [Read](#read)
     * [Write](#write)
* [Special Functions](#special-functions)
     * [Collections](#collections)
     * [Avoiding Iteration](#avoiding-iteration)
* [Objcts and Classes](https://github.com/nmondal/njexl/wiki/11-Objects-And-Related-Concepts)
* [Sample Programs](#sample-programs)
     * [Measuring Speed of Execution](#speed-of-execution)
     * [FizzBuzz](#fizz-buzz)
     * [Scramble](#scramble)
     * [Next Higher Permutation](#next-higher-permutation)
     * [Random No. Generation](#random-computing)


## Overview 

nJexl, short for new Java Expression Language, is a hybrid functional programming language. 
It was created with business programming ( business logic ) and test automation validaition 
in mind.

nJexl smoothly integrates features of object-oriented and functional languages and is interpreted to run on the Java Virtual Machine. 

### About nJexl

Here is the important list of features, which make nJexl a first choice of the business developers.

#### nJexl is Embeddable in Java Code

nJexl scripts are easy to be invoked as stand alone scripts, also from withing java code, 
thus making integration of external logic into proper code base easy. Thus Java code
can call nJexl scripts very easily, and all of nJexl functionality is programmatically 
accessible by Java caller code. This makes it distinct from Scala, where it is almost impossible
to call scala code from Java. Lots of code of how to call nJexl can be found 
in the [test](https://github.com/nmondal/njexl/tree/master/lang/src/test/java/com/noga/njexl/lang) directory. Many scripts are there as sample in [samples](https://github.com/nmondal/njexl/tree/master/lang/samples) folder.

##### nJexl gets interpreted on the JVM
nJexl is interpreted by a Java runtime. This means that nJexl and Java have a common runtime platform. You can easily move from Java to nJexl and vice versa.

##### nJexl can Execute any exisiting Java Code
nJexl enables you to use all the classes of the Java SDK's in nJexl, and also your own, custom Java classes, or your favourite Java open source projects.


#### nJexl is functional
nJexl is also a functional language in the sense that every function is a value and because every value is an object so ultimately every function is an object.

nJexl provides a lightweight syntax for defining anonymous functions, it supports higher-order functions, it allows functions to be nested, and supports [currying](https://en.wikipedia.org/wiki/Currying). 

#### nJexl is dynamically typed
nJexl, unlike other statically typed languages, does not expect you to provide type information. You don't have to specify a type in most cases, and you certainly don't have to repeat it.


### nJexl vs Java

nJexl has a set of features, which differ from Java. Some of these are:

* All types are objects.

* Type inference.

* Nested Functions.

* Functions are objects.

* Closures.

[Back to Contents](#contents)

In case you want to delve down further, read the [complete wiki](https://github.com/nmondal/njexl/wiki).

## Environment Setup

#### Install a Java Runtime 
 You need to install Java runtime 1.8 ( 64 bit ).
 To test whether or not you have successfully installed it try this 
 in your command prompt :

    $ java -version
    java version "1.8.0_60"
    Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
    Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)

#### Download nJexl one jar 

Download the latest one-jar.jar file from [here](https://oss.sonatype.org/content/repositories/snapshots/com/github/nmondal/njexl.lang/0.3-SNAPSHOT/).

#### Add it Up to Path  

If you are using \*nix platform, then you should create an alias :

     alias njexl = "java -jar njexl.lang-0.3-<time-stamp>-onejar.jar" 

in your .login file.

If you are using Windows, then you should create a batch file that looks like this:

     @echo off
     rem njexl.bat 
     java -jar njexl.lang-0.3-<time-stamp>-onejar.jar  %1 %2 %3 %4 %5 %6

and then add path to this njexl.bat file in your path variable, see [here](https://www.google.co.in/search?client=safari&rls=en&q=adding+path+in+windows&ie=UTF-8&oe=UTF-8&gfe_rd=cr&ei=EWpIVqjUJ-PI8AfWm4GgBQ).

#### Test Setup 

Open a comamnd prompt, and type :

     $njexl 
     (njexl)

It should produce the prompt of [REPL](https://en.wikipedia.org/wiki/Read–eval–print_loop) of (njexl).

[Back to Contents](#contents)


## Basic Syntax 

If you have some understanding on C,C++, Java, then it will be very easy for you to learn nJexl. The biggest syntactic difference between nJexl and other languages is that the ';'' statement end character is optional. When we consider a nJexl program it can be defined as a collection of objects that communicate via invoking each others methods. 

#### Methods 
A method is basically a behavior. A class can contain many methods. It is in methods where the logics are written, data is manipulated and all the actions are executed.

#### Object 
 Objects have states and behaviors. Example: A dog has states - color, name, breed as well as behaviors - wagging, barking, eating. An object is an instance of a class.

#### Class 
A class can be defined as a template/blueprint that describes the behaviors/states that object of its type support.

#### Fields 
Each object has its unique set of instant variables, which are called fields. An object's state is created by the values assigned to these fields.

### First Program

#### Interactive Mode Programming
Invoking the interpreter without passing a script file as a parameter brings up the following prompt:

    C:\>njexl
    (njexl)write("Hello, nJexl!")
    Hello, nJexl!
    =>null
    (njexl)

#### Script Mode Programming

    /* This is my first nJexl program.  
    * This will print 'Hello World' as the output
    */
    write('Hello, nJexl!')

Save this as a file "tmp.jxl" and run :

    C:\>njexl tmp.jxl
    Hello, nJexl!
    C:\>

[Back to Contents](#contents)

## Basic Syntax

#### Case Sensitivity 
nJexl is case-sensitive, which means identifier Hello and hello would have different meaning.

#### Method & Class 
Are defined with "_def_"  keyword.

#### Identifiers

All nJexl components require names. Names used for objects, classes, variables and methods are called identifiers. A keyword cannot be used as an identifier and identifiers are case-sensitive. There are following four types of identifiers supported :

##### Alphanumeric identifiers
An alphanumeric identifier starts with a letter or underscore, which can be followed by further letters, digits, or underscores. The '$' character is a reserved keyword in nJexl and should not be used in identifiers. Following are legal alphanumeric identifiers:

     age, salary, _value,  __1_value

Following are illegal identifiers:

123abc, -salary

##### Operator identifiers
An operator identifier consists of one or more operator characters. Operator characters are printable ASCII characters such as +, :, ?, ~ or #. Following are legal operator identifiers:

     + += : 

#### Keywords:

Some are reserved words, and can not be used as identifiers.
Some are semi reserved, and can be used as identifiers, for example *me* and *my*.

##### Reserved Kewords

     if  else where while for import as continue break size empty #def def 


#### Comments 

    /* This is my first nJexl program.  
    * This will print 'Hello World' as the output
    */
    write('Hello, nJexl!') // writes the string back , line comment 
    ## This is also another line comment 


#### Newline Characters:
nJexl is a line-oriented language where statements may be terminated by semicolons (;) or newlines. 
A semicolon at the end of a statement is usually optional. You can type one if you want but you don't have to if the statement appears by itself on a single line. On the other hand, a semicolon is required if you write multiple statements on a single line:

    s = "hello"; write(s)

[Back to Contents](#contents)

## Data Types

nJexl is untyped in most cases. That actually means that the interpreter handles the type for you,
and you dont need to bother about it, for 99.999999% of the cases. But for some cases you need types, 
hence some types can be converted :

     byte, char, short, int, long, float, double, big int, big decimal, string, method , class 

Are the types supported. There is no way one can be sure about the type assigned, and there is no type.

#### Basic Literals 

The rules nJexl uses for literals are simple and intuitive. 
This section explains all basic Literals.

##### Integer Literals
Integer literals are usually of type Int, or of type Long when followed by a L or l suffix. Here are some integer literals:

    0 // simple in decimal 
    035 // in octal 
    21 
    0xFFFFFFFF  // in hex 
    0777L // octal decimal Long 

##### Floating Point Literals
Floating point literals are of type Float when followed by a floating point type suffix F or f, and are of type Double otherwise. Here are some floating point literals:

    0.0 
    1e30f 
    3.14159f 
    1.0e100
    .1

##### Boolean Literals

The boolean literals _true_ and _false_ are members of type Boolean.

##### String Literals

A string literal is a sequence of characters in  single or double quotes. 
The characters are either printable unicode character or are described by escape sequences. Here are some string literals:

    "Hello,\nWorld!"
    "This string contains a \" character."

##### The special *null* literal 

_null_ is a specific literal, equivalent to C,C++ NULL and Java's null.

##### List Literal 

A list / array literal is defined as :

     l = [ 1 , 2, 3, 4 , 'foo' , x ]
     y = l[3] // y is assigned 4, 0 based index  

##### Dict Literal 

A Dict literal is defined as :

     h  = { 1 : 2, 3 : 4 , 'foo' :  x }
     x = h[1] // x is assigned 2 

[Back to Contents](#contents)

## Variables 

Variables are nothing but reserved memory locations to store values. This means that when you create a variable you reserve some space in memory.

Based on the data type of a variable, the interpreter allocates memory and decides what can be stored in the reserved memory. Therefore, by assigning different data types to variables, you can store integers, decimals, or characters in these variables.

#### Variable Declaration

nJexl has the different syntax for the declaration of variables and they can be defined as valueFollowing is the syntax to define a variable using var keyword:

     var x = 0 // this is a global variable 

Local variables can be defined using : 

     z = 0 // local variable 

There is no data type, of course one can force-cast it :

     z = int( '34' ) // z is integer 34 

##### Multiple assignments

     #(a,b,c,d) = [  10 , '20' , true , null ]

Now a is 10, b is string '20' , c is _true_ and d is set to _null_ value.

##### Method Parameters

Method parameters are variables, which are used to pass the value inside a method when the method is called.Method parameters are only accessible from inside the method but the objects passed in may be accessible from the outside, if you have a reference to the object from outside the method. Method parameters are always mutable.


##### Local Variables

Local variables are variables declared inside a method. Local variables are only accessible from inside the method, but the objects you create may escape the method if you return them from the method. 

[Back to Contents](#contents)

## Operators

An operator is a symbol that tells the compiler to perform specific mathematical or logical manipulations. nJexl is rich in built-in operators and provides following type of operators:

* Arithmetic Operators

* Relational Operators

* Logical Operators

* Bitwise Operators

* Assignment Operators

This chapter will examine the arithmetic, relational, logical, bitwise, assignment and other operators one by one.

#### Arithmetic Operators

There are following arithmetic operators supported :
Assume variable A holds 10 and variable B holds 20, then:



| Operator  | Description  | Example  |
|---|---|---|
| + | Adds two operands  |  A + B will give 30 |
| - | Subtracts second operand from the first | A - B will give -10 |
| * | Multiplies both operands |  A * B will give 200 |
| / | Divides numerator by de-numerator |  B / A will give 2 |
| % | Modulus Operator and remainder of after an integer division | B % A will give 0 |


#### Relational Operators

Relational operators are to be used over neumericals, also between collections.

##### Neumerical Relations

Assume variable A holds 10 and variable B holds 20, then:

| Operator  | Description  | Example  |
|---|---|---|
|== or eq |Checks if the values of two operands are equal or not, if yes then condition becomes true | (A == B) is not true |
| !=  or ne| Checks if the values of two operands are equal or not, if values are not equal then condition becomes true| (A != B) is true|
|>  or gt | Checks if the value of left operand is greater than the value of right operand, if yes then condition becomes true| (A > B) is not true|
|<  or lt | Checks if the value of left operand is less than the value of right operand, if yes then condition becomes true|  (A < B) is true|
|>=  or ge | Checks if the value of left operand is greater than or equal to the value of right operand, if yes then condition becomes true |(A >= B) is not true|
|<= or le |Checks if the value of left operand is less than or equal to the value of right operand, if yes then condition becomes true |(A <= B) is true|


#### Logical Operators

There are following logical operators supported:

Assume variable A holds _true_ and variable B holds _false_ , then:

| Operator  | Description  | Example  |
|---|---|---|
| &&  or and  | Logical AND operator : when both the operands are true then condition becomes true | (A && B) is false |
| \|\| or  or | Logical OR Operator : when any of the two operands is non zero then condition becomes true | (A \|\| B) is true |
| ! or not  | Logical NOT Operator : Reverses the logical state of its operand. If a condition is true then Logical NOT operator will make false.|!(A && B) is true|


#### Assignment Operators

| Operator  | Description  |
|---|---|
| = | C = A + B will assign value of A + B into C|
| +=| C += A is equivalent to C = C + A|
| -=| C -= A is equivalent to C = C - A|


#### Collection Operations

Over collections one sometime needs to operate upon for various reasons.


##### Arithmetic

One may need to add an element in a list:

    (njexl)l = [1,2,3,4]
    =>@[1, 2, 3, 4]
    (njexl)l + 6
    =>[1, 2, 3, 4, 6]
    (njexl)l
    =>@[1, 2, 3, 4]

We note that nJexl philosophy says that the collections are immutable by definition.
Thus, the array *l* was not modified, but a new list was created. We can easily fix it by :

    (njexl)l += 6
    =>[1, 2, 3, 4, 6]
    (njexl)l
    =>[1, 2, 3, 4, 6]

Thus we understand that ordinary *+* and *-* acts like addition and subtraction of items from a collection.
In the same spirit :

    (njexl)l
    =>[1, 2, 3, 4, 6]
    (njexl)l -= 3
    =>[1, 2, 4, 6]
    (njexl)l
    =>[1, 2, 4, 6]


###### Multiplication, Join 

Collections can be multiplied, that is termed as join:
For example :

    (njexl)b = [0,1]
    =>@[0, 1]
    (njexl)b * b
    =>[[0, 0], [0, 1], [1, 0], [1, 1]]

This generated all possible tuple of binary 2 valued numbers.
Collections can be multiplied again and again :

    (njexl)b * b * b
    =>[[0, 0, 0], [0, 0, 1], [0, 1, 0], [0, 1, 1], [1, 0, 0], [1, 0, 1], [1, 1, 0], [1, 1, 1]]

Or rather :

    (njexl)b * B* b
    =>[[0, true, 0], [0, true, 1], [0, false, 0], [0, false, 1], [1, true, 0], [1, true, 1], [1, false, 0], [1, false, 1]]


##### Relations between Collections

Suppose we have  E =[] ,  A = [1,2,3] , AA = [2,1,3] ,  B = [2,3,4,1 ] , D = [0,10] 
Now, 

| Operator  | Description  | Example  |
|---|---|---|
|== or eq |Checks if the two operands are *same* collection | (A == B) is false while (A == AA) is true |
|!= or ne| Checks if the values of two operands are not *same* collection | (A != B) is true while (A != AA) is false |
|> or gt | Checks if the left operand is a *strict super collection* of the right operand | (B > A) is true while (E > A)is false |
|< or lt | Checks if the left operand is a *strict sub collection* of the right operand|  (A < B) is true while (A < E) is false |
|>= or ge | Checks if the left operand is *super collection* of the right operand |(A >= AA ) is true but ( E >= A ) is false |
|<= or le |Checks if the left operand is *sub collection* of the right operand |(A <= B) is true, so is (A<=A) true but (A <= E ) is false |

###### Inversion of Operators

If this relation *A OP B* is true, that does not guarantee that the inverse operator would be false, 
for collection relations. For example :

     D < A // is false 
     D > A // is false 
     D == A // is false 
     D != A // is true 


This happens because D,A are not comparable collection wise. 
Note that empty collections, i.e. *E* is always everyones *sub collection*.


##### Set Like Operations 

Suppose we have  E =[] ,  A = [1,2,3] , AA = [2,1,3] ,  B = [2,3,4,1 ] , D = [0,1,10] 

| Operator  | Description  | Example  |
|---|---|---|
| &  | Intersection of two collections | (A & B) is [1,2,3]  |
| \| | Union of two collections | (A \| D ) is [0,1,2,3,10]  |
| - | Collection subtraction |  (B - A) = [4]  while (A - B) = [] |
| ^ | Collection symmetric difference |  (A-D) | (D-A) :: means : (A^D) = [0, 2, 3, 10] |


[Back to Contents](#contents)

## If and Else

Following is the general form of a typical decision making IF...ELSE structure found in most of the programming languages:

![Conditional](http://www.tutorialspoint.com/scala/images/scala_decision_making.jpg)

#### The if Statement
An if statement consists of a Boolean expression followed by one or more statements.

##### Syntax
The syntax of an if statement is:

    if ( Boolean_expression ) {
       // Statements will execute if the Boolean expression is true
    }

If the boolean expression evaluates to true then the block of code inside the if statement will be executed. If not, the first set of code after the end of the if statement (after the closing curly brace) will be executed.

##### Example

      x = 10
      if( x < 20 ){
         write("This is if statement")
      }

This would produce following result:

     This is if statement


#### The if...else Statement

An if statement can be followed by an optional else statement, which executes when the Boolean expression is false.

##### Syntax
The syntax of a if...else is:

    if( Boolean_expression ) {
       //Executes when the Boolean expression is true
    } else {
       //Executes when the Boolean expression is false
    }

##### Example:

      var x = 30;
      if( x < 20 ){
         write("This is if statement");
      }else{
         write("This is else statement");
      }

This would produce the following result:

    This is else statement

#### The if...else if...else Statement:

An if statement can be followed by an optional else if...else statement, which is very useful to test various conditions using single if...else if statement.
When using if , else if , else statements there are few points to keep in mind.

* An if can have zero or one else's and it must come after any else if's.

* An if can have zero to many else if's and they must come before the else.

* Once an else if succeeds, none of he remaining else if's or else's will be tested.

##### Syntax

    if(Boolean_expression 1){
       //Executes when the Boolean expression 1 is true
    }else if(Boolean_expression 2){
       //Executes when the Boolean expression 2 is true
    }else if(Boolean_expression 3){
       //Executes when the Boolean expression 3 is true
    }else {
       //Executes when the none of the above condition is true.
    }

##### Example 

      var x = 30;
      if( x == 10 ){
         write("Value of X is 10");
      }else if( x == 20 ){
         write("Value of X is 20");
      }else if( x == 30 ){
         write("Value of X is 30");
      }else{
         write("This is else statement");
      }

[Back to Contents](#contents)

## Loops

There may be a situation, when you need to execute a block of code several number of times. In general, statements are executed sequentially: The first statement in a function is executed first, followed by the second, and so on.

Programming languages provide various control structures that allow for more complicated execution paths.

A loop statement allows us to execute a statement or group of statements multiple times and following is the general from of a loop statement in most of the programming languages:

![Loop Structure](http://www.tutorialspoint.com/scala/images/loop_architecture.jpg)

nJexl provides three types of loops to handle looping requirements. 

#### GoTo

The syntax of _goto_ is very simple to understand :


    // initialize 
    x = 10 
    #start // start of the loop, labels are marked with #
    write(x) 
    x -= 1 // decrement 
    goto  #start x > 0 // condition 
    write('out of loop')

Note that _goto_ can be unconditional too. 

#### While 

The same loop can be represented with while loop,
it is very simple to understand. 

##### Syntax of While 
 
      while ( boolean-condition-true ){
           // execute this body 
      }

##### Example 

    // initialize 
    x = 10 
    while ( x > 0 ) { // start of the loop
        x -= 1 // decrement 
        write(x) 
    }
    write('out of loop')

#### For 

The same loop can be represented with for loops : 

##### Syntax of for with conditions   
 
      for ( init-statement ; condition ; post-statement ){
           // execute this body if condition is true
           // then execute post statement 
      }

##### Example 

    for ( x = 10 ; x > 0 ; x-= 1 ){ 
        write(x) 
    }
    write('out of loop')

##### The Range type 

In nJexl a range is a kind of lazy [iterator](https://en.wikipedia.org/wiki/Iterator).
This is defined as :

      (njexl)r = [10:0]
      =>[10:0:-1]

This shows that the iterator starts at 10, takes a step -1, and ends by reaching 0.
For loops in nJexl can take iterators, hence : 

##### Syntax of for with iterator  
 
      for ( item  : iterator ){
           // execute this body 
      }

##### Example 

    for ( x : [10:0] ){ 
        write(x) 
    }
    write('out of loop')

#### Continue

Sometimes it is needed to skip the rest of the loop body, when some condition 
gets satisfied. For example, take the [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz) problem.

A very unoptimal solution would be :

    for ( i : range ){
        if ( i % 15 ) { 
             write ( 'FizzBuzz' )
        } else if ( i % 5 )  { 
             write ( 'Buzz' )
        } else if ( i % 3 ){
            write ( 'Fizz' )
        } else {
            write (i)
        }
    }

Notice that too many else-if one needs to code.
A slightly more optimal solution would be :

    for ( i : range ){
        div_by_3 = i % 3 
        div_by_5 = i % 5 
        
        if ( !div_by_3 and !div_by_5 ) { 
             write ( i )  
             continue // do not execute code below
        } 
        if ( div_by_3 )  { 
             write ( 'Fizz' )
        } 
        if ( div_by_5 ) {
            write ( 'Buzz' )
        } 
    }

Now if one notices that continue, it is like the _if_ is added to the continue, 
so more succint way one can write it :

    for ( i : range ){
        div_by_3 = i % 3
        div_by_5 = i % 5 
        // skip code - when - after executing this ( optional )
        continue ( !div_by_3 and !div_by_5 ) { write ( i ) }       
        if ( div_by_3 ) write ( 'Fizz' )
        if ( div_by_5 ) write ( 'Buzz' )
    }

#### Break

It is sometimes needed to find some item with matching characteristics, 
for example, find the element in a list which is greater than a specified one :
    
    x = null 
    l = [ 1, 10, 98, 4, 23 ,21 , 0 ]
    for ( i : l ){
        if ( i > 50 ) { 
            x = i 
            break // breaks the loop
        } 
    }
    if ( x != null ){
        write ( "found item : %d \n" , x )
    }

This can also be written in succint way :
    
    x = null 
    l = [ 1, 10, 98, 4, 23 ,21 , 0 ]
    for ( i : l ){
        // break - when - after executing the block (optional ) 
        break ( i > 50 ) { x = i } 
    }
    if ( x != null ){
        write ( "found item : %d \n" , x )
    }

[Back to Contents](#contents)

## Functions 

A function is a group of statements that together perform a task. You can divide up your code into separate functions. How you divide up your code among different functions is up to you, but logically, the division usually is so that each function performs a specific task.

A function definition can appear anywhere in a source file and nJexl permits nested function definitions, that is, function definitions inside other function definitions.

### Function Definition

Function definition has the following form:

    def [ optinal-functionName ] ([optional-list-of-parameters]) {
       function body
    }

##### Example 

      def add_int( a, b ) {
          sum = int(a,0) + int(b,0)
          return sum
      }

One should note the casting into integer by the standard function _int_(x,opt-value).
As this is a dynamic language with no type check, it is quite possible that the params
a, b are not even integer. Thus, int() function, when failed to cast them back to integer,
casts them to 0. 

Now, to call the function :

     r = add_int(6,36)
     write(r) // prints 42

##### The \_\_args\_\_ construct 

nJexl methods can take arbitrary no. of parameters, hence, there has to be 
a way to get the arguments given at runtime. This is done by the \_\_args\_\_ construct.

      // no arguments mean nothing actually 
      def add_ints() {
          sum = 0
          // the arguments gets stored in the __args__ 
          for ( x : __args__ ){
               sum += int(x,0)
          }
          sum // implicit return 
      }
      r = add_ints(2, 4, 8, 12, 16)
      write(r) // prints 42

##### Mixing of named params with unnamed 

nJexl methods can be made to mixed with arbitrary no. of parameters :

      // This has two formal arguments, but can take more... 
      def add_ints(a,b) {
          sum = int(a,0) + int(b,0)
          // the arguments gets stored in the __args__ 
          for ( i = 2 ; i < size( __args__ ) ; i+= 1 ) {
               sum += int( __args__[i] ,0)
          }
          sum // implicit return 
      }
      r = add_ints(2, 4, 8, 12, 16)
      write(r) // prints 42


##### Default values for named params  

nJexl methods can be made to give default values for named parameters:

      // This has two formal arguments with default 
      def add_ints(a = 6 , b = 36 ) {
          sum = int(a,0) + int(b,0)
          // the arguments gets stored in the __args__ 
          for ( i = 2 ; i < size( __args__ ) ; i+= 1 ) {
               sum += int( __args__[i] ,0)
          }
          sum // implicit return 
      }
      r = add_ints()
      write(r) // prints 42
      write( add_ints(6) ) // prints 42

[Back to Contents](#contents)

### Functions as Variables 

Functions are objects and hence can be stored in a variable.
So, this become perfectly valid :


      def add_int( a, b ) {
          sum = int(a,0) + int(b,0)
          return sum
      }
      fp = add_int // store the function reference in fp variable
      write ( fp(10,20) ) // prints 30 

#### Defined as Variables

Functions can be defined as a variable, for example :

      fp = def ( a, b ) { // fp is assigned an anonymous function 
          sum = int(a,0) + int(b,0)
          return sum
      }
      write ( fp(10,20) ) // prints 30 

#### Inside other Data Structures 

Functions can be stored inside other data structures once created like variables :

      add = def ( a, b ) {  int(a,0) + int(b,0) }
      sub = def ( a, b ) {  int(a,0) - int(b,0) }
      operations = { '+' : add , '-' : sub }

This has practical usage, for example, suppose one wants to create a dynamic expression 
evaluator, where based on the operator, operation has to be performed :

       if ( op == '+') return add( a,b) 
       if ( op == '-') return sub( a,b) 
       write ( 'No matching operator!' )

Instead of this, one can do this now :

       f = operations[op] 
       if ( f != null ) return f(a,b) 
       write ( 'No matching operator!' )

#### As argument to another function 

Suppose we want to create a minimum function over a list.
To do so, we need to compare items in the list :

     def find_min(l){
         min = l[0]
         for ( i = 1 ; i < size(l) ; i += 1 ){
             if ( l[i] < min ) min = l[i]
         }
         return min 
    }

The problem is, we do not know what the items of the list are, 
and if they are comparable using "<" operator.

So, suppose that we create a function compare :

     l = [ { 'x' : 10 } ,  { 'x' : 100 } , { 'x' : 30 } , { 'x' : 1 } ]
     // elementary compare function 
     def compare(a,b){
         if ( a.x < b.x ) return true 
         return false 
     }

And then pass it to the find_min :

     def find_min(l){
         min = l[0]
         for ( i = 1 ; i < size(l) ; i += 1 ){
             if ( compare ( l[i] , min ) ) { min = l[i] }
         }
         return min 
    }

The problem here is that the function is hard coded inside find_min.
To avoid that:

    def find_min(l, cmp_func ){
         min = l[0]
         for ( i = 1 ; i < size(l) ; i += 1 ){
             if ( cmp_func ( l[i] , min ) ) { min = l[i] }
         }
         return min 
    }
    // call 
    find_min ( l, compare )

The problem with this is now that one always has to pass an implementation
of the comapre function. Can we pass a suitable default? Yes, we can :


    def find_min(l, cmp_func = def(a,b){ a < b } ){
         min = l[0]
         for ( i = 1 ; i < size(l) ; i += 1 ){
             if ( cmp_func ( l[i] , min ) ) { min = l[i] }
         }
         return min 
    }
    // call 
    min = find_min ( [ 20, 0 , 10]  ) // using default 
    write(min)

[Back to Contents](#contents)

## Closures

A closure is a function, whose return value depends on the value of one or more variables declared outside this function. Consider the following piece of code with anonymous function:

    multiplier = def(i) { i * 10 }

Here the only variable used in the function body, i * 0, is i, which is defined as a parameter to the function. Now let us take another piece of code:

    multiplier = def (i) {  i * factor } 

There are two free variables in multiplier: i and factor. One of them, i, is a formal parameter to the function. Hence, it is bound to a new value each time multiplier is called. However, factor is not a formal parameter, then what is this? Let us add one more line of code:

     factor = 3
     multiplier = def (i) {  i * factor } 

Now, factor has a reference to a variable outside the function but in the enclosing scope. Let us try the following example:

      write ( multiplier(1) ) // prints 3 
      write ( multiplier(14) ) // prints 42 

Above function references factor and reads its current value each time. If a function has no external references, then it is trivially closed over itself. No external context is required.

### Partial Function

The issue with the previous closure was that the global factor change would 
effect the yield of the function multiplier.
Is there a way to fix it so that the parameters stays *bound* to the function.
Yes, there is, it is called partial function :


    // a function returning another 
    def mult( a ){
       // this is the function , completion 
       def arg2( b ){
           a * b 
       }
       // return the partial function 
       return arg2
    }
    // closure of mult : partial
    m1 = mult(3)
    // complete closure 
    write( m1(14) ) // prints 42

A more elaborate discussion can be found [here](http://stackoverflow.com/questions/11590847/partial-application-and-closures).

[Back to Contents](#contents)

## Input Output

nJexl is open to make use of any Java objects and java.io.File is one of the objects which can be used in Scala programming to read and write files. However, it makes stuff a much easier by overloading 
many fuctionality in the same functions.
Let's start with read.

### Read 

Suppose we need to read from a file named 'test.txt'. To do so, first we check 'test.txt' :

    C:\>type test.txt
    Hello, World!

Now, we want to read this file, so :

    (njexl)lines = read('test.txt')
    =>Hello, World!

Suppose there are multiple lines :

    (njexl)lines = read('test.txt')
    =>Hello, World!
    Another Line!
    Third line!

Now, we can read even from the command line :

    /* This is how you read from console */
    x = read() // reads a line 
    write(x) // writes back 

Read can also read from an url, and generate a get request.

    (njexl)_url_ = 'http://jsonplaceholder.typicode.com/posts/1'
    =>http://jsonplaceholder.typicode.com/posts/1
    (njexl)read(_url_)
    =>{
      "userId": 1,
      "id": 1,
      "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
      "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
    }


### Write 

We are already familiar with write() call.
Generally whatever you want to write back, you put through write.

Write also takes format arguments, just like [String.format](http://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html).

Write writes back to the console, as well as named files :

     write('test.txt', "Hello, I am writing in here!")

will create a file 'test.txt' and write this line to it.

Write can also write to an url, that is generate a post request.

    (njexl)    _url_ = 'https://httpbin.org/post'
    =>https://httpbin.org/post
    (njexl)
    (njexl)params = { 'foo' : 'bar' , 'complexity' : 'sucks'  }
    =>{complexity=sucks, foo=bar}
    (njexl)write(_url_,params)
    =>{
      "args": {}, 
      "data": "", 
      "files": {}, 
      "form": {
        "complexity": "sucks", 
        "foo": "bar"
      }, 
      "headers": {
        "Accept": "text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2", 
        "Content-Length": "24", 
        "Content-Type": "application/x-www-form-urlencoded", 
        "Host": "httpbin.org", 
        "User-Agent": "Java/1.8.0_60"
      }, 
      "json": null, 
      "origin": "124.123.179.172", 
      "url": "https://httpbin.org/post"
    }


## Special Functions

There are many special functions those create specific data structures, for example 

 * list
 * array 
 * dict 

These creates collection types.
The functions :

 * empty 
 * size

are used to find if collections are empty and the size of the collections.
*null* is empty, as well as *size(null)* < 0.

### Collections

#### List and Array 

Creating a mutable list is easy :

    (njexl)l =  list()
    =>[]
    (njexl)l =  list(1,2,3)
    =>[1, 2, 3]

Accessing individual elements can be done :

    (njexl)l[0]
    =>1

Finding size of a list is trivial with special function named *size* :

    (njexl)size(l)
    =>3

And same is the case for an array :

    (njexl)a =  array(1,2,3)
    =>@[1, 2, 3]


#### Dictionary 

The function *dict* creates a dictionary :

    (njexl)d = dict()
    =>{} // empty dictionary 

It can be created from a two lists, one holding the keys, another the values :

    (njexl)d = dict([1,2], [3,4])
    =>{1=3, 2=4}


### Avoiding Iteration

Suppose one wants to generate a list from an exisiting list, for example, square all numbers 
between 1 to n ?
Generally one would write an iteration like :

    n = 10
    l = list()
    for ( x  : [1:n + 1] ){
        l += x**2 // square and then add, the '+' is overloaded
    }
    write(l)

The same code can be achived by the list() function :

    (njexl)list{ $**2 }([1:11])
    =>[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

Here we notice that the code you would have written inside the for-body, you write 
in the curly braces after the function _list_ . Also, we note that there is this implicit variable "$" that holds the current element of the list.
This anonymous function block is called Anonymous Parameter to the function, any function can have it, but not all functions process it.

#### Searching for Something 

One needs to search for some specific element inside a collection.

##### Index 

Suppose I want to find the first element of the list which is greater then 10:

    (njexl)y = list{ $**2 }([1:11])
    =>[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    (njexl)index{ $ > 10 }(y) // returns the index
    =>3
    (njexl)y[3] // get the item ?
    =>16

This is the job of the _index_ function. Here one specifies the condition which should become true, 
to say when the searching ended. The *rindex* function finds the index in reverse, so :

    (njexl)y
    =>[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    (njexl)rindex{ $ < 15 }(y)
    =>2
    (njexl)y[2]
    =>9

###### Where

What about we want to get both the index as well as the item? 
We use what is known as the [closure property]() of the anonymous function with respect to the external script :

    (njexl)y = list{ $**2 }([1:11])
    =>[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    (njexl)d = { : }
    =>{}
    (njexl)index{ where( $ > 10 ){ d[_] = $ } }(y) // observe the *where* clause 
    =>3
    (njexl)d
    =>{3=16}

Note the *where* clause. where returns the value of the ( expression ), while executing any code inside the block.

##### Select 

However, sometimes we need to search the whole collection, and select elements where the elements match some criterion : 

    (njexl)select{ $ > 10 }(y)
    =>[16, 25, 36, 49, 64, 81, 100]

Now, suppose I also want to return the indices, as well as the elements :

    (njexl)s = {:}
    =>{}
    (njexl)select{ where($ > 10){ s[_] = $ } }(y)
    =>[16, 25, 36, 49, 64, 81, 100]
    (njexl)s
    =>{3=16, 4=25, 5=36, 6=49, 7=64, 8=81, 9=100}

Now, the variable ( hash ) "s" has the selection items, with index -> value form.

##### Partition 

Well, sometime we do not only want to select items, but also reject some, basically we conditionally 
parition the items : 

    (njexl)partition{ $ > 10 }(y)
    =>@[[16, 25, 36, 49, 64, 81, 100], [1, 4, 9]]

So, the items which got selected are returne as first item of an array, 
while the items which got rejected are the seccond item of the array.

##### Folds

How should I add a list of integers?  That brings the issue of [folding](https://en.wikipedia.org/wiki/Fold_(higher-order_function)) here.

Generally, the sum function can be written as :

    sum = 0 
    for ( x : range ){
        sum += x 
    }
    sum 

The corresponding recursive code can be :

     def sum( iterator , cur_sum ){
        if ( ! iterator.hasNext() ) return cur_sum
        cur_sum += iterator.next()
        return sum( iterator, cur_sum )
     }
     // call it 
     r = sum(l,0)

But, recursion is bad, and thus the same code can be done with fold functions ( either lfold or rfold ) :

    (njexl)y
    =>[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    (njexl)lfold{  _$_ + $ }(y,0)
    =>385

Note that the initial seed is passed as 0, the anonymous parameter passed tells to add the 
current element with partial result generated thereby , which is accessed by  " \_$\_ " .

*lfold* folds a structure from *left* (traversing from left to right) while *rfold* 
travarses from right to left.

Observe this:

    (njexl)lfold(y)
    =>1∅4∅9∅16∅25∅36∅49∅64∅81∅100
    (njexl)rfold(y)
    =>100∅81∅64∅49∅36∅25∅16∅9∅4∅1

With nothing, it produces a *unique* string representation of the list from left to right
and right to left.

## Sample Programs 

>Theory and practice are the same in theory, but in practice, they differ.

Thus, we need to apply what we learned in practice.
Hence, we showcase some *interview twisters* that people ( mostly wrongly ) asks
in the interviews ( trust me, they are not useful to judge ones ability).

### Speed of Execution 
Here we show-case that for a declarative language ( rather an interpreted language )
if-elses are costly than that of lower level stuff.

May we introduce a new friend of us the _clock_ function.

    (njexl)clock()
    =>@[0, null]

This function *clocks* the time taken ( in nano secs ) to execute the anonymous
block passed into this. As one can see, an empty block results in almost 0 nanosec 
processor time.

Now let's run some more :

    (njexl)clock{ for(i : [0:9999999]){ x = i } }()
    =>@[631216236, 9999998]

Ok, that is, it actually works. Now, we have this issue of converting nano to millisec, 
so : 

    (njexl)_o_[0]/1000000.0 ## Note, the _o_ stores the last result in REPL
    =>631.216236 ## pretty fast? 

Compare this with Python:

```python
ts = time.time()*1000.0
for i in xrange(0,9999999):
    x = i
te = time.time()*1000.0
print(int(te - ts))
```

And the result comes :

     840 // python is *SLOWER*, enjyoy.

[Back to Contents](#contents)

### Fizz Buzz 
We encountered this before, and solved it, using if elses.
Now, let's put that code to test, for speed:

    /* The iteration and conditional model */
    def fb1(range){
        for ( i : range ){
            div_by_3 = i % 3
            div_by_5 = i % 5 
            // skip code - when - after executing this ( optional )
            continue ( !div_by_3 and !div_by_5 ) { /* write ( i ) */ }       
            if ( div_by_3 ) { /* write ( 'Fizz' ) */  }
            if ( div_by_5 ) { /* write ( 'Buzz' ) */  }
        }
    }
    // test that 
    #(o,e) = clock{ fb1([0:100]) }()
    write(o/1000000.0)
    /* Pure If/Else */
    def fbI(range){
      for ( i : range ){
          if ( i % 3 == 0 ){
              if ( i % 5 == 0 ){ 
                /*  write('FizzBuzz') */ 
              } else {
                  /* write('Fizz') */
              }
          }else{
              if ( i % 5 == 0 ){
                /* write('Buzz') */
                }else{
                  /*  write(i) */
                }
          }
      }
    }
    // is  faster?
    #(o,e) = clock{ fbI([0:100]) }()
    write(o/1000000.0)
    /* The hash based model */
    def fb2(range){
      d = { 0 : 'FizzBuzz' , 
            3 : 'Fizz' , 
            5 : 'Buzz' ,  
            6 : 'Fizz', 
           10 : 'Buzz' , 
           12 : 'Fizz' }
      for ( x : range ){
           r = x % 15  
           continue ( r @ d ){ /* write ( d[r]) */ }
           /* write(x) */ 
      }
    }
    // is indeed fastest?
    #(o,e) = clock{ fb2([0:100]) }()
    write(o/1000000.0)

You would be astonished by the result :

    9.465945 // conditional 1
    7.408961 // conditional If-else
    4.067821 // hash access 

Thus, we have established that conditionals are bad in almost all cases in a declarative
paradigm, pick the hash one instead. Same is true for Python.

[Back to Contents](#contents)

### Scramble

I am sure you guys have know the game of jumbled up words.
For example, someone gives you "Bonrw" and you need to say : "Brown" !
Suppose I tell you to write a program to do it. 
It can be done in the hard way or the easy way.

#### Hard Way, Permutation 
The hard way is to permutate the word – and then get a match that exists
in the dictionary. 

That brings the question, how can I write a permutation program, 
such that given a word, it generates all possible permuation 
of the letters of the word?

First we need to create a variable holding the string :

    (njexl)word = "Bonrw"
    =>Bonrw

we are supposed to permutate 
the indices... so :

     (njexl)t = [0:size(word)].list
     =>[0, 1, 2, 3, 4]

Now the permuation problem formally is, well see it :

     (njexl)p = join{ #|set($)| == #|word|  }(__args__= list{ t }([0: #|word|]) )

With this, p holds the permuation of indices.
Now, we generate the permuation of the words from this *p* :

     perms = list{  str( list{ word[$] }($) ,'' ) }(p)

And I am done!
Now, I need to figure out if any of these *perms* are in an English dictionary, 
which is easy :

     select{ $ @ some_eng_dict }(perm)

And that solves it, the really hard way!

#### Easy Way, Light Bulb
But there is a better way, what if we sort the all the words in a dictionary  letter by letter and then use that sorted word as a key? Then we can easily solve the  problem by 
sorting on the letters of the word given and then checking if that as key exist
in the dictionary, then, find all possible matches !

That would be :

    (njexl)key = str( sorta( word.toLowerCase().toCharArray ) , '')
    =>bnorw
    (njexl)sorted_eng_dict[key] // and you have the list !

[Back to Contents](#contents)

### Next Higher Permutation

The problem is as follows :

>given an integer, find the next integer which has the same digits,
but is greater than the input integer.

The hard way is easy to do here:

* Find all permuations of the integer string
* casting them back to integer 
* Sort these integers
* Find the one immediately larger than the input integer

#### High Complexity Implementation

     // suppose "p" has the permuations, so step 2
     perms = list{  int ( str( list{ word[$] }($) ,'' ) ) }(p)
     // step 3
     perms = sorta(perms)
     // step 4
     i = index( x < $ )(perms)
     perms[i] // is the answer

#### Using Permutation Index

To do a slightly better  solution, one needs to understand 
the notion of the permutation index.
Suppose the length of the integer as string is *n*.
Suppose we sort the digits of the number and then 
order them below :

      0    1    2    3 ...   n-2     n-1 
    d[0] d[1] d[2] d[3]    d[n-2]  d[n-1] 

Such that d[i] <= d[i+1] holds. 
This would be the starting number to generate the permutation index.
Now, this indices 012...(n-2)(n-1) is representable in the 
base *n* form.

Next permutation of this indices becomes next integer in base *n* such 
that all digits are distinct! 
This indices in base *b* form are known as permutation index.

Given I have the permutation indices of a number, 
we know that next base *n* number higher than that of this index
represented in base *n* may be the higher permutation ( may not be also, 
because digits can be repeating!)

Thus, easiest way to achive this is to :
 
 * Generate the permutation index of the number  
 * switch to base *b*, 
 * generate the next higher permutation index ( just addition of 1 in base *b* ) 
 * check if this number has all digits distinct & has same no. of digits as input
 * check if this number is indeed higher than the input

[Back to Contents](#contents)

Thus, the code looks like this :

    /* Finds the current permutation index */
    def perm_index(i){
       x = str(i)
       d = dict()
       i = 0 
       x = x.toCharArray()
       y = sorta(x)
       for ( c : y ){
          if ( !(c @ d) ){
            d[c] = list()
          }
          d[c].add(i)
          i += 1
       }
       // this is the index
       s = lfold{ _$_ + d[$].remove(0)  }(x,'')
       return [ s , y]
    }
    /* Next Higher permutation */
    def next_higher_perm(i){
      // get the permutation index 
      #(pi, y ) = perm_index(i)
      n = #|pi| // the base 
      si = str(i) // the string representation 
      h = INT(pi,n)  // higher perm index 
      while ( true ){
         h += 1 // higher it 
         s = str(h,n) 
         if ( #|s| > n ){ return 'No Higher Permutation Possible!' }
         if ( #|s| < n ){ s = '0' + s } // when a perm starts with 0
         if ( #|set( s.toCharArray() )| == n ){
            // Possible to get something here : back to normal int 
            hp = lfold{ _$_ + y[int($)] }( s.toCharArray() ,'')
            hp = int(hp)
            if ( hp > i ) { return hp } // this is the result
         }
      } 
    }
    is = int ( __args__[1] )
    write(is)
    write( next_higher_perm(is) )

[Back to Contents](#contents)

#### A more Optimal Solution 

We note that the problem can be reformulated as:

> For a number of length *n* find i-k , i such that :
  * d[i-k] < d[i] 
  * i is minimum
  * k is minimum 

Hence, this code is slightly optimal :

    /* Next Higher permutation */
    def next_higher_perm(x){
      si = str(x).toCharArray()
      k = #|si| - 1
      while ( k >= 1 ){
        for ( i = k - 1 ; i >=0 ; i-= 1 ){
           if ( si[k] > si[i] ){  
              t = si[k] ; si[k] = si[i] ; si[i] = t ;
              return str(si,'') 
           }
        }
        k -= 1
     }
     return 'None Found!'
    }
    is = int ( __args__[1] )
    write(is)
    write( next_higher_perm(is) )

[Back to Contents](#contents)

## Random Computing

Sometimes it is of importance to get data from random range.
We will showcase some examples for this.

The handy function is :

    (njexl)r = random()
    =>java.security.SecureRandom@6438a396

### Numbers within a Range

Suppose I need to generate an integer between 100000:500000.
There are many ways to do it. 

#### Base Offset Solution 

Observe that :

     500000 = 400000 + 100000

So, we can split the distribution as :

    (njexl) x = 100000 + r.nextInt(400000)

The same can be done, easily with :
    
    (njexl) x =  random(100000 , 500000)

But then, this begs the question will this *nextInt()* distribute the 
numbers *uniformly* enough? 

#### Random Digits Solution 

Another way to look at it would be generate the digits at random.
We notice that the first digit would be between [1:5] , and 
rest 5 digits (at iteration of range [0:5] ) can take values between [0:10].   
Thus, we can generate individual digits easily :

    (njexl)x = lfold{ _$_ * 10  + random([0:10]) }([0:5], random([1:5]) )

In this case the individual digits are generated at random.
