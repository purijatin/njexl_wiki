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


 ## Arithmetic Operators 

These would be "+", "-", "*", "/".
It is customary to define them as is, with Complex number as an example, sans the "/". So we present that accordingly : 


	import 'java.lang.System.out' as out

	def Complex {
	def __new__ (me,x=0.0,y=0.0){
	  me.x = x
	  me.y = y
	}
	def __str__(me){
	  str:format('(%f,i %f)', me.x, me.y)
	}
	def __add__(me,o){
	  return new ('Complex' , me.x + o.x , me.y + o.y )
	}
	def __sub__(me,o){
	  return new ('Complex' , me.x - o.x , me.y - o.y )
	}
	def __mul__(me,o){
	  return new ('Complex' , me.x * o.x  - me.y * o.y , me.x * o.y + me.y * o.x )
	}
	}
	c1 = new ( 'Complex' , 1.0, 2.0 )
	out:printf( 'c1 : %s\n' , c1 )
	c2 = new ( 'Complex' , 2.0, 1.0 )
	out:printf( 'c2 : %s\n' , c2 )
	out:printf( 'c1 + c2 : %s\n' , c1 + c2 )
	out:printf( 'c1 - c2 : %s\n' , c1 - c2 )
	out:printf( 'c1 * c2 : %s\n' , c1 * c2 )


This generates, as expected : 

       Script imported : JexlMain@/Codes/Java/njexl/target/../samples/class_demo2.jexl
       c1 : (1.000000,i 2.000000)
       c2 : (2.000000,i 1.000000)
       c1 + c2 : (3.000000,i 3.000000)
       c1 - c2 : (-1.000000,i 1.000000)
       c1 * c2 : (0.000000,i 5.000000)
 
And that tells you something about Arithmetics.



## Other Logical Operators

Other operators which can be overloaded are "|" or the "or" operator, "&" or the "and" operator, 
"^" or the "xor" operator. I do not see they are much useful, and in case they are, for those rare scenarios, they are overloaded anyways,before hand e.g. in case of Sets, Arrays, and Lists.


