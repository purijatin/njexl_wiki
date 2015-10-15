# Theory of Types 
 
The general types which can be converted are : 
      
    type(x) // tells what type x is
    byte(x)
    short(x)
    char(x)     
    int(x)
    bool(x)
    long(x)
    double(x)
    str(x)
    date(x) // converts to java.lang.Date 
    time(x) // converts to JodaTime --> http://www.joda.org/joda-time
    array(x,y,z,...) // object array : with functionals  
    [ x, y, z, ... ]  // Full sized object array 
    list(x,y,...) // generates an ArrayList 
    set(x,y,z,...) // a ListSet, ensures that the set is indexible 
    dict(...) // a dictionary 
    [x:y:z] // is a range type -- notably of lang or Joda DateTime 

## The Define[d]s
In a scripting language, it is important to understand that variables gets declared on the fly.
Thus, it is important to know whether or not a variable is defined or not. That gets solved with the trick : 
 
    (njexl)#def x // functional form --> only references are allowed 
    =>false
    (njexl)#def(10) // function form --> anything is allowed
    =>true
    (njexl)#def(x.y) // function form --> again
    =>false

### Getting Objects Defined in the Script 
Defined get's used as to find script objects like methods and classes.

    import 'java.lang.System.out' as out

    def generic(){
       out:println("I am generic function")
    }
    def gen_before(){
       out:println("I am generic before : " + __args__ )
    }
    m = #def( 'my:generic' )
    out:println(m)

When we run this script, we get : 
  
    njexl ../src/lang/samples/tmp.jxl 
    ScriptMethod{ name='generic', instance=false}
    
It returned the method which got defined by the name 'generic'.
In the same way, it can return class too.


## The Literals

Literals are stuff those are terminal node of a language grammar, essentially constant values which get's assigned to variables.
So we start with the primitive ( albeit boxed ) types : 

    (njexl)x='hello'
    =>hello
    (njexl)type(x)
    =>class java.lang.String

Then we have boolean.

     (njexl)x=true
     =>true
     (njexl)type(x)
     =>class java.lang.Boolean

Now, we have integer family ( natural numbers ) : 

     (njexl)x=1
     =>1
     (njexl)type(x)
     =>class java.lang.Integer
     (njexl)x=100000000000
     =>100000000000
     (njexl)type(x)
     =>class java.lang.Long
     (njexl)x=100000000000000000000000000
     =>100000000000000000000000000
     (njexl)type(x)
     =>class java.math.BigInteger
     

And then we have rational types - the floating point numbers : 

    (njexl)x=0.1
    =>0.1
    (njexl)type(x)
    =>class java.lang.Float
    (njexl)x=0.000000010001
    =>1.0001E-8
    (njexl)type(x)
    =>class java.lang.Float
    (njexl)x=0.000000010001010101
    =>1.0001010101E-8
    (njexl)type(x)
    =>class java.lang.Double
    (njexl)x=0.000000010001010101000000000000101
    =>1.0001010101000000000000101E-8
    (njexl)type(x)
    =>class java.math.BigDecimal
   

Thus we see that the type assignment is pretty much magical.
However, one can force it to be a specific type. In that case: 

    (njexl)x=0.1b
    =>0.1
    (njexl)type(x)
    =>class java.math.BigDecimal
    (njexl)x=0.1d
    =>0.1
    (njexl)type(x)
    =>class java.lang.Double
    (njexl)x=1h
    =>1
    (njexl)type(x)
    =>class java.math.BigInteger



## General Numeric Type Conversion 

The general idea is to convert type peacefully, i.e. without any stupidity called exception.
Primitive types are primitive, hence they are non null, and hence nullables can be used to 
to convert an object peacefully. If it can not, it would return null.

    (njexl)int('xx')
    =>null

This is bad. What if you really want a fallback - when one can not type convert?

      (njexl)bool('xx',false)
      =>false 
      (njexl)int('xx',42)
      =>42

Same with any other types. The functions DEC() and INT() can be used to convert things into big decimal and big integers. 

     (njexl)x=DEC(0.1)
     =>0.1
     (njexl)type(x)
     =>class java.math.BigDecimal
     (njexl)x=INT(1)
     =>1
     (njexl)type(x)
     =>class java.math.BigInteger

