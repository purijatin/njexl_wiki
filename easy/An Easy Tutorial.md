# Welcome to the nJexl Easy Tutorial

## Contents 

* [Overview](#overview) 
    *[About nJexl](#about-njexl)
    *[nJexl Vs. Java](#njexl-vs-java)
* [Environment Setup](#environment-setup)
* [Basic Syntax](#basic-syntax) 
    * [First Program](#first-program)
* [Data Types](#data-types)
* [Variables](#variables) 
* [Operators](#operators)  
* [If/Elses](#if-and-else) 
* [Loops](#loops) 
    * [Goto](#goto)
    * [While](#while)
    * [For](#for)
    * [Continue](#continue)
    * [Break](#break)

* [Functions](#functions) 
* Closures 
* Classes & Objects 
* I/O 

## Overview 

nJexl, short for new Java Expression Language, is a hybrid functional programming language. 
It was created with business programming ( business logic ) and test automation validaition 
in mind.

nJexl smoothly integrates features of object-oriented and functional languages and is interpreted to run on the Java Virtual Machine. 

### About nJexl

Here is the important list of features, which make nJexl a first choice of the business developers.

#### nJexl is Embeddable in Java Code

nJexl scripts are easy to be invoked as stand alone scripts, also from withing java code, 
thus making integration of external logic into proper code base easy.

#### nJexl is functional:
nJexl is also a functional language in the sense that every function is a value and because every value is an object so ultimately every function is an object.

nJexl provides a lightweight syntax for defining anonymous functions, it supports higher-order functions, it allows functions to be nested, and supports [currying](https://en.wikipedia.org/wiki/Currying). 

#### nJexl is dynamically typed:
nJexl, unlike other statically typed languages, does not expect you to provide type information. You don't have to specify a type in most cases, and you certainly don't have to repeat it.

#### nJexl gets interpreted on the JVM:
nJexl is interpreted by a Java runtime. This means that nJexl and Java have a common runtime platform. You can easily move from Java to nJexl and vice versa.

#### nJexl can Execute any exisiting Java Code:
nJexl enables you to use all the classes of the Java SDK's in nJexl, and also your own, custom Java classes, or your favourite Java open source projects.

### nJexl vs Java

nJexl has a set of features, which differ from Java. Some of these are:

* All types are objects.

* Type inference.

* Nested Functions.

* Functions are objects.

* Closures.

[Back to Contents](#contents)

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

It should produce the prompt (njexl).

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

### First Program:

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

Save this as a file "hello.jxl" and run :

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

     + ++ ::: <?> :>

#### Keywords:

Some are reserved words, and can not be used as identifiers.
Some are semi reserved, and can be used as identifiers.

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

     l  = [ 1 , 2, 3, 4 , 'foo' , x ]

##### Dict Literal 

A Dict literal is defined as :

     h  = { 1 : 2, 3 : 4 , 'foo' :  x }

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


    +   Adds two operands   A + B will give 30
    -   Subtracts second operand from the first A - B will give -10
    *   Multiplies both operands    A * B will give 200
    /   Divides numerator by de-numerator   B / A will give 2
    %   Modulus Operator and remainder of after an integer division B % A will give 0

#### Relational Operators

    == or eq Checks if the values of two operands are equal or not, if yes then condition becomes true.  (A == B) is not true.
    !=  or ne  Checks if the values of two operands are equal or not, if values are not equal then condition becomes true. (A != B) is true.
    >  or gt Checks if the value of left operand is greater than the value of right operand, if yes then condition becomes true. (A > B) is not true.
    <  or lt Checks if the value of left operand is less than the value of right operand, if yes then condition becomes true.    (A < B) is true.
    >=  or ge Checks if the value of left operand is greater than or equal to the value of right operand, if yes then condition becomes true. (A >= B) is not true.
    <=  or le Checks if the value of left operand is less than or equal to the value of right operand, if yes then condition becomes true.    (A <= B) is true.

#### Logical Operators

There are following logical operators supported:
Assume variable A holds _true_ and variable B holds _false_ , then:


    &&  or and  Called Logical AND operator. If both the operands are non zero then condition becomes true. (A && B) is false.
    ||  or or Called Logical OR Operator. If any of the two operands is non zero then condition becomes true. (A || B) is true.
    ! or not  Called Logical NOT Operator. Use to reverses the logical state of its operand. If a condition is true then Logical NOT operator will make false.    !(A && B) is true.


#### Assignment Operators

     =     C = A + B will assign value of A + B into C.
     +=    C += A is equivalent to C = C + A
     -=    C -= A is equivalent to C = C - A

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

#### Go To Statement 

The syntax of _goto_ is very simple to understand :


    // initialize 
    x = 10 
    #start // start of the loop, labels are marked with #
    x -= 1 // decrement 
    write(x) 
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

### Storing Functions in Variables 




