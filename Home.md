# Welcome to the nJexl wiki!

nJexl is continuation of [Apache Jexl](http://commons.apache.org/proper/commons-jexl ) project.

## A brief History

All I wanted is a language where I can write my test automation freely - i.e. using theories from testing.
The standard book, and there is only one for formal software testing is :  [Beizer](http://www.amazon.com/Software-Testing-Techniques-2nd-Edition/dp/1850328803)

There was no language available which lets me intermingle with Java POJOs and let me write my test automation. Worse still - one can not write test automation freely using Java - and almost all of modern enterprise apps are written using Java.

Thus, one really needs a channel of working out a JVM scripting language that can freely call and act on POJOs.
Thus, the idea of extending jexl come into my mind.

After cloning jexl - and modifying it real heavy - it looked like a better option would be a public release in a public repository where multiple people can look into it, rather than one working from his den.
And hence nJexl was born. The *n* stands for Neo, not Noga, which is, by the way, my nick name.

## About the Language 

It is an interpreted language. It is asymptotically as fast as python, with a general lag of 200 ms of reading and parsing files, where native python is faster. After that the speed is the same.
 
It is a multi-paradigm language. It supports functionals ( i.e. anonymous functions ) out of the box, and every function by design can take functional as input. There are tons of in built methods which uses functional.

It supports OOP. Albeit not recommended, as [OOPs!](http://harmful.cat-v.org/software/OO_programming/why_oo_sucks)
clearly shows why. In case you want C++ i.e.  [multiple inheritance](http://en.wikipedia.org/wiki/Multiple_inheritance) with full operator overloading, friend functions, etc. then this is for you anyways. Then, probably you would love it to the core.

Python is a brilliant language, and I shamelessly copied many, and many adage of Python here. The heavy use of "__xxx__" literals, and the *me* directive, and *def* is out and out python. 

The space and tab debate is very religious, and hence jexl is "{ }" : Brace yourself.
Pick tab/space to indent - none bothers here.
You can use ";" to separate statements in a line. Lines are statements.


## If you like what you see 

There are many samples in the samples folder.
As of the moment it is going through heavy churn in design and code.
If you want to use it - you have to download it using git. 
You need to have java 1.8 installed as well as maven.

Once they are installed, and java 1.8 is in maven's path e.g. JDK_HOME is set properly, 
the source can be compiled. Some tests fails, so it is ok.

      mvn clean install -DskipTests 

Should do the job fine.
Now, if you want to experience the nJexl, go to the target folder, and type in : 

     java -jar njexl-0.1-SNAPSHOT.jar

And you would be in the nJexl command prompt.
     
     (njexl)"Hello, World!"
     =>Hello, World!
     (njexl)
 
You are all set!
Enjoy the new language. 
And let me know how it feels!



 