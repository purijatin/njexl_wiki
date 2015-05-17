# Object Oriented Programming 

Please try to avoid it. Objects are inherently harmful, and more harmful if you are no experienced enough, which is probably never. In any case, nJexl supports full object oriented paradigm, if one choose to mess with the structure of nature i.e. software testing.


## Defining Objects

We use the *def* keyword again, to define a class. Why *def* ? Because if we use *class* as keyword, then it messes up java class keywords. Clearly, in java one can not have foo.if as field. Because if is a keyword.
In the same way,  x.class becomes impossible to access in nJexl if one use 'class' as keyword. That is the technical reason. The more succinct reason is, why do one need *class*? It is pretty apparent that we are calling a class because there is no "()" in the declaration!  

	def  MyClass{
	}

Thus, this *MyClass* is a class. That is all there is to define one.

### new, and using Members of Class 

Fields are what makes the class as a state-machine, and thus, class may have fields. As always, it is meaningless to have fields declaration, because it is a dynamic language. Thus, any method, like this : 


	import 'java.lang.System.out' as out 
	def  MyClass{
	    def  member(me, y){
	        me.y = y 
	        out:printf('My field "y" is valued [%s]\n', me.y )
	    }
	}
	mc = new ( 'MyClass')
	mc.member(10)


Trying to access a field using the *me* keyword, would *create* a field called "y".
Yes, you guessed it right, class is a oversized Hash. It is exactly what it really is.
We also know that *new* can be used to create even nJexl *objects*. 
We also understood that member functions can be called using x.member() syntax.

The Python *self* becomes *me* in here. Why *me* ? Because it is smaller to type in.
This is *only* a keyword if used inside a method, and as a first parameter.
Else it is an ordinary literal.

The result of the earlier code would be: 

     My field "y" is valued [10]

And that demonstrates the class creation and using members.
Clearly new can take parameter args - and those parameters are instance initialiser arguments.
Note that class does not get created here, they merely gets initialised. 


## Inheritance & Polymorphism

Nothing is as dangerous as a battle of inheritance. Thus, learning from history, I found that not allowing multiple inheritance was a terrible idea. One, should, have multiple parents. Even bacterium can have multiple parents.
And the language users are (mostly) human, and I fail to see one of their parents being an interface.


Thus, nJexl supports multiple inheritance.
The syntax of inheritance is simple : 

	import 'java.lang.System.out' as out 
	def  MyClass{
	    def  member(me, y){
	        me.y = y 
	        out:printf('MyClass field "y" is valued [%s]\n', me.y )
	    }
	}
	mc = new ( 'MyClass')
	mc.member(10)
	def ChildClass : MyClass{
	}
	cc = new ( 'ChildClass')
	cc.member(100)
	
And as a result : 
     
        MyClass field "y" is valued [10]
        MyClass field "y" is valued [100]

Amen to that. Importantly, as one can see, we have *polymorphism*, another terribly bad idea - waiting to go wrong : 


	import 'java.lang.System.out' as out 
	def  MyClass{
	    def  member(me, y){
	        me.y = y 
	        out:printf('MyClass field "y" is valued [%s]\n', me.y )
	    }
	}
	mc = new ( 'MyClass')
	mc.member(10)
	def ChildClass : MyClass{
		def  member(me, y){
	        me.y = y 
	        out:printf('ChildClass field "y" is valued [%s]\n', me.y )
	    }
	}
	cc = new ( 'ChildClass')
	cc.member(100)


Which diligently outputs : 

      MyClass field "y" is valued [10]
      ChildClass field "y" is valued [100]

 Whose "y" I am now dealing with? God should be knowing this, but we know better. ChildClass.

### The supers[] syntax : Polymorphism

Now, suppose I need to use the parents member(), how should I do it?

	cc = new ( 'ChildClass')
	cc.member(100)
	cc.supers['MyClass'].member(42)
	
And this diligently generate : 

      MyClass field "y" is valued [10]
      ChildClass field "y" is valued [100]
      MyClass field "y" is valued [42]


Now then, we have two different fields, one of the parent, one of the child, having same name.
This can be made pretty clear by : 


	cc = new ( 'ChildClass')
	cc.member(100)
	cc.supers['MyClass'].member(42)
	out:println(cc.y)
	out:println(cc.supers['MyClass'].y)


Which, of course generates  : 

       MyClass field "y" is valued [10]
      ChildClass field "y" is valued [100]
      MyClass field "y" is valued [42]
      100
      42

