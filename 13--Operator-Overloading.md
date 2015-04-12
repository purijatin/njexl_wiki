# Operators & Overloading 

This is a very handy feature in many occasions, and hence implemented.

## Overloadable Operators 

I have taken a subset of the C++ and Java, and implemented it.
The Java like thing is obviously the comparator, which is special.

### About toString() and Comparing 

We have : equals and compare copied from Java. They are nice ideas, and often they do not commute.
As an example, two Objects might be equal, but not comparable, i.e. there might not be any [order](http://en.wikipedia.org/wiki/Order_theory) between them. For example, two complex numbers can be equal, but it is totally axiomatic to decide which one is bigger than which one. In that case you may want to work with equals, but not with compare. In any case, we start with the equal operation first.


	import 'java.lang.System.out' as out
	def MyClass{
	    def __new__ (me,a='xxx'){
	          me.s = a
	      }
	    def __eq__ ( me, o ){ // this is equals()
	       ( me.s == o.s ) 
	    }
	    def __str__(me){ // This is the toString()
	        str(me.s) 
	    }
	}
	x = new ('MyClass')
	y = new ('MyClass')
	out:printf("%s == %s ? %s\n", x,y, x==y)

This generates the reasonable output : 
    
       Script imported : JexlMain@/Codes/Java/njexl/target/../samples/class_demo2.jexl
       xxx == xxx ? true
 
This showcase very interesting things, that *str* is the one used for converting the object into string, and then *eq* is the one which compares two objects. 

### Other Comparison Operators 

Now other comparisons are "<", "<=" , ">" , ">=".
Too many, and thus, we use java compareTo() hocus focus. In fact we use it slightly differently.
The function is demonstrated here : 


	    def __cmp__(me, o ){
	        if ( me.s < o.s ){ return -1 }
	        if ( me.s > o.s ){ return 1 }
	        return 0 
	    }
        }// end of class body 
	x = new ('MyClass',10)
	y = new ('MyClass',120)
	out:printf("%s < %s ? %s\n", x,y, x < y)
	out:printf("%s > %s ? %s\n", x,y, x > y)
	out:printf("%s <= %s ? %s\n", x,y, x <= y)
	out:printf("%s >= %s ? %s\n", x,y, x >= y)


When we add it to the class MyClass, we see the following : 

       10 < 120 ? true
       10 > 120 ? false
       10 <= 120 ? true
       10 >= 120 ? false
 
And that is pretty good, should we say?
Note that the equal() and compareTo() == 0 ideally should match. If they do not, it is your problem, not mine.


  