Generally this is to be used to type promotion ( upward ) : 

    (njexl)x=0.1
    =>0.1
    (njexl)type(x)
    =>class java.lang.Float
    (njexl)x=DEC(x)
    =>0.1
    (njexl)type(x)
    =>class java.math.BigDecimal


     
## Date & Time
Simplification of date & time are premium from a testing perspective.
Thus, we have much easier functions   

    (njexl)date()
    =>Tue Mar 31 19:03:12 IST 2015
    (njexl)str(date())
    =>20150331


The string conversion is easy with str(). But in what format?
If one is using date() object - then the format used is [Java Date Format](http://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html).

In any case - the formatting can be changed : 

    (njexl)str(date(),'yyyy/dd/MM')
    =>2015/31/03

In any case - the time() function can be used to get time():

    (njexl)time()
    =>2015-03-31T19:08:49.723+05:30
    (njexl)str(time(),'yyyy/dd/MM')
    =>2015/31/03

The formatting guide is : [Joda Time](http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html)

The reverse creation of date / time is *obviously* possible. 
    
    (njexl)time('2015/31/03','yyyy/dd/MM')
    =>2015-03-31T00:00:00.000+05:30
    (njexl)date('2015/31/03','yyyy/dd/MM')
    =>Tue Mar 31 00:00:00 IST 2015 

Formatting guide as stated above. 
For invalid dates, it would return null. No leniency. Thus: 

    (njexl)date('20150230')
    =>null
    (njexl)date('20150222')
    =>Sun Feb 22 00:00:00 IST 2015

### Valid Date / Time
If you are trying to convert a string to a date/time     


### Strings 
Strings are not much interesting - apart from : 

    (njexl)s="hi all!"
    =>hi all!
    (njexl)s[2]
    => 
    (njexl)s[3]
    =>a
    (njexl)s[5]
    =>l

Which basically means string[index] is what returns the individual characters.

    (njexl)s[6].getClass()
    =>class java.lang.Character

size() works as intended : 

    (njexl)size(s)
    =>7
    (njexl)s.length()
    =>7
Another way to use the same size functionality is to use #|expr|.

    (njexl)#|s|
    =>7

Which is much more accessible short hand.
The type works as expected : 

    (njexl)type(0)
    =>class java.lang.Integer
    (njexl)type(0l)
    =>class java.lang.Long
    (njexl)type(0.0)
    =>class java.lang.Float
    (njexl)type(0.0d)
    =>class java.lang.Double
    (njexl)type("hi")
    =>class java.lang.String
    (njexl)type(date())
    =>class java.util.Date
    (njexl)type(time())
    =>class org.joda.time.DateTime


The specific operator *isa* lets you know if one object type is actually of another type or not : 

    (njexl)1 isa int(0)
    =>true
    (njexl)1.0 isa int(0)
    =>false
    (njexl){:} isa dict()
    =>true
    (njexl)[] isa array()
    =>true

And then, finally -- Integer/Numeric types are well converted : 

    (njexl)byte(1)
    =>1
    (njexl)byte('a')
    =>null
    (njexl)short('a')
    =>97
    (njexl)char(1)
    =>
    (njexl)char(0)
    =>
    (njexl)char('Z')
    =>Z

## The str() function 

str() is used to make everything back to string type.
This is a multi-utility function, capable of doing way more things than you can imagine.
A classic case is that of formatting floating point numbers.

    (njexl)x=1.2345
    =>1.2345
    (njexl)str(x,1)
    =>1.2
    (njexl)str(x,2)
    =>1.23
    (njexl)str(x,3)
    =>1.235
    (njexl)x=0.1b
    =>0.1
    (njexl)str(x,10) ## seamless with big decimals even...
    =>0.1000000000
    
That is darn good. Now then, this can be implicitly used in comparing floating point numbers.

    (njexl)x=1.2345
    =>1.2345
    (njexl)y=1.235
    =>1.235
    (njexl)str(x,2) == str(y,2)
    =>false
    (njexl)str(x,3) == str(y,3)
    =>true

Thus, it is not truncate, but it is -- round() that is taking place.
Hence it is very important for testing and business process.


# Collections 

Collections are list, set, hash and then tuples.
There are two generic operations over collection.

    (njexl)a = list()
    =>[]
    (njexl)empty(a)  // if a collection is empty 
    =>true 
    (njexl)size(a)  // what is the size of the collection 
    =>0 

Now then size() is a kind of bad way of representing things so we have #|expr| operator.

    (njexl)#|1.0-0.4|
    =>0.6
    (njexl)#|1.0-4|
    =>3.0
    (njexl)a = [1,2,3,4]
    =>@[1, 2, 3, 4]
    (njexl)#|a|
    =>4
    (njexl)#|"Hello!"|
    =>6
    (njexl)#|10.0b - 0.1| ## potentially seamless on Big Types...
    =>9.9
    (njexl)type(_o_) ## a trick, the output is stored as _o_
    =>class java.math.BigDecimal

      
Thus, #|expr| also is abs() function, which is important for Business Logic and Validation.

## Difference in size() and #|| operator

The clear difference of operation is the treatment of null.
Sometimes you need to use size of null as 0. 
Sometimes you need to treat size of null as < 0, i.e. -1.
For that :

    (njexl)size(null)
    =>-1
    (njexl)#|null|
    =>0

The issue is that #|| operator can not return signed value, ever. Hence, it is scaled to 0.
size() on the other hand - should handle null. Thus, to check null values, a handy operation 
should be : 

    (njexl)x = null
    =>null
    (njexl)size(x)<0
    =>true

along with the very specific : 

    (njexl)null == x
    =>true

Also every collection is indexable, col[x] is valid for all of them. And that includes for Sets too.

## Array
The most easily used one is an array type. Everything inside it automatically becomes an Object.
Thus : 

    (njexl)a = [1,2,3,4] 
    => @[1, 2, 3, 4]
    (njexl)a[0].getClass()
    =>class java.lang.Integer

As we can see arrays can be indexed by their offset. 
You can assign stuffs to arrays.

    (njexl)a = [1,2,'hi', "hello"]
    =>@[1, 2, hi, hello]
    (njexl)a[0]
    =>1
    (njexl)a[2]
    =>hi
    (njexl)a[2] = "bye"
    =>bye
    (njexl)a[2]
    =>bye

## Lists 
Interestingly, same is applicable for lists! 
One can easily convert an array to a list : 

    (njexl)a = list(a)
    =>[1, 2, bye, hello] 
    (njexl)a[0]
    =>1
    (njexl)a[2] = 'Australia 2015'
    =>Australia 2015
    (njexl)a
    =>[1, 2, Australia 2015, hello] 

### Anonymous Functions 

Sometimes you want to create a list, but then want to apply transform on each element.
Fear not : 

    (njexl)a
    =>[1, 2, Australia 2015, hello]
    (njexl)b = list{int($)}(a) // anything inside that { } gets applied over all the elements in the list 
    =>[1, 2, null, null]

Wait. That is a problem. 
None ordered those nulls!  So what should we do to filter out the elements who are not integers?
      
    (njexl)b = select{ where(int($)){ $ = int($) }}(a)
    =>[1, 2]

What's that? WHERE is proverbial *where* in SQL. When the expression for when is true - the block gets executed.
Inside the block the $ = int($) assigning the value of the element back after transform!
How cool is that?

## Sets
Sets are lists where occurrences are not repeated.
Thus : 

    (njexl)l = list(1,2,2,3,4,4,5)
    =>[1, 2, 2, 3, 4, 4, 5]
    (njexl)s = set(l)
    =>S{ 1,2,3,4,5 }


Anonymous functions becomes crucial here, because the key is the element itself : 

    (njexl)l = list(1,"1  ", "1") 
    =>[1, 1  , 1]
    (njexl)s = set(l)
    =>S{ 1,1  ,1 }

Should not they be all 1 ? You bet they should be, in that case : 

    (njexl)s=set{int($)}(l)
    =>S{ 1 }


just works!  

## MultiSet
A multi set is essentially a list - where items can be repeated. But using that construct one can compare lists.
How so? Because we start treating list as Maps, the key is the element (by default ) - and the values are the list of such elements. This interpretation makes sense. Let's take a look around it : 

    (njexl)list(1,1,2,2,3,2,4)
    =>[1, 1, 2, 2, 3, 2, 4]
    (njexl)mset(1,1,2,2,3,2,4)
    =>{1=[1, 1], 2=[2, 2, 2], 3=[3], 4=[4]} 

Note that, it actually keyed down the elements and stored them as list!
But why it does that? That is because sometimes different objects can be *mapped* to same. For example : 

    (njexl)mset{ int($) } ( 1, 1.05, "1" , 2, 2.1 )
    =>{1=[1, 1.05, 1], 2=[2, 2.1]}


Where do I need it? Yes, we need it in case of SQL Group By - and more interestingly when we compare 2 lists which can be keyed!

    (njexl)ms1 = mset{ int($) } ( 1, 1.05, "1" , 2, 2.1 )                      
    =>{1=[1, 1.05, 1], 2=[2, 2.1]}
    (njexl)ms2 = mset{ int($) } ( 1, 1.05, "1" , 2 )          
    =>{1=[1, 1.05, 1], 2=[2]}
    (njexl)set:mset_diff(ms1,ms2)
    =>{1=[I@53e25b76, 2=[I@73a8dfcc}
    (njexl)d = set:mset_diff(ms1,ms2)
    =>{1=[I@ea30797, 2=[I@7e774085}
    (njexl)list(d[1])  
    =>[3, 3]
    (njexl)list(d[2])
    =>[2, 1]


It basically saying that for the key 1, both left and right multisets are having same number of values (3). 
But, for the key 2, left and right multisets are having different values , left : 2 and right : 1.
Of course all list operations are treated as multiset operations.

## Relation between Lists and Sets 
Those can be found by : 

    (njexl)set:set_relation([1],[2])
    =>INDEPENDENT
    (njexl)set:set_relation([1,2],[2,3])
    =>OVERLAP
    (njexl)set:set_relation([1,2,3],[1,2,3])
    =>EQUAL
    (njexl)set:list_relation([1,2],[2,3])
    =>OVERLAP



## Hashes or Dictionaries 
They are list of key value pairs.
They are accessible by syntax value = hash[key] 

    (njexl)cwc = { 2011 : 'Ind' , 2015 : 'Aus' }
    =>{2011=Ind, 2015=Aus}
    (njexl)cwc[2011]
    =>Ind
    (njexl)cwc[1981]
    =>null
 
Empty dictionaries can be created : 
      
    (njexl)ed = {:}
    =>{}
    (njexl)empty(ed)
    =>true
    (njexl)size(ed)
    =>0

And you can start putting things in it : 

    (njexl)ed[0]=0
    =>0
    (njexl)ed
    =>{0=0}
 
Whatmore, you can take two lists and make a dictionary out of it : 

    (njexl)cwc = dict([2011,2015],['Ind','Aus'])
    =>{2011=Ind, 2015=Aus}
    (njexl)cwc[2015]
    =>Aus


The syntax container[x] is overloaded. It works for every container type, including Sets which are made by nJexl. 
       
    s = set(1,2,3,4)
    =>S{ 1,2,3,4 }
    (njexl)s[0]
    =>1

At the same time - it is also a short hand for property!
For example : 

    (njexl)import 'java.lang.System' as SYS
    =>class java.lang.System
    (njexl)SYS['out']
    =>java.io.PrintStream@58d25a40

How cool is that? At the same time - we can start using it even : 

    (njexl)SYS['out'].println("Hello, Awesome nJexl!")
    Hello, Awesome nJexl!
    =>null

Thus, we can access any field of any class instance as this O['field name'] syntax!

### The String operation on Collections

Anything that is complex is a collection. Be it list, be it Hash, be it an Object.
In any case, sometimes we need to serialise back the object in question -- preferably so that it becomes a tuple.
How so? This demonstrates how : 


    (njexl)d={'a': 'A' , 'b' : 'B' }
    =>{a=A, b=B}
    (njexl)str{ [ $.a , $.b  ]}(d)
    =>A,B
    (njexl)str{ [ $.a , $.b  ]}(d,'#')
    =>A#B


This way, one can take arbitrary  object and can get a serialised form out of it.
Notice the use of "[" and "]" to make the functional expression as an *array*. Thus, we actually can take any list - and serialise it straight : 

    (njexl)str{ [ 1,2,3 ]}(0)
    =>1,2,3
    (njexl)str([1,2,3])
    =>1,2,3

Now, interestingly, we can use any string to use as delimiter : 

    (njexl)str([1,2,3],"&&")
    =>1&&2&&3
 
That should be good!

### Linearizing Tuple 
The usage of such a craft is to reduce dimension of multi dimensional arrays, and lists.
This is a handy example : 

    (njexl)D = [ {'a': 'A1' , 'b' : 'B1' } , { 'a' : 'A2' , 'b' : 'B2' } ]
    =>@[{a=A1, b=B1}, {a=A2, b=B2}]
    (njexl)x = set{  str{ [ $.a , $.b] }($, '#')  }(D) ## Use the str() to linearize the individual rows 
    =>S{ A1#B1,A2#B2 } ## Here, a 2-d array became single D array!



## Range Data Type
Another significant data type is called range, which is a type of iterator.
A range is generally written as [start:stop:spacing]. One can omit the spacing, 
thus one can have only [start:stop]. A range is an abstract entity and does not contains 
physically the yielded result of the iterator. 

For example :

    (njexl)[0:11]
    =>[0:11:1]
Stating that the start is 0, end is 11 ( exclusive ), step is 1.
The most important type of ranges are long and the date types.

### Long Range 

One can yield the list which is encoded by a range simply by:

    (njexl)r = [0:11]
    =>[0:11:1]
    (njexl)r.list()
    =>[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

One can give a different step :

    (njexl)r = [0:11:3]
    =>[0:11:3]
    (njexl)r.list()
    =>[0, 3, 6, 9]

### Date / Time Range

For business programming date/time range is important. Like Long range, it can be created by :

    (njexl)r = [date('20150101') : date('20150110')]
    =>Thu Jan 01 00:00:00 IST 2015 : Sat Jan 10 00:00:00 IST 2015 : PT86400S
    (njexl)r.list()
    =>[2015-01-02T00:00:00.000+05:30, 2015-01-03T00:00:00.000+05:30, 2015-01-04T00:00:00.000+05:30, 
    2015-01-05T00:00:00.000+05:30, 2015-01-06T00:00:00.000+05:30, 2015-01-07T00:00:00.000+05:30, 
    2015-01-08T00:00:00.000+05:30, 2015-01-09T00:00:00.000+05:30, 2015-01-10T00:00:00.000+05:30]


So, one can see that the default spacing is in ISO format designating it to be a single day.
One can read more about the spacing formats : [here](http://en.wikipedia.org/wiki/ISO_8601#Time_intervals).
In general the format is given by:

    PyYmMwWdDThHmMsS 

Curiously, date/time range also has many interesting fields : 

    r.years 
    r.months 
    r.weeks 
    r.days 
    r.hours 
    r.minutes 
    r.seconds 
    r.weekDays ## tells you number of working days ( excludes sat/sun ) between start and end!

Thus, this becomes a very important tool for business side of programming.

### String or Alphabet Range 

Another type of range is String range, this is defined as :

    (njexl)ci = ['a':'z']
    =>[a:z:1]

Now, to see what it actually contains, do the yielding :

     (njexl)ci.list()
      =>[a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z]

Now, most of the time we need to use a compressed format, hence :

     (njexl)ci.str
     =>abcdefghijklmnopqrstuvwxyz

Obviously it takes spacing element, thus:

    (njexl)ci = ['a':'z':3]
    =>[a:z:3]
    (njexl)ci.str
    =>adgjmpsvy

## Range in Reverse 

Ranges in general are in forward, but soemtimes one needs a decreasing range.
For example :

     (njexl)[9:0:-1].list()
     =>[9, 8, 7, 6, 5, 4, 3, 2, 1] // python like 
     (njexl)[9:0:-1].reverse // am I in reverse?
     =>true
     (njexl)[9:0:-1].reverse()
     =>[1, 2, 3, 4, 5, 6, 7, 8, 9]

### Range Checking 

Ranges ensures that the range when yielded would not be infinite.

    (njexl)[9:0:1]
    Error : Invalid Range! : at line 1, cols 1:7
    Caused By : java.lang.IllegalArgumentException: Sorry, can not make the loop infinite by range!

