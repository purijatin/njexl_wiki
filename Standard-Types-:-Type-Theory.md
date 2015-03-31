# Theory of Types 

What the design of jexl did - is type compression.
By that what we really mean is - it finds the minimal container type of an object while computing stuff.

    njexl>a = list(0,1,2)
    =>[0, 1, 2]
    njexl>a[0].getClass()
    =>class java.lang.Byte

This is kind of intriguing and saves memory.
But it is confusing - and hence - if you wan to compare and be sure - then you should always type cast.
    
     njexl>a = list{int($)}(0,1,2)
     =>[0, 1, 2]
     njexl>a[0].getClass()
     =>class java.lang.Integer

   
This happens *only* when one passes constant values in the parameters in a function.
The general types which can be converted are : 
     
      int(x)
      bool(x)
      long(x)
      double(x)
      str(x)
      date(x) // converts to java.lang.Date 
      time(x) // converts to JodaTime --> http://www.joda.org/joda-time
      list(x,y,...) // generates an ArrayList 
      set(x,y,z,...) // a ListSet, ensures that the set is indexible 

## General Numeric Type Conversion 
The general idea is convert type peacefully.
That idea xxx(y) would convert an object peacefully. If it can not, it would return null.

      njexl>int('xx')
      =>null

This is bad. What if you really want a fallback - when one can not type convert?

      njexl>bool('xx',false)
      =>false 
      njexl>int('xx',42)
      =>42

Same with any other types.
      
## Date & Time
Simplification of date & time are premium from a testing perspective.
Thus, we have much easier functions      
     njexl>date()
     =>Tue Mar 31 19:03:12 IST 2015
     njexl>str(date())
     =>20150331
The string conversion is easy with str(). But in what format?
If one is using date() object - then the format used is : http://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html 
In any case - the formatting can be changed : 

     njexl>str(date(),'yyyy/dd/MM')
     =>2015/31/03
In any case - the time() function can be used to get time():

     njexl>time()
     =>2015-03-31T19:08:49.723+05:30
     njexl>str(time(),'yyyy/dd/MM')
     =>2015/31/03

The formatting guide is : http://joda-time.sourceforge.net/apidocs/org/joda/time/format/DateTimeFormat.html

 



