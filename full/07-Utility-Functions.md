# Utility Functions

## Contents
* [Anonymous Function](#anonymous-function)
    * [Rationale](#rationale)
    * [Keywords](#keywords)
* [Numerics](#numerics)
    * [Defaults](#defaults)
    * [byte](#byte)
    * [char](#char)
    * [short](#short)
    * [int](#int)
    * [long](#long)
    * [INT](#int) --> converts to big integer
    * [float](#float)
    * [double](#double) 
    * [DEC](#dec) --> converts to big decimal
* [Collections](#collections)
    * [array](#array) --> creates an array from arguments 
    * [list](#list) --> creates a list from args 
    * [set](#list) --> creates a set from args 
    * [dict](#dict) --> creates a dictionary from args
* [Find Operations](#find-operations)
    * [index](#index) --> finds item in an indexable collection return the index 
        * [rindex](#rindex) --> finds item in an indexable collection from end 
    * [select](#select) --> selects items from a list matching a predicate 
    * [partition](#partition) --> simultaneously partition elements into match and no match 
* [join](#join) --> [Join](https://en.wikipedia.org/wiki/Join_(SQL))s two or more lists, based on 
condition
* [sort](#sort) ( sorta | sortd ) --> sort a list in ascending|descending order 
* [Random Operations](#random-operations)
    * [shuffle](#shuffle) --> shuffles a list/array 
    * [random](#random) -> selects element[s] from a list/array/enums/string  randomly
* [Input Output](#input-output)
    * [read](#read) --> read from standard input or a location completely, returns a string 
    * [write](#write) --> to a file or PrintStream or does a POST to a web url 
    * [send](#send) --> Send parameters to a URL using http protocols
    * [fopen](#fopen) --> opens a file for read/write/append 
    * [json](#json) --> read json file or a string and return a hash
    * [xml](#xml) --> read xml from file or string and returns an XmlMap data structure 
* [type](#type) --> Get's the type of a variable 
    * [inspect](#inspect) 
* [matrix](#matrix)
* [db](#db)
* [Higher Order Functions](#higher-order-functions)
    * [sqlmath](#sqlmath) --> finds min, Max, sum of a list in a single pass, for scalars
    * [minmax](#minmax) --> finds min, max of a list in a single pass, for those who are non scalar
    * [fold](#fold) ( lfold or rfold ) --> [fold](https://en.wikipedia.org/wiki/Fold_(higher-order_function)) functions, important on collection traversal 
* [Error Handling](#error-handling)
    * [try](#try) --> guards a native Java function call to ensure it does not throw exceptions
    * [Multiple Return](#multiple-return) 
* [load](#load) --> load arbitrary jars from a directory location, recursively
* [system](#system) --> make arbitrary system calls
* [thread](#thread) --> makes and starts a thread, with parameters 
* [clock](#clock) --> This is to tune method calls| code blocks, to check how much time was taken to run for a piece of code 
* [until](#until) --> A polling based waiter, waits till a duration till a condition is true 
* [hash](#hash) --> Generates md5 hash from string data 
* [tokens](#tokens) --> tokenizes a String 

## Overview

In this section we would list out some standard functions 
which are available.
In fact you have already met with some of them.
Thus, we would be familiarizing you guys with all of them.

## Anonymous Function

A basic idea about what it is can be found [here](https://en.wikipedia.org/wiki/Anonymous_function).
As most of the Utility functions use a specific type of anonymous function, 
whic his nick-named as "Anonymous Parameter" to a utility function.

#### Rationale 

Consider a simple problem of creating a list from an exisiting one, 
by modifyig individual elements in some way. This comes under [map](https://en.wikipedia.org/wiki/Map_(higher-order_function)) , but the idea can be shared much simply :

     l = list()
     for ( x : [0:n] ){
         l.add ( x * x ) 
     }
     return l 

Observe that the block inside the *for loop* takes minimal two parameters, 
in case we write it like this :

     def map(x){ x * x } 
     l = list()
     for ( x : [0:n] ){
         l.add( map(x) )   
     }
     return l 

Observe now that we can now create another function, lets call it list\_from\_list :

     def map(x){ x * x } 
     def list_from_list(fp, old_list)
         l = list()
         for ( x : old_list ){
             l.add( map(x) )   
         }
        return l
    } 
    list_from_list(map,[0:n]) // same as previous 2 implementaions

The same can be achived in a much sorter way, with this :

     list{ $*$ }([0:n])

The curious block construct after the list function is called anonymous (function) parameter, 
which takes over the map function. The loop stays implicit, and the result is equivalent 
from the other 3 examples.

#### Keywords

For an anonymous function parameters, there are 3 implicit guranteed arguments :

* $ --> Signifies the item of the collection , we call it the *ITEM*
* $$ --> The context, or the collection itself , we call it the *CONTEXT* 
* \_ --> The index of the item in the collection, we call it the *ID* 

Another case to case parameter is :

* _$_ --> Signifies the partial result of the processing , we call it *PARTIAL*

Thus, armed with these concepts we can proceed to understand Utility functions.

## Numerics

These are type conversion and type coercing functions.
These comes with what is known as fallback or default values:

#### Defaults

All the function is of the form :

      convert_to_type_name( value_to_be_converted [, fail_safe_value = null ] )

The way it works is, if the function *convert_to_type_name* fails to convert 
*value_to_be_converted* into *type_name* , then , it would return the *fail_safe_value*.
It always returns a [Java Boxed type](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html) , hence the failsafe is actually *null*. 

#### Byte

Converts a number literal into a byte:

    (njexl)byte(10000)
    =>16
    (njexl)byte(0x0a)
    =>10
    (njexl)byte('acx')
    =>null // failed to convert
    (njexl)byte('acx',0) // defaults to 0
    =>0


#### Short

Converts a number literal or a character into a short:

    (njexl)short('a')
    =>97
    (njexl)short('b')
    =>98
    (njexl)short('0')
    =>0
    (njexl)short('1')
    =>1
    (njexl)short('A')
    =>65

#### Int

Converts an object into an integer:

    (njexl)int(' 12 ')
    =>12
    (njexl)int(3.2)
    =>3
    (njexl)int(-3.2)
    =>-3

#### Long

Converts an object into a long:

    (njexl)long(-3.2)
    =>-3
    (njexl)long('112121111')
    =>112121111


#### INT 

Converts an object into a [BigInteger](http://docs.oracle.com/javase/8/docs/api/java/math/BigInteger.html):

    (njexl)INT(3)
    =>3
    (njexl)INT('1010', 2) // specify base 
    =>10
    (njexl)INT(1010, 2)
    =>10

#### Float

Converts an object into a float:

    (njexl)float(0.01)
    =>0.01
    (njexl)float('0.01')
    =>0.01
    (njexl)float('0.0x', 10.0)
    =>10.0

#### Double

Converts an object into a double:

    (njexl)double(0.00000001)
    =>9.99999993922529E-9 // observe the issue here...

Compare this against automatic type casting of nJexl :

    (njexl)x = 0.00000001
    =>1.0E-8
    (njexl)type(x)
    =>class java.lang.Float

#### DEC

Converts an object into a [BigDecimal](http://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html):

    (njexl)x = DEC(0.00000001)
    =>1.0E-8
    (njexl)type(x)
    =>class java.math.BigDecimal

## Collections

nJexl has particularly interesting collections, and all of them comes with map functionalities.

### Array 

The most generic type that can encompass the list of objects gets into the array type , observe :

    (njexl)x = [1,2,3]
    =>@[1, 2, 3]
    (njexl)type(x)
    =>class [I // integer array 
    (njexl)x = [1,2,3,0.1]
    =>@[1, 2, 3, 0.1]
    (njexl)type(x)
    =>class [Ljava.lang.Number; // number array 
    (njexl)x = [1,2,3,0.1,'hows this?']
    =>@[1, 2, 3, 0.1, hows this?]
    (njexl)type(x)
    =>class [Ljava.lang.Object; // now, becomes object array 

To ensure that we generate a type erased object array, we have *array* function :

    (njexl) x = array(1,2,3,4)
    =>@[1, 2, 3, 4]
    (njexl)type(x)
    =>class [Ljava.lang.Object;
    (njexl)x[0] = 's'
    =>s
    (njexl) x 
    =>@[s, 2, 3, 4]

One can use map function while creating the array :

    (njexl) x = array{ $**3 }(1,2,3,4)
    =>@[1, 8, 27, 64]

### List

Clearly default arrays are not mutable, so we can not add/remove items.
Hence, we need the *list* guy (or gal, based on your perspective) :

    (njexl)x = list(1,2,3,'s')
    =>[1, 2, 3, s]
    (njexl)type(x)
    =>class com.noga.njexl.lang.extension.datastructures.XList
    (njexl)x+= 10
    =>[1, 2, 3, s, 10]
    (njexl)x
    =>[1, 2, 3, s, 10]
    (njexl)x-= 's'
    =>[1, 2, 3, 10]
    (njexl)x
    =>[1, 2, 3, 10]

One can use map function while creating the list :

    (njexl) x = list{ $**3 }(1,2,3,4)
    =>[1, 8, 27, 64]

### Set 

Sets are lists where the following properties are hold true :

* All the item objects are unique  
* Searching time for an object ( contains() method ) takes theta(1) time.

Thus:

    (njexl)x = set(1,2,3,'s')
    =>S{ 1,2,3,s }
    (njexl)x-= 's'
    =>S{ 1,2,3 }
    (njexl)type(x)
    =>class com.noga.njexl.lang.extension.datastructures.ListSet

Suppose I want to identify unique items from a list :

    (njexl)x = [1,2,2,3,4,5,1,'1','4']
    =>@[1, 2, 2, 3, 4, 5, 1, 1, 4]
    (njexl)s = set(x)
    =>S{ 1,2,3,4,5,1,4 }

Observe the issue of '4' != 4. To fix that :

    (njexl)s = set{int($)}(x)
    =>S{ 1,2,3,4,5 }


### Dict 

Hashes are lists where the following properties are hold true :

* All the item objects are a 2-tuple ( pair ) of ( key, value )  
* Searching time for an object using the key ( get(key) method ) takes theta(1) time.

Hence, we need to give the *dict* function 

#### From Two Lists

    (njexl)nums = [0,1,2]
    =>@[0, 1, 2]
    (njexl)names = ['zero', 'one', 'two' ]
    =>@[zero, one, two]
    (njexl)x = dict(nums, names)
    =>{0=zero, 1=one, 2=two}

#### From A Single List of Complex Object

Suppose I want to group by a list of dates by the milli seconds :

    (njexl)d = date()
    =>Mon Nov 30 20:29:45 IST 2015
    (njexl)d.time
    =>1448895585279

Now observe :

    (njexl)dates = [date() , date(), date() ]
    =>@[Mon Nov 30 20:30:33 IST 2015, Mon Nov 30 20:30:33 IST 2015, Mon Nov 30 20:30:33 IST 2015]
    (njexl)d = dict{ [$.time , $] }(dates)
    =>{1448895633778=Mon Nov 30 20:30:33 IST 2015}

The issue is that the millisec is not unique enough. To avoid collision :

    (njexl)d = dict{ k = $.time; [ k , (k @ _$_ )?(_$_[k] += $ ): list($) ] }(dates)
    =>{1448895633778=[Mon Nov 30 20:30:33 IST 2015, Mon Nov 30 20:30:33 IST 2015, Mon Nov 30 20:30:33 IST 2015]}


## Find Operations

These functions lets you find items from a collection when some conditons are satisfied.
That condition is specified using Anonymous Parameter ( function ).

### Index 

We ask *what is the index of an item for a collection where a particular condition is true?*
To answer :

    (njexl)l = [3,4,1,2,3,5,6]
    =>@[3, 4, 1, 2, 3, 5, 6] 
    (njexl)index(5,l) // search 5 in l
    =>5

Now a condition : where at an item exist whose value is greater than 4 ?

    (njexl)index{ $ > 4 }(l)
    =>5
    (njexl)l[5]
    =>5

#### Sorted Order

Now, whether a list is in sorted order ?

    (njexl)l
    =>@[3, 4, 1, 2, 3, 5, 6]
    (njexl)index{ _> 0 and $ < $$[_-1] }(l)
    =>2 // sorted : false 
    (njexl)sorted
    =>@[0, 1, 2, 3, 4]
    (njexl)index{ _> 0 and $ < $$[_-1] }(sorted)
    =>-1 //sorted : true 

#### List in a List

Suppose we need to find if a list is in another list in the same order or not.
For example, take the lists [2,3] which is inside [1,2,3,4] :

    (njexl)index( [2,3] , [1,2,3,4] )
    =>1

This is only applicable for premitive types only.

#### RIndex 

*rindex* finds the index in reverse :

    (njexl)l = list{ random(20) }([0:10])
    =>[10, 12, 5, 12, 4, 0, 11, 16, 17, 10]
    (njexl)rindex{ $ < 8 }(l)
    =>5
    (njexl)index{ $ < 8 }(l)
    =>2


### Select 

We ask *what are the items in a collection where a particular condition is true?*
In the last example, find the list of items which are out of order ?
To answer : 

    (njexl)l
    =>@[3, 4, 1, 2, 3, 5, 6]
    (njexl)select{ _> 0 and $ < $$[_-1] }(l)
    =>[1]

Suppose now we make it slightly better, given a collection find out which are greater than 10:

    (njexl)l = list{ random(20) }([0:10])
    =>[1, 16, 17, 10, 17, 12, 7, 6, 5, 11]
    (njexl)select{ $ > 10  }(l)
    =>[16, 17, 17, 12, 11]

You can read about the *random* function in here, later.

### Partition 

The function partition() is a fairly interesting one, which given a criterion splits a collection into two parts,
one that matches the criterion, and another that does not match.

    (njexl)marks = [ 34, 45, 23 , 66, 89, 91 , 25, 21, 40  ]
    =>@[34, 45, 23, 66, 89, 91, 25, 21, 40]
    (njexl)#(pass, fail) = partition{ $ > 30 }(marks)
    =>@[[34, 45, 66, 89, 91, 40], [23, 25, 21]]
    (njexl)pass
    =>[34, 45, 66, 89, 91, 40]
    (njexl)fail
    =>[23, 25, 21]


What we are doing here is partitioning the marks using the criterion, if anything is over 30, it is pass else fail.

## Join

Join is the natural extension of multiplication over sets and list.
For more information on join see [Cartesian Product](https://en.wikipedia.org/wiki/Cartesian_product). Note that this is trivial in nJexl :

    (njexl)b = [0,1]
    =>@[0, 1]
    (njexl)B = [false, true ]
    =>@[false, true]
    (njexl)b * B 
    =>[[0, false], [0, true], [1, false], [1, true]]

But when we need to *select* specific tuples based on a condition, then *join* becomes useful.
Suppose I want to create all possible pairs from a list such that ( x, y ) with x < y :
otherwise known as combination of elements from a list of size 2 :

    (njexl)l = [1,2,3,4]
    =>@[1, 2, 3, 4]
    (njexl)join{ $.0 < $.1 }(l,l)
    =>[[1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]]

Hence, creating a general combination should be easy, and can be found [here]().

### Unjoin with Division operator 

Given we have two lists, which can actually be multiplied togather ( join{true} ),
can we unjoin them again? The short answer is yes, we can :

    (njexl)l=[1,2,3,4]
    =>@[1, 2, 3, 4]
    (njexl)m = ['a', 'b' ]
    =>@[a, b]
    (njexl)x = l * m
    =>[[1, a], [1, b], [2, a], [2, b], [3, a], [3, b], [4, a], [4, b]]
    (njexl)x / l  
    =>[] ## because, the order of multiplication matters, so does division
    (njexl)x / m 
    =>[1, 2, 3, 4] ## this is fine...

Notice that the division x / l did not yield a result, because unline a scalar multiplication where :

      a * b = b * a 

Vectored multiplications are not that way, simply because :

    (njexl)x = l * m 
    =>[[1, a], [1, b], [2, a], [2, b], [3, a], [3, b], [4, a], [4, b]]
    (njexl)y = m * l
    =>[[a, 1], [a, 2], [a, 3], [a, 4], [b, 1], [b, 2], [b, 3], [b, 4]]

The generated tuples are simply of different order.
Hence :

    a * b != b * a //in general 

And thus, to recover the multiplicand, we need to reverse the tuples in the list :

    (njexl)y = x.map{ list( $[1], $[0] ) }()
    =>[[a, 1], [b, 1], [a, 2], [b, 2], [a, 3], [b, 3], [a, 4], [b, 4]]
    (njexl)y / l
    =>[a, b] ## This is 'm'


## Sorting 

Sorting is trivial: 

    (njexl)sorta(cards) // ascending 
    =>[A, B, C, D]
    (njexl)cards
    =>@[D, A, C, B]
    (njexl)sortd(cards) //descending 
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

## Random Operations

### Shuffle

In general, testing requires shuffling of data values. Thus, the function comes handy:

    (njexl)cards = [ 'A', 'B' , 'C' , 'D' ] 
    =>@[A, B, C, D]
    (njexl)shuffle(cards)
    =>true ## returns true/false stating if shuffled
    (njexl)cards
    =>@[D, A, C, B]

### Random 

*random* function is multi-utility function.

#### Secure Random 

With no arguments it returns a SecureRandom :

    (njexl)random()
    =>java.security.SecureRandom@722c41f4
    (njexl)random()
    =>java.security.SecureRandom@722c41f4

and this is one singleton copy.

#### Random Selection 

This function can be used in selecting a value at random from a collection :

    (njexl)l = [0,1,2,3,4]
    =>@[0, 1, 2, 3, 4]
    (njexl)random(l)
    =>1
    (njexl)random(l)
    =>3

#### Next Random Number

##### Gaussian 

Passing 0 in *random* generates a next Gaussian no:

    (njexl)random(0)
    =>0.9131393731199431
    (njexl)random(0)
    =>0.009358487404312056

##### Long 

Passing a long value in *random* generates a next random long :

    (njexl)random(1l)
    =>7068829011849732717
    (njexl)random(1l)
    =>1901973770576958250

##### Double 

Passing a double value in *random* generates a next random double (0,1) :

    (njexl)random(1.0d)
    =>0.12311179484333468
    (njexl)random(1.0d)
    =>0.628967216979326

##### Float 

Passing a float value in *random* generates a next random float (0,1) :

    (njexl)random(0.1f)
    =>0.16727042
    (njexl)random(0.1f)
    =>0.78145444

##### Big Decimal 

Passing a BigDecimal value in *random* generates a next random BigDecimal (-1,1) :

    (njexl)random(0.1b)
    =>0.6417086806782025625338158493597255590
    (njexl)random(0.12b)
    =>-0.17245931155157498232577765084961997466372636935429851783


##### Boolean 

Passing a boolean value in *random* generates a next random boolean [false,true] :

    (njexl)random(true)
    =>false
    (njexl)random(true)
    =>true
    (njexl)random(true)
    =>true

##### Big Integer 

Passing a BigInteger value in *random* generates a next random BigInteger at least 4 times
the decimal digit size of the input big integer in binary : 

    (njexl)random(2h)
    =>-69850602459196581995882918417905179234
    (njexl)random(2h)
    =>1757842021475808004834441916751385577

##### Generating Random String 

Using this big integer facility one can generate random string in an alphabet :

    (njexl)s = random(10h)
    =>-916871517100790515212023227986211915036839442604969003661944492722491029115
    (njexl)s.toString(26)
    =>-o50gj0ndf0afjdp7bn8f1md8gb9cme5mbd55gg1ap677c2lbhd0h9
    (njexl)s.toString(36)
    =>-1th6m6zbeybrmghrvtr62oqfcwzp8hqa3xgdkzitvao4mlv23


## Input Output

I/O is very simple in nJexl. Observe the following.

### read 

Generally can be used to read from :

#### Standard Input 

With no arguments, reads a *line* from standard input :

    line = read() // reads a line 
    write(line) // writes the line back again 

#### File

With a string argument reads the whole file :

    (njexl)read('tmp.jxl')
    =>line = read()
    write(line)

#### Url 

With a string argument matching url gets the data from there like [curl]() :

    (njexl)read('http://jsonplaceholder.typicode.com/posts/1')
    =>{
      "userId": 1,
      "id": 1,
      "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
      "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
    }

#### PrintStream

With a InputStream argument reads one line at a time from that:


### write 

Generally can be used to write into :

#### Standard Out

With one arguments, writes a *line* into standard output :

    (njexl)write('hello, world!')
    hello, world!
    =>null

One can do [printf]() like formatting on the argument :

    (njexl)write('hello, world! %s\n', "Noga" )
    hello, world! Noga
    =>null

#### File 

With 2 arguments and no formatting , first argument is taken as file location,
and the 2nd argument as data string to be written to the file :

    (njexl)write('tmp.txt' , "Hello!")
    =>null

and we check the file :

    cat tmp.txt
    Hello!

#### Url 

If the first parameter is an URL, then the *write* function actually *post* the next parameter
there, which is to be a Dictionary contaning parameters to do *post*.

#### PrintWriter

Given the first parameter is a PrintWriter, write writes back a line to the PrintWriter.

### Send 

First parameter is an URL, then the *send* function actually use the next parameter
as the http protocol to send data to the url, using the last parameter
which is to be a Dictionary contaning parameters.


### FOpen

*fopen* lets one do file operations.
With no arguments it lets one return a BufferedReader over system input.
With one argument it returns a BufferedReader.
With 2 arguments, it is like this :

     fopen('path', mode )

With mode parameters are :

* "r" --> read only, file must exist 
* "w" --> write only, file will be created if not there, truncated to 0 size if exists 
* "a" --> append mode, file will be created if not there, start append to the last  


### json 

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

### xml 

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


### type

*type* gets the type of the variable :

    (njexl)type([1])
    =>class [I
    (njexl)type(list(1))
    =>class com.noga.njexl.lang.extension.datastructures.XList
    (njexl)[] isa [2,3]
    =>false
    (njexl)[0] isa [2,3]
    =>true


#### Inspect 

The function *inspect* returns funcion names and field names of an object or a class:

    (njexl)inspect([])
    =>Type : [Ljava.lang.Object;
    ==== Fields ===
    === Methods ===
    getClass
    wait
    hashCode
    equals
    notifyAll
    clone
    finalize
    toString
    registerNatives
    notify



### matrix 

It has it's own  seperate doc, see [Data Tables]()

### db 

It has it's own  seperate doc, see [Database]()


## Higher Order Functions

Higher order functions are functions taking another function as input

### SQLMath

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

### MinMax 
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


## Fold 

One can notice a pattern from the list(), the select() and the minmax().
All we are trying to do, is to recursively apply some function to *transform* the original list.
Can we generalize it? Yes, we can, and that brings to the folding part.
The syntax of fold is :

     <lfold|rfold> [ anonymous function ] ( list-argument [, initial-seed ] )

The *lfold* folds left wise, while *rfold* folds right.
Now, an example is needed, finding the sum of all numbers in a list :


     (njexl)a = [0,1,2,3]
     =>@[0, 1, 2, 3]
     (njexl)lfold{ _$_ + $ }(a,0) // note the seed element is 0 
     =>6

Now, finding product of all items in a list :
     
     (njexl)a = [1:6].list()
      =>[1, 2, 3, 4, 5]
     (njexl)lfold{  _$_ * $ }(a,1)
     =>120

Finding minimum of a list (oh yes, we do have a problem here with hard coding ) :

     (njexl)rfold{ continue( _$_  < $ ) ; _$_ = $ ;  }(a,100)
     =>1 

Note that the curious 

    _$_  

signifies and stores the partial result of the fold. 

## Error Handling

As we build on Java, and Java has exceptions - sometimes we are not sure if some function would generate exception
or not. 

### Try

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

### Multiple Return 

The try{}() function is a very poor remnant of C++ bad design. A better idea is to make a function return 
multiple value. In fact, the function would always return a single value, but in case it generates error, 
one can easily catch that by catching the function error values in a tuple.

#### Using Tuples

Here is how a Tuple works :

    (njexl)#(a,b) = [1,2,3] // stores a = 1, b = 2
    =>[1, 2] // splits the array/list
    (njexl)#(a,b,c,d) = [1,2,3]
    =>@[1, 2, 3, null] // excess will be filled in null.
    

Just like one can splice it from left, one can splice it from right:

    (njexl)#(:a,b) = [1,2,3]
    =>@[2, 3] // from right 
    (njexl)#(:a,b,c,d) = [1,2,3]
    =>@[null, 1, 2, 3] // follows the same protocol 

Hence, Tuples can be used to splice through an array/list.

#### Using Tuple to Catch Errors

Now, to store err values one must use a tupe of size 2 with ":error_holder" in the right: 

    (njexl)#(o,:e) = my_undefined_var
    =>[null, com.noga.njexl.lang.JexlException$Variable: 
            com.noga.njexl.lang.Main.interpret@98![11,27]: 
           '#(o,:e)  = my_undefined_var;' undefined variable : 'my_undefined_var' ]    

So you see, the syntax ":var" in the tuple actually catches the error if any occurs in evaluating the expression in the right side. When you are specifying ":var", you are being explicit to the interpreter that exception can happen, and please catch it for me in the variable var.

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


## load  

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


## System

It is necessary to inter operate with Operating Systems.
To do so, we have the system() function:

    (njexl)system("ls -l")
    total 24
    -rw-r--r--  1 noga  wheel  8299 May 17 18:28 README.md
    drwxr-xr-x  4 noga  wheel   136 May 13 22:20 doc
    drwxr-xr-x  9 noga  wheel   306 May 24 19:18 lang
    drwxr-xr-x  9 noga  wheel   306 May 13 22:20 script_testing
    drwxr-xr-x  5 noga  wheel   170 May 13 22:20 testing
    =>0 # This is the exit status for success
    (njexl)

### Error 
And in case of error :

    (njexl)system("lslx")
    Error : method 'system' in error : at line 1, cols 1:14
    Caused By : java.io.IOException: Cannot run program "lslx": error=2, No such file or directory

In case of a program which exist, but the run was unsuccessful :

    (njexl)system("ls -l /xxx")
    ls: /xxx: No such file or directory
    =>1

## Thread

Part of interacting with OS is to reply on native threads. This is what Java does, 
for most parts - and we rely on Java to do it's job, as of now.
Let's understand threading by using the sample : 


    // this is the function to call inside a thread 
    def thread_func( a, b){
       c = a + b 
       write("hello! : %s\n" , c )
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

### Clock 

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
    write(t)


The result this would be :
    
    prompt$ njexl tmp.jxl 
    13742064


### Until

Many times we need to write this custom waiter, where 
the idea is to wait till a condition is satisfied.
Of course we need to have a timeout, and of course we need a polling interval.
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
     until { i-= 1 ; i == 0 }( 1000, 200 ) // induce a bit of wait 
     // this returns true : the operation did not timeout 
     i = 100 
     until { i-= 1 ; i == 0 }( 1000, 200 ) // induce a bit of wait 
     // this returns false : timeout happened  

Thus, the return values are *true* / *false* based on whether the condition met before timeout
happened or not. That solves the problem of waiting in general.

### Tokens

In some scenarioes, one needs to read from a stream of characters (String)
and then do something about it.

One such typical problem is to take CSV data, and process it. 
Suppose one needs to parse CSV into a list of integers.
e.g. 
  
      s = '11,12,31,34,78,90' // CSV integers 
      (njexl)l = select{ where ( ($ = int($) ) !=null ){ $ } }(s.split(','))
      =>[11, 12, 31, 34, 78, 90] // above works

But then, the issue here is : the code iterates over the string once to 
generate the split array, and then again over that array to generate the list of integers, selecting only when it is applicable.

Clearly then, a better approach would be to do it in a single pass, so :

    s = '11,12,31,34,78,90'
    tmp = ''
    l = list()
    for ( c : s ){
        if ( c == ',' ){
            l += int(tmp) ; tmp = '' ; continue 
        }
        tmp += c
    }
    l += int(tmp) 
    out:println(l)

Which produces this :

    >[11, 12, 31, 34, 78, 90]

Thus, it works. However, this is a problem, because we are using too much coding.
Fear not, we can reduce it :

    tmp = ''
    l = select{ 
        if ( $ == ',' ){  $ = int(tmp) ; tmp = '' ; return true } 
        tmp += $ ; false  }(s.toCharArray() )
    l += int(tmp) 

Ok, goodish, but still too much code. Real developers abhor extra coding.
The idea is to generate a state machine model based on lexer, in which case,
the best idea is to use the *tokens* function :

     tokens( <string> , <regex> , match-case = [true|false] ) 
     // returns a matcher object 
     tokens{ anon-block }( <string> , <regex> , match-case = [true|false] ) 
     // calls the anon-block for every  matching group  
     
Thus, using this, we can have:

    l = tokens{ int($) }(s, '\d+')
    // what to do : string : regex 


And this is now : cool. That works.
That is what the people wants, attain more with very less.


## Hash 

Sometimes it is important to generate *hash* from a string.
To do so : 

    (njexl)hash('abc')
    =>900150983cd24fb0d6963f7d28e17f72

It defaults to "MD5", so :

    (njexl)hash( 'MD5' ,'abc')
    =>900150983cd24fb0d6963f7d28e17f72

They are the same.
One can obviously change the algorithm used :

     hash([algo-string , ] <string> )

