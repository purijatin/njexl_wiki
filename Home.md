# Welcome to the nJexl wiki!

nJexl is continuation of [Apache Jexl](http://commons.apache.org/proper/commons-jexl ) project.

## A brief History

All I wanted is a language where I can write my test automation freely - i.e. using theories from testing.
The standard book, and there is only one for formal software testing is 
that of [Beizer](http://www.amazon.com/Software-Testing-Techniques-2nd-Edition/dp/1850328803).

There was no language available which lets me intermingle with Java POJOs and let me write my test automation. 
Worse still - one can not write test automation freely using Java. 
As almost all of modern enterprise application are written using Java, it is impossible to avoid Java 
and write test automation : in many cases you would need to call appropriate Java methods to automate APIs.

Thus, one really needs a JVM scripting language that can freely call and act on POJOs.
The idea of extending JEXL thus came into my mind : a language that has all the good stuffs from
the vast Java libraries, but clearly not verbose enough.

After cloning JEXL - and modifying it real heavy - a public release in a public repository
seemed a better approach. There, multiple people can look into it, rather than one lone ranger working from his den.
And hence nJexl was born. The *n* stands for Neo, not Noga, which is, by the way, my nick name.

## About the Language 

It is an interpreted language. It is asymptotically as fast as python, with a general lag of 200 ms of reading and parsing files, where native python is faster. After that the speed is the same.
 
It is a multi-paradigm language. It supports functionals ( i.e. anonymous functions ) out of the box, and every function by design can take functional as input. There are tons of in built methods which uses functional.

It supports OOP. Albeit not recommended, as [OOPs!](http://harmful.cat-v.org/software/OO_programming/why_oo_sucks)
clearly shows why. In case you want C++ i.e.  [multiple inheritance](http://en.wikipedia.org/wiki/Multiple_inheritance) with full operator overloading, friend functions, etc. then this is for you anyways. 
Probably you would love it to the core.

Python is a brilliant language, and I shamelessly copied many, and many adages of Python here. The heavy use of 

      __xxx__    

literals, and the *me* directive, and *def* is out and out python. 

The space and tab debate is very religious, and hence JEXL is "{ blocked }" : Brace yourself.
Pick tab/space to indent - none bothers here.
You can use ";" to separate statements in a line. 
Lines are statements.


## If you like what you see 

There are many samples in the  [samples](https://github.com/nmondal/njexl/tree/master/core/samples)  folder.
If you want to use it - you have to download it using git. 
You need to have java 1.8 installed as well as maven.

Once they are installed, and java 1.8 is in maven's path e.g. JDK_HOME is set properly, 
the source can be compiled. Some tests fails, so it is ok.

      mvn clean install -DskipTests 

Should do the job fine.
Now, if you want to experience the nJexl, go to the target folder, and type in : 


       java -jar njexl.lang-0.1-SNAPSHOT.jar 


And you would be in the nJexl command prompt.
     
     (njexl)"Hello, World!"
     =>Hello, World!
     (njexl)
 
You are all set!
Enjoy the new language. 
And let me know how it feels!


# How to include it in your project

In the dependency section : 

      <dependency>
          <groupId>com.github.nmondal</groupId>
          <artifactId>njexl.lang</artifactId>
          <version>0.1</version>
       </dependency>

That should immediately make your project a nJexl supported one. 

## Setting it up for use
You can download the latest released ( snapshot ) one-jar from here : 
[SNAPSHOTS](https://oss.sonatype.org/content/repositories/snapshots/com/github/nmondal/njexl.lang/0.2-SNAPSHOT/)

It would look like : njexl.lang-0.2-<time-stamp>-onejar.jar.
Once downloaded, put this back in your PATH, in *nix like environment : 


    alias njexl='java -jar /<location>/njexl.lang-0.2-<time-stamp>.one-jar.jar'
 

And you are pretty much ready to go. Now type "njexl" from anywhere in the command prompt - and you are ready inside the njexl prompt.

### The IDE debacle
IDEs are good - and that is why we have minimal editor support, [Sublime Text ](http://www.sublimetext.com) is my favourite one. You also have access to the syntax highlight file for jexl and a specially made theme for jexl editing - ( ES ) both of them can be found :   [here](https://github.com/nmondal/njexl/tree/master/doc).


If you use them with your sublime text editor - then typical jexl script file looks like this : 

![Sample Editing in Sublime Text ](http://s12.postimg.org/5dmcutp59/Screen_Shot_2015_05_02_at_6_23_45_pm.png)

Happy Coding!