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

Now 
 

