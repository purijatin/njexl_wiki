# Java Inter-Operatibility

The idea is simple : make nJexl extend Java objects.
This can be done trivially, with : 


	/* Showing inheritence from native Java Objects :
	ref : http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/lang/String.java
	One can use this to find out the internals
	*/

	import 'java.lang.System.out' as out
	import 'java.lang.String' as String


	def XString : String {
	     // have a constructor
	     def __new__(me,s=''){
	         //me.__anc__('String',s)
	     }
	     // define the toString()
	     def __str__(me){
	        me.supers['String']
	     }
	}

	cs = new ('XString',"hello, world!")
	out:println(cs)
	out:println(cs.length())
	out:println(cs.charAt(3))
	out:println(cs isa String)
 

As we can see, any standard Java objects can be extended, with nJexl classes. The result is the same, only that it allows now multiple inheritance.

The result that comes from it: 

       Script imported : JexlMain@/Codes/Java/njexl/samples/java_inherit.jexl
       hello, world!
       13
       l
       true

Thus, the interop, is perfectly done. The *isa* also works as expected.
One needs to understand that the cost of extending Java object is higher than the cost of extending native nJexl objects, and thus - one should be prepared for the performance hits that come in with it.


## The Ancestors Function 

That brings the question of, how to call ancestors constructors?
Clearly with a language in multiple inheritance, there has to be a way to call super class constructor, in this case, one can actually call any ancestors! That syntax is : 


	def XString : String {
	     // have a constructor
	     def __new__(me,s=''){
	     	 // call ancestor : 'String'
	         me.__anc__('String',s)
	     }
	     // define the toString()
	     def __str__(me){
	        me.supers['String']
	     }
	}


This is how any superclass and upwards *constructor* can be *called*, with desired parameters.







