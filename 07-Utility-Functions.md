# Utility Functions
In this section we would list out some standard functions 
which are available.
In fact you have already met with some of them:

* byte
* char
* short
* int
* long
* INT -> converts to big integer
* float
* double 
* DEC -> converts to big decimal
* list
* dict
* array --> creates an array from arguments 
* index --> finds item in an indexable collection return the index 
* select
* partition -> simultaneously partition elements into match and no match 
* shuffle -> shuffles a list/array 
* random -> selects element[s] from a list/array/enums/string  randomly
* sorta --> sort a list in ascending order 
* sortd --> sort a list in descending order 
* read --> read from standard input or a location completely, returns a string 
* write --> to a file or standard output 
* json --> read json file or a string and return a hash
* xml --> read xml from file or string and returns an XmlMap data structure 
* type --> type of a container 
* minmax --> finds min, max of a list in a single pass, for those who are non scalar
* sqlmath --> finds min, Max, sum of a list in a single pass, for scalars
* try --> guards a native Java function call to ensure it does not throw exceptions
* load --> load arbitrary jars from a directory location, recursively
* system --> make arbitrary system calls
* thread --> makes and starts a thread, with parameters 
* clock --> This is to tune method calls, how much time was taken for a piece of code 
* until --> A polling based waiter, waits till a duration till a condition is true 

Thus, we would be familiarizing you guys with some of them.

## Partition 
The function partition() is a fairly interesting one, which given a criterion splits a collection into two parts,
one that matches the criterion, and another that does not match.
   
    (njexl)marks = [ 34, 45, 23 , 66, 89, 91 , 25, 21, 40  ]
    =>@[34, 45, 23, 66, 89, 91, 25, 21, 40]
    (njexl)p = partition{ $ > 30 }(marks)
    =>@[[34, 45, 66, 89, 91, 40], [23, 25, 21]]

What we are doing here is partitioning the marks using the criterion, if anything is over 30, it is pass else fail.

## Shuffling 
In general, testing requires shuffling of data values. Thus, the function comes handy:

    (njexl)cards = [ 'A', 'B' , 'C' , 'D' ] 
    =>@[A, B, C, D]
    (njexl)shuffle(cards)
    =>true ## returns true/false stating if shuffled
    (njexl)cards
    =>@[D, A, C, B]


## Sorting 
Sorting is trivial: 

    (njexl)sorta(cards)
    =>[A, B, C, D]
    (njexl)cards
    =>@[D, A, C, B]
    (njexl)sortd(cards)
    =>[D, C, B, A]

Now, sorting is anonymous block ( function ) ready, hence we can sort on specific attributes.
Suppose we want to sort a list of complex objects, like a Student with Name and Ids: 

    (njexl)students = [ {'roll' : 1, 'name' : 'X' } , {'roll' : 3, 'name' : 'C' } , {'roll' : 2, 'name' : 'Z' } ]
    =>@[{roll=1, name=X}, {roll=3, name=C}, {roll=2, name=Z}]


And now we are to sort based on "name" attribute:

    (njexl)sorta{ $[0].name < $[1].name }(students)
    =>[{roll=3, name=C}, {roll=1, name=X}, {roll=2, name=Z}]

Obviously we can do it using the roll too:

    (njexl)sorta{ $[0].roll < $[1].roll }(students)
    =>[{roll=1, name=X}, {roll=2, name=Z}, {roll=3, name=C}]


Thus, the standard comparison function can be passed straight in the anonymous function block.

## Loading arbitrary Jars and calling Classes 

The big issue with Java is CLASSPATH. To remedy, we have load() function call.
It can load arbitrary jars from a directory, recursively - and then instantiate and call
arbitrary classes. 


### Creating Java Classes

new() creates Java classes. Obviously, the class has to be in the class path.
To demonstrate - take a look around the following code sample :


    (njexl)load('/Users/noga/.m2/repository/xerces/xercesImpl')
    =>true
    (njexl)x = new('org.xml.sax.SAXException', 'foo bar' )
    =>org.xml.sax.SAXException: foo bar
    (njexl)x.getMessage()
    =>foo bar

