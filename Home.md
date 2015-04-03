# Welcome to the nJexl wiki!

nJexl is continuation of Apache Jexl project ( http://commons.apache.org/proper/commons-jexl ).
All I wanted is a language where I can write my test automation freely - i.e. using theories from testing.
There was none available. Worse still - one can not write test automation freely using Java - and as almost all of modern enterprise apps are written using Java.

Thus, one needs a channel of working out a JVM scripting language that can freely call and act on POJOs.
Thus, the idea of extending jexl.

After cloning jexl - and modifying it heavily - it looked like a better option would be a public release.
And hence nJexl was born. The *n* stands for Neo.

It is an interpreted language. And it is fast, at least 2 times faster than python.
 
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



 