That is that. And, no, parent has no idea about child's members, but child has total knowledge about parents members. Inverting, then : 

     cc = new ( 'ChildClass')
     cc.supers['MyClass'].member(42) // create parent member - before child
     out:println(cc.y)
     out:println(cc.supers['MyClass'].y)
	
Would work, as *expected* and thus : 

    MyClass field "y" is valued [10]
    MyClass field "y" is valued [42]
    42
    42

And this is how the polymorphism really works. Bottom, UP.

### The root of some Evil : Multiple Inheritance 

One can, inherit as many *class* as they want. Thus, a very simple demonstration would be : 

    import 'java.lang.System.out' as out 
    def Some1{
        def __new__ (me){
              me.s = 'xxx'
          }
        def do_print(me){
            out:println('Some1!')
        }
    }
    def Some2{
       def do_print(me){
           out:println('Some2!')
       }
    }
    def Complex : Some1 , Some2 {
    }
    c = new ('Complex')
    c.do_print()
    

In here, suppose I try to call do_print(), what happens?
*BOOM*.

        Exception in thread "main" java.lang.Error: java.lang.Exception: Ambiguous Method : 'do_print' !
	at noga.commons.njexl.extension.oop.ScriptClassInstance.execMethod(ScriptClassInstance.java:53)
        .... <more>

Basically it says, sorry, I found multiple matching methods, **be specific**.
Thus, in the next iteration, we fix it, using supers.

    c = new ('Complex')
    c.supers['Some1'].do_print()
    c.supers['Some2'].do_print()

Now what happens?

     Some1!
     Some2!

Yeppie! We are good! A specific method can be tracked down to the core - and can be called if need be!
Vast improvement over standard polymorphism!

These whole discussion becomes kind of moot, if we add the do_print() method to the Complex class: 

    def Complex : Some1 , Some2 {
        def do_print(me){
           out:println('Complex!')
       }
    }

    c = new ('Complex')
    c.do_print()
    c.supers['Some1'].do_print()
    c.supers['Some2'].do_print()

In this case, the result is pretty boring : 


     Complex!
     Some1!
     Some2!


Which is boring, because it is supposed to be that way.


## Cross File Importing

Is, in fact supported. Highly not recommended though. It is better to have all the class intermingling in the same file, but then some might want to extend you. Albeit rarely done in practice, unless forced too, 
that is a distinct possibility. Hence, this is how it is done : 


    import  '_/class_demo.jexl' as  CD 
    def ExternalImport : CD:Some1 {}
    s1 = new ( 'CD:Some1' )
    s1.do_print()
    ei = new ( 'ExternalImport' )
    ei.do_print()
    ei.supers['CD:Some1'].do_print()
    return 0

If we run it, we get : 
    
    Script imported : JexlMain@/Codes/Java/njexl/target/../samples/jclass.jexl
    Script imported : CD@/Codes/Java/njexl/target/../samples/class_demo.jexl
    Some1!
    Some1!
    Some1!

That should conclude the OOPs! section.
Next are operators, and overloading of them.

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


## Eventing 
In any form of programming, eventing is nothing but a way to have callable functions inserted before
and after some method call happens. That is made easy here.
To support eventing one should override the methods *__before__* and *__after__*.

An example would explain the idea : 

    /*
     Showcases Eventing.
     The crucial methods are __before__ and __after__
    */
    import 'java.lang.System.out' as out

    def  MyEventClass {
         //  before hook
         def __before__(){
             out:printf("Before : %s\n", __args__)
         }
         // after hook 
         def __after__(){
             out:printf("After : %s\n", __args__)        
         }
         // define a standard function 
         def say_hello(){
            out:printf("Hello : %s\n", __args__)
         }
    }
    x = new ( 'MyEventClass' )
    // before this method __before__ would be called 
    @@x.say_hello(" I am Eventing ")
    // __after__ would be called after this 

And thus, when we run it - we have :

    njexl ../src/lang/samples/eventing.jexl 
    Script imported : JexlMain@/Codes/Java/nJexl/src/lang/samples/eventing.jexl
    Before : @@ | say_hello | @[ I am Eventing ]
    Hello :  I am Eventing 
    After : @@ | say_hello | @[ I am Eventing ]

Thus, any class can oversee any of it's function by attaching event to them.



# Java Interoperability

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