Thus, one can continue programmatic with arbitrary classes - loaded from arbitrary locations.

### Almost Java - Web Driver 

A much more interesting example is using Selenium Webdriver: 

    //loading the path from where all class jars will be loaded 
    load('/selenium/lib') or bye('Could not load class path!')
    //import classes 
    import 'org.openqa.selenium.firefox.FirefoxDriver' as fdriver
    import 'org.openqa.selenium.firefox.internal.ProfilesIni' as profile 
    import 'java.lang.Thread' as thread 
    //
    allProfiles = new ( profile )
    p = allProfiles.getProfile("auto")
    //init a web driver 
    driver = new (fdriver,p)
    // go to the url 
    driver.get("http://www.google.com/webhp?complete=1&hl=en")
    // call java thread as if in a namespace 
    thread:sleep(1000)
    // exit 
    driver.quit()
    // return if needed 
    return 0 


## Inter Operating with Operating System

To do so, we have the system() function:

    (njexl)system("ls -l")
    total 24
    -rw-r--r--  1 noga  wheel  8299 May 17 18:28 README.md
    drwxr-xr-x  4 noga  wheel   136 May 13 22:20 doc
    drwxr-xr-x  9 noga  wheel   306 May 24 19:18 lang
    drwxr-xr-x  9 noga  wheel   306 May 13 22:20 script_testing
    drwxr-xr-x  5 noga  wheel   170 May 13 22:20 testing
    =>0 # This is the exit status 
    (njexl)

And in case of error :


    (njexl)system("lslx")
    Error : method 'system' in error : at line 1, cols 1:14
    Caused By : java.io.IOException: Cannot run program "lslx": error=2, No such file or directory


In case of a program which exist, but the run was unsuccessful :

    (njexl)system("ls -l /xxx")
    ls: /xxx: No such file or directory
    =>1

### The Threading issue 
Part of interacting with OS is to reply on native threads. This is what Java does, 
for most parts - and we rely on Java to do it's job, as of now.
Let's understand threading by using the sample : 


    import 'java.lang.System.out' as out 
    // this is the function to call inside a thread 
    def thread_func( a, b){
       c = a + b 
       out:printf("hello! : %s\n" , c )
       return c // unlike my ancestor java, I can return value 
    }

    def main(){
        // no arguments gets the current thread 
        ct = thread() 
        r = 0 // this is my return value 
        // create another thread 
        t = thread{  
            r = thread_func( __args__=$$ )  // note the syntax, I am saying $$ is the arguments 
                } ( 1, 2) // I am passing arguments 1,2 
        while ( t.isAlive() ){
           ct.sleep(1000) // yes, I can freely call Java functions!
        }
        return r // this is the return value 
    }

    // call main which now returns "r"
    main()


Note the curious "$$" usage, which signifies the arguments passed to the thread() function.
This is accessible in the anonymous method block.
The standard variable "$" contains the thread object itself, while "_" has the thread id.

### Tuning Code Snippets : clocking it 

Sometimes you need to find how much time a particular method or code snippet is taking.
Fear not, there is this clock function who would cater to your need.


    import 'java.lang.System.out' as out

    def long_method(){
       for ( i : [1:10000]){
          x = 0 // just ensuring the code snippet runs 
       }
    }

    /*  
     now i need to clock this method 
     The idea is it would pass the time in nanosec 
     or an error if any as an array: 
    */
    #(t,e) = clock{
       long_method()
    }()
    out:println(t)


The result this would be :
    
    prompt$ njexl tmp.jxl 
    13742064


### Waiting for Good Things to Happen : Until

Many times we need to write this custom waiter, where 
the idea is to wait till a condition is satisfied.
Of course we need to have a timeout, and of course we need a polling interval

To make this work easy, we have *until*.
The syntax is :

    until [ { condition-body-of-function } ]
           ( [ timeout-in-ms = 3000, [ polling-interval-in-ms = 100] ]) 

As all the stuff are optional, to get a 3 sec wait, just use :

     until() // this is cool 
     until (4000) // should be cool too !
     until (4000, 300 ) // not cool : 300 is rejected 

