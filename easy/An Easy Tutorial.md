# Welcome to the nJexl Easy Tutorial

## Contents 

* [Overview](#overview) 
* [Environment Setup](#environment-setup)
* [Basic Syntax](#basic-syntax) 
* [Data Types](#data-types)
* [Variables](#variables) 
* [Operators](#operators)  
* If/Elses 
* Loops 
* Functions 
* Closures 
* Classes & Objects 
* I/O 

## Overview 

nJexl, short for new Java Expression Language, is a hybrid functional programming language. 
It was created with business programming ( business logic ) and test automation validaition 
in mind.

nJexl smoothly integrates features of object-oriented and functional languages and is interpreted to run on the Java Virtual Machine. 

Here is the important list of features, which make nJexl a first choice of the business developers.

### nJexl is Embeddable in Java Code

nJexl scripts are easy to be invoked as stand alone scripts, also from withing java code, 
thus making integration of external logic into proper code base easy.

### nJexl is functional:
nJexl is also a functional language in the sense that every function is a value and because every value is an object so ultimately every function is an object.

nJexl provides a lightweight syntax for defining anonymous functions, it supports higher-order functions, it allows functions to be nested, and supports [currying](https://en.wikipedia.org/wiki/Currying). 

### nJexl is dynamically typed:
nJexl, unlike other statically typed languages, does not expect you to provide type information. You don't have to specify a type in most cases, and you certainly don't have to repeat it.

### nJexl gets interpreted on the JVM:
nJexl is interpreted by a Java runtime. This means that nJexl and Java have a common runtime platform. You can easily move from Java to nJexl and vice versa.

### nJexl can Execute any exisiting Java Code:
nJexl enables you to use all the classes of the Java SDK's in nJexl, and also your own, custom Java classes, or your favourite Java open source projects.

### nJexl vs Java:

nJexl has a set of features, which differ from Java. Some of these are:

* All types are objects.

* Type inference.

* Nested Functions.

* Functions are objects.

* Closures.


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

### Basic Syntax

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
