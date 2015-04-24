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


## General Numeric Type Conversion 
The general idea is convert type peacefully.
That idea xxx(y) would convert an object peacefully. If it can not, it would return null.

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


     
## Date & Time
Simplification of date & time are premium from a testing perspective.
Thus, we have much easier functions   

   
     (njexl)date()
     =>Tue Mar 31 19:03:12 IST 2015
     (njexl)str(date())
     =>20150331


The string conversion is easy with str(). But in what format?
If one is using date() object - then the format used is : http://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html 
In any case - the formatting can be changed : 

     (njexl)str(date(),'yyyy/dd/MM')
     =>2015/31/03

In any case - the time() function can be used to get time():

     (njexl)time()
     =>2015-03-31T19:08:49.723+05:30
     (njexl)str(time(),'yyyy/dd/MM')
     =>2015/31/03

The formatting guide is : http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html

The reverse creation of date / time is *obviously* possible. 
    
     (njexl)time('2015/31/03','yyyy/dd/MM')
     =>2015-03-31T00:00:00.000+05:30
     (njexl)date('2015/31/03','yyyy/dd/MM')
     =>Tue Mar 31 00:00:00 IST 2015 

Formatting guide as stated above.

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


The specific operator "isa" lets you know if one object type is actually of another type or not : 

      (njexl)1 isa int(0)
      =>true
      (njexl)1.0 isa int(0)
      =>false
      (njexl){:} isa dict()
      =>true
      (njexl)[] isa array()
      =>true

And then, finally -- integer types are well converted : 

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

And thus, this - section ends.
