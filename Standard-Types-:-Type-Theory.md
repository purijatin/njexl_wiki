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

Same with any other types.
      
## Date & Time