But, now with the expression on :

     i = 3 
     until { i = i - 1 ; i == 0 }( 1000, 200 ) // induce a bit of wait 
     // this returns true : the operation did not timeout 

     i = 100 
     until { i = i - 1 ; i == 0 }( 1000, 200 ) // induce a bit of wait 
     // this returns false : timeout happened  

Thus, the return values are *true* / *false* based on whether the condition met before timeout
happened or not. That solves the problem of waiting in general.


## The power of Randomness : using random function

With no argument, it returns you a Java 
[SecureRandom](https://docs.oracle.com/javase/8/docs/api/java/security/SecureRandom.html) object.
That is : 
   
    (njexl)r = random()
    =>java.security.SecureRandom@7106e68e
    (njexl)r.nextInt()
    =>580069037
    (njexl)r.nextBoolean()
    =>true
    (njexl)r.nextBoolean()
    =>true
    (njexl)r.nextBoolean()
    =>false
    (njexl)r.nextDouble()
    =>0.0420150972829294

Most of the time we need to select one from a set of choices.
To do so, we use random with a parameter :

    (njexl)x = [1,2,3,4,5]
    =>@[1, 2, 3, 4, 5]
    (njexl)random(x)
    =>4
    (njexl)random(x)
    =>4
    (njexl)random(x)
    =>1
    (njexl)random(x)
    =>4
    (njexl)random(x)
    =>2

But then, sometimes we need to select *k* items randomly from a list of *n* items :

    (njexl)random(x,10)
    =>[4, 2, 3, 3, 1, 3, 5, 5, 4, 2]
    (njexl)random(x,10)
    =>[5, 3, 2, 5, 4, 2, 5, 4, 2, 3] 


Same holds true with dictionary objects :

    (njexl)x = { 1:1 , 2:2 , 3:3 , 4:4 }
    =>{1=1, 2=2, 3=3, 4=4}
    (njexl)
    (njexl)random(x)
    =>@[3, 3]

And moreover :


    (njexl)random(x,2)
    =>{2=2, 4=4}
    (njexl)random(x,2)
    =>{2=2}
    (njexl)random(x,2)
    =>{4=4}


That should explain the tactics.
However, a more interesting stuff happens with string :

     (njexl)random("I am a Good Boy!")
    => 
    (njexl)random("I am a Good Boy!")
    =>y
    (njexl)random("I am a Good Boy!")
    =>m
    (njexl)random("I am a Good Boy!")
    =>!
    (njexl)random("I am a Good Boy!")
    =>y
    (njexl)random("I am a Good Boy!")
    =>o

While, this is seamless :
    
    (njexl)random("I am a Good Boy!", 10)
    =>yoyy m! y 

Thus, suppose, as an practical example - I need to select strings using alphabet and numbers:

     (njexl)random(['a':'z'].str + ['0':'9'].str , 10)
     =>m1onldd5s0

That is easy, right? It is, for sure!


## Using JSON

This is what JSON is : http://www.w3schools.com/json/
Typical JSON looks like : 

    $cat sample.json 
    {"employees":[
            {"firstName":"John", "lastName":"Doe"}, 
            {"firstName":"Anna", "lastName":"Smith"},
    {"firstName":"Peter", "lastName":"Jones"}
    ]}
    $

And now, thanks to already awesome way nJexl handles Hashes and Arrays, it is a perfectly valid Hash Object.
Thus, the standard : json() function works straight. 

        (njexl)j=json('sample.json')
        =>{employees=[Ljava.util.HashMap;@66048bfd}
        (njexl)j['employees']
        =>@[{firstName=John, lastName=Doe}, {firstName=Anna, lastName=Smith}, {firstName=Peter, lastName=Jones}]

Now obviously - One can access the fields : 

        (njexl)j['employees'][0]
        =>{firstName=John, lastName=Doe}
        (njexl)j['employees'][0]['firstName']
        =>John
        (njexl)j['employees'][0]['lastName']
        =>Doe

Awesome, right? You bet. No extra fancy stuff - nothing really. Less talk - and more work. The way the pioneers of computer science envisioned programming - which later the *software engineers* destroyed.


## SQL Math

Sometimes it is important to sum over a list or find min or max of a list where items can be made into numbers.
Clearly we can find a max or min by abusing the power of select statement :

    (njexl)x = [ 2,3,1,0,1,4,9,13]
    =>@[2, 3, 1, 0, 1, 4, 9, 13]
    (njexl){ min = x[0] ; select{ where( min > $  ){  min = $  }}(x)  } 
    (njexl)[1,0]
    (njexl)min
    =>0

This is a nice trick. But, then a better idea is to have an in built function to get the job done:

    (njexl)sqlmath(x)
    =>@[0, 13, 33]  # first min, 2nd Max, 3rd sum

Like always, this function also takes anonymous function as input :

    (njexl)sqlmath{$*$}(x)
    =>@[0, 169, 281]

Thus, it would be very easy to define on what we need to sum over or min or max.
Essentially the anonymous function must define a scalar to transfer the object into.

## Min and Max 
It is sometimes important to find min and max of items which can not be cast into numbers directly.
For example one may need to find if an item is within some range or not, and then finding min and max becomes important.
Thus, we can have : 

    (njexl)x = [ 2,3,1,0,1,4,9,13]
    =>@[2, 3, 1, 0, 1, 4, 9, 13]
    (njexl)minmax(x)
    =>@[0, 13]
    (njexl)x = [ "a" , "b" , "e" ]
    =>@[a, b, e]
    (njexl)minmax(x)
    =>@[a, e]
    (njexl)students = [ {'roll' : 1, 'name' : 'X' } , {'roll' : 3, 'name' : 'C' } , {'roll' : 2, 'name' : 'Z' } ]
    =>@[{roll=1, name=X}, {roll=3, name=C}, {roll=2, name=Z}]
    (njexl)minmax{ $[0].name < $[1].name }(students)
    =>@[{roll=3, name=C}, {roll=2, name=Z}]


## Truly Exceptional Cases 

As we build on Java, and Java has exceptions - sometimes we are not sure if some function would generate exception
or not. 

### The Try Function 

For those scenarios - we have try() function.

    (njexl)import 'java.lang.Integer' as Integer
    =>class java.lang.Integer
    (njexl)Integer.parseInt('xxx')
    Error : java.lang.NumberFormatException: For input string: "xxx"method invocation error : 'parseInt' : at line 1, cols 9:23
    (njexl)try { Integer.parseInt('xxx') } (null)
    =>null
    (njexl)e = try { Integer.parseInt('xxx') } ()
    =>java.lang.NumberFormatException: For input string: "xxx"

Thus, the idea is to eat up the exception, and then give a suitable default.

### Multi Valued Return 

The try{}() function is a very poor remnant of C++ bad design. A better idea is to make a function return 
multiple value. In fact, the function would always return a single value, but in case it generates error, 
one can easily catch that by catching the function error values in a tuple.

Here is how a Tuple works :

    (njexl)#(a,b) = [1,2,3] // stores a = 1, b = 2
    =>[1, 2]
    
Now, to store err values : 

    (njexl)#(o,:e) = my_undefined_var
    =>[null, com.noga.njexl.lang.JexlException$Variable: 
            com.noga.njexl.lang.Main.interpret@98![11,27]: 
           '#(o,:e)  = my_undefined_var;' undefined variable : 'my_undefined_var' ]    

So you see, the syntax ":var" in the tuple actually catches the error if any occurs in evaluating the 
expression in the right side. When you are specifying ":var", you are being explicit to the interpreter
that exception can happen, and please catch it for me in the variable var.

Tuples error return can be used in many ways: 

     #(o,:e) = out:printf("%d", "xxxx" )
     out:printf("%s\n", o )
     out:printf("%s\n", e )

Produces the output :

     null
     java.util.IllegalFormatConversionException: d != java.lang.String   

Finally, the integer parsing :

    (njexl)import 'java.lang.Integer' as Integer
    =>class java.lang.Integer
    (njexl)#(o,:e) = Integer:parseInt('32')
    =>[32, null]
    (njexl)#(o,:e) = Integer:parseInt('Sri 420')
    =>[null, java.lang.NumberFormatException: For input string: "Sri 420"]



## The excess of Xml

Xml was, is, and always will be a [very bad idea](http://harmful.cat-v.org/software/xml/)


    > “XML combines the efficiency of text files with the readability of binary files” – unknown


But thanks to many *big* enterprise companies - it became a norm to be abused human intellect - the last are obviously Java EE usage in Hibernate, Springs and Struts. Notwithstanding the complexity and loss of precise network bandwidth - it is a very popular format. Thus - against my better judgment I could not avoid XML.

First let me show the xml : 

```xml
<slideshow 
title="Sample Slide Show"
date="Date of publication"
author="Yours Truly"
>

<!-- TITLE SLIDE -->
<slide type="all">
  <title>Wake up to WonderWidgets!</title>
</slide>

<!-- OVERVIEW -->
<slide type="all">
    <title>Overview</title>
    <item>Why <em>WonderWidgets</em> are great</item>
    <item/>
    <item>Who <em>buys</em> WonderWidgets</item>
</slide>
</slideshow>
```

Now how to use it :

    (njexl)x = xml('sample.xml')
    =>{ "name" : "slideshow" , "ns" : "", "prefix" : "", "text" : "\n    \n    \n    \n    \n", 
       "attr" : {"date" : "Date of publication","author" : "Yours Truly","title" : "Sample Slide Show"}, 
       "children" : [ { "name" : "slide" , "ns" : "", "prefix" : "", "text" : "\n        \n    ", 
       "attr" : {"type" : "all"}, "children" : [ 
       { "name" : "title" , "ns" : "", "prefix" : "", "text" : "Wake up to WonderWidgets!", "attr" : {}, 
       "children" : [  ] } ] },
       { "name" : "slide" , "ns" : "", "prefix" : "", "text" : "\n        \n        \n        \n        \n    ", 
        "attr" : {"type" : "all"}, "children" : [ { "name" : "title" , "ns" : "", "prefix" : "", 
        "text" : "Overview", "attr" : {}, "children" : [  ] },
       { "name" : "item" , "ns" : "", "prefix" : "", "text" : "Why  are great", "attr" : {}, 
       "children" : [ { "name" : "em" , "ns" : "", "prefix" : "", "text" : "WonderWidgets", "attr" : {}, 
       "children" : [  ] } ] },
       { "name" : "item" , "ns" : "", "prefix" : "", "text" : "", "attr" : {}, "children" : [  ] },
       { "name" : "item" , "ns" : "", "prefix" : "", "text" : "Who  WonderWidgets", "attr" : {}, 
       "children" : [ { "name" : "em" , "ns" : "", "prefix" : "", "text" : "buys", "attr" : {}, 
       "children" : [  ] } ] } ] } ] }

Thus, any node can be accessed like objects straight away.
This is the json form. Now, to convert this to an dictionary object form :

    (njexl)json(x)
     =>{ns=, children=[Ljava.util.HashMap;@506e1b77, prefix=, name=slideshow, text=
    
    
    
    
     , attr={date=Date of publication, author=Yours Truly, title=Sample Slide Show}} 


This is how one can convert XML to JSON objects, which is a bloated hash in nJexl.
The object container *x* is of type XmlMap - and there one can use xpath expressions straight away.
For example :
 
     (njexl)x.xpath('//title[1]')
     =>Wake up to WonderWidgets!

Now finding elements:

     (njexl)es = x.elements('//title')
        =>[{ "name" : "title" , "ns" : "", "prefix" : "", "text" : "Wake up to WonderWidgets!", "attr" : {}, "children" : [  ] }, { "name" : "title" , "ns" : "", "prefix" : "", "text" : "Overview", "attr" : {}, "children" : [  ] }]

To take a closer look :

      (njexl)es[0]
        =>{ "name" : "title" , "ns" : "", "prefix" : "", "text" : "Wake up to WonderWidgets!", "attr" : {}, "children" : [  ] }
        (njexl)es[1]
        =>{ "name" : "title" , "ns" : "", "prefix" : "", "text" : "Overview", "attr" : {}, "children" : [  ] }

Accessing properties would be :

    (njexl)es[1].text
    =>Overview



