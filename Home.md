# Welcome to the nJexl wiki!

nJexl is continuation of [Apache Jexl](http://commons.apache.org/proper/commons-jexl ) project.

## A brief History

All I wanted is a language where I can write my test automation freely - i.e. using theories from testing.
The standard book, and there is only one for formal software testing is 
that of [Beizer](http://www.amazon.com/Software-Testing-Techniques-2nd-Edition/dp/1850328803).

There was no language available which lets me intermingle with Java POJOs and let me write my test automation (validation and verifications). 
Worse still - one can not write test automation freely using Java. 
As almost all of modern enterprise application are written using Java, it is impossible to avoid Java 
and write test automation : in many cases you would need to call appropriate Java methods to automate APIs.

Thus, one really needs a JVM scripting language that can freely call and act on POJOs.
The idea of extending JEXL thus came into my mind : a language that has all the good stuffs from
the vast Java libraries, but clearly not verbose enough.

After cloning JEXL - and modifying it real heavy - a public release in a public repository
seemed a better approach. There, multiple people can look into it, rather than one lone ranger working from his den.
And hence nJexl was born. The *n* stands for Neo, not Noga, which is, by the way, my nick name.

A [very fast introduction](https://github.com/nmondal/njexl/wiki/0-An-Easy-Tutorial) can be used to start using it. 

### Note on Java
This actually was an open challenge [here](http://steve-yegge.blogspot.in/2006/03/execution-in-kingdom-of-nouns.html)

>A noted PhD from Sun, read this essay, and had this to say: 
"hmm, chuckle :) This guy has too much time on his hands ! he should be doing useful work, or inventing a new language to solve the problems. Its easy to throw stones - harder to actually roll up your sleeves and fix an issue or two, or write/create a whole new language, and then he should be prepared to take the same criticism from his peers the way he's dishing it out for others. Shame - I thought developers were constructive guys and girls looking to make the lives of future software guys and girls easier and more productive, not self enamouring pseudo-intellectual debaters, as an old manager of mine used to say in banking IT - 'do some work' !"

And I just did. That too, while at home, in vacation time, and in night time ( from 8 PM to 3 AM. - see the check-ins.)
Alone.

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

       java -jar njexl.lang-0.3-SNAPSHOT.jar 


And you would be in the nJexl command prompt.
     
     (njexl)"Hello, World!"
     =>Hello, World!
     (njexl)
 
You are all set!
Enjoy the new language. 
And let me know how it feels!


# How to include it in your project

In the dependency section (latest release is 0.2 ) : 

```xml
<dependency>
  <groupId>com.github.nmondal</groupId>
  <artifactId>njexl.lang</artifactId>
  <version>0.2</version> <!-- or 0.3-SNAPSHOT -->
</dependency>
```


That should immediately make your project a nJexl supported one. 

## Setting it up for use
You can download the latest released ( snapshot ) one-jar from here : 
[SNAPSHOTS](https://oss.sonatype.org/content/repositories/snapshots/com/github/nmondal/njexl.lang/0.3-SNAPSHOT/)

It would look like : njexl.lang-<version>-onejar.jar.
Once downloaded, put this back in your PATH, in *nix like environment : 


    alias njexl='java -jar /<location>/njexl.lang-<version>.one-jar.jar'
 

And you are pretty much ready to go. Now type "njexl" from anywhere in the command prompt - and you are ready inside the njexl prompt.

### The IDE debacle
IDEs are good - and that is why we have minimal editor support, [Sublime Text](http://www.sublimetext.com) is my favourite one. You also have access to the syntax highlight file for jexl and a specially made theme for jexl editing - ( ES ) both of them can be found :   [here](https://github.com/nmondal/njexl/tree/master/doc).


If you use them with your sublime text editor - then typical jexl script file looks like this : 

![Sample Editing in Sublime Text ](http://s12.postimg.org/5dmcutp59/Screen_Shot_2015_05_02_at_6_23_45_pm.png)

Happy Coding!

### Final Words from Ryan Dahl

My experience in Industry is aptly summarized by [Ryan Dahl](https://en.wikipedia.org/wiki/Node.js) 
in [here](http://harmful.cat-v.org/software/node.js) , the creator of Node.js :

>I hate almost all software. It’s unnecessary and complicated at almost every layer. At best I can congratulate someone for quickly and simply solving a problem on top of the shit that they are given. The only software that I like is one that I can easily understand and solves my problems. The amount of complexity I'm willing to tolerate is proportional to the size of the problem being solved...(continued)...
Those of you who still find it enjoyable to learn the details of, say, a programming language - being able to happily recite off if NaN equals or does not equal null - you just don’t yet understand how utterly fucked the whole thing is. If you think it would be cute to align all of the equals signs in your code, if you spend time configuring your window manager or editor, if put unicode check marks in your test runner, if you add unnecessary hierarchies in your code directories, if you are doing anything beyond just solving the problem - you don’t understand how fucked the whole thing is. No one gives a fuck about the glib object model.
*The only thing that matters in software is the experience of the user.*
– Ryan Dahl, creator of Node.js

Amen to that. 
Hope, the people eventually understands it.
It will make the world a better place.

## Code Coverage 

PS. We should preach what we practice.
Hence, here is the code coverage result of the njexl, showing 80% coverage ( almost )
excluding the parser.

![Code Coverage](http://s30.postimg.org/c2fzwv5pd/Screen_Shot_2015_12_04_at_11_49_55_pm.png)