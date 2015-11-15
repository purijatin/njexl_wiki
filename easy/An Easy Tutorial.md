# Welcome to the nJexl Easy Tutorial

## Contents 

* [Overview](#Overview) 
* [Environment Setup](#Environment%20Setup)
* Basic Syntax 
* Data Types 
* Variables 
* Operators 
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
