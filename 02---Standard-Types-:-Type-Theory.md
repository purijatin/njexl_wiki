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

Same with any other types.
      
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



