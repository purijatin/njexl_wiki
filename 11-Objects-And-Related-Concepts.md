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

      // some class body 

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

    def generic(){
       out:println("I am generic function")
    }

    def gen_event(){
       event = __args__[0]
       out:printf("I am generic %s \n", event )
    }

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
    // get the say_hello method 
    m = x.NClass.method.say_hello
    out:println(m)
    // this get's the method 
    e = #def( 'my:gen_event' )
    out:println(e)
    m.before.add(e)
    // before this method __before__ would be called 
    @@x.say_hello(" I am Eventing ")
    // __after__ would be called after this 


    m = #def( 'my:generic' )
    out:println(m)

    // add before handler 
    m.before.add(e)
    // call and see what happens ?
    generic()
    // remove before handler 
    m.before.remove(e)
    // add a programmatic handler with MyEventClass
    // __before__ would be called 
    m.before.add(x)
    // add to after 
    m.after.add(e)
    // call and see what happens ?
    generic()


And thus, when we run it - we have :

    ScriptMethod{ name='say_hello', instance=false}
    ScriptMethod{ name='gen_event', instance=false}
    Before : @@ | say_hello | @[ I am Eventing ]
    I am generic __before__ | ScriptMethod{ name='say_hello', instance=false} | @[ I am Eventing ] 
    Hello :  I am Eventing 
    After : @@ | say_hello | @[ I am Eventing ]
    ScriptMethod{ name='generic', instance=false}
    I am generic __before__ | ScriptMethod{ name='generic', instance=false} | @[] 
    I am generic function
    Before : __before__ | ScriptMethod{ name='generic', instance=false} | @[]
    I am generic function
    I am generic __after__ | ScriptMethod{ name='generic', instance=false} | @[]


Thus, any class can oversee any of it's function by attaching event to them.

## Instance methods as Method Closure

It is known that the instance methods are static methods, with 
first argument being the instance object pointer.
Hence, it is reasonable to assume that one instance of an instance method 
would be bound to the instance, in some way.

Suppose here is the script :

    var count = 0 
    def MyClass{
        def __new__ (me,v=10){
            me.v = v
        }
        def my_before(me){
            out:println(me.v)
            count += me.v 
        }
    }
    mc1 = new ( 'MyClass' , 42 )
    mc2 = new ( 'MyClass' , 24 )

Now, given this, what would be the behaviour of the method "mc1.my_before" ?
This is a problem. The solution is non trivial, it assumes that the 
instance of the instance method is bounded to the instance, 
Hence, it is legal to have the following:


    b1 = mc1.my_before  // b1 is a closure of my_before(mc1) 
    b2 = mc2.my_before  // b2 is a closure of my_before(mc2)  

And now, it is perfectly legal to call them :

    b1() // prints 42
    b2() // prints 24

Running this gets :

    42
    24

So, the idea is that instance methods can be made bound to variable, 
when it is done, one can use that as a closure, and then call like anything.

## Static Method

One doees not need static methods, because as of now there are
no static variables. But in any case, this should explain static stuff :


    import 'java.lang.System.out' as out
    var count = 0 

    def MyClass{
        // This is a static method 
        def my_static(){
            out:println(" I am static!")
            // there are no static variables, 
            //but global var
            count += 42 
        }
    }
    // this is how you get the classes
    cs = my:classes()
    out:println(cs)
    // to access one class out of many 
    my_class = cs.MyClass
    out:println(my_class)
    // to find one method 
    m = my_class.methods.my_static
    out:println(m)
    // now call the method 
    m()
    // see count increased 
    out:println(count)
    // now a simple way to call, again 
    MyClass:my_static()
    // see count increased, again 
    out:println(count) 

When one runs this, the result is :

    {MyClass=nClass JexlMain:MyClass}
    nClass JexlMain:MyClass
    ScriptMethod{ name='my_static', instance=false}
     I am static!
    42
     I am static!
    84

Note the usage of *my:* directive to get the defined classes in the current module.
Also note that instance type is made to be false.

## A bit of Reflection  

Reflection is built-in, because it is a dynamic language.
Look at the below code :

    import 'java.lang.System.out' as out

    def MyClass{
        // just a constructor 
        def __new__(me, v=42){
           me.v = v 
        }
        // an instance method 
        def instance_func(me){
            out:println(me.v += 42 )
        }
    }
    obj = new('MyClass', 11)
    // call normally 
    obj.instance_func()
    // some dynmaic stuff 
    f_name = 'instance_func' // store name 
    f = obj[f_name] // this is good enough 
    // now we can call it 
    f()
    // same with variables
    v_name = 'v' // usage is simple 
    out:println( obj[v_name])

When we run it, we get this response :

    53
    95
    95

### Know Thyself 

How an instance knows that it is what type?
That requires knowing oneself, and that is done by :

    import 'java.lang.System.out' as out
    def Super{
        def __new__(me, svar=0){
            me.svar = svar
            out:printf("new %s : %s\n", me.$.name, me.svar )
        }
        def __str__(me){
            str:format("Super : (%d)", me.svar)
        }
    }
    s = new ('Super')
    out:println(s.$)

This prints :

    new Super : 0
    nClass JexlMain:Super

This tells that the type of the class is 'Super' and it is defined 
in the main Script.

Now, how to find list of all functions defined in here?

     out:println(s.$.methods)

Does the trick :

    {__new__=ScriptMethod{ name='__new__', instance=true}}

One can obviously find the superclass informtions too:

    def Child : Super {
        def __new__(me,cvar=11){
            me.cvar = cvar
        }
        def __str__(me){
            str:format("Child : (%d%d)", me.cvar , me.svar)
        }
    }
    c = new ( 'Child' )
    out:println(c)
    // find supers?
    out:println(c.$.supers)

This generates :

    new Super : 0
    Child : (110)
    {Super=nClass JexlMain:Super, JexlMain:Super=nClass JexlMain:Super}

Notice the duplicate binding of the name 'Super'.
It has bounded to the same class, essentially unique values are what matters.
So, we can replace that with :

    // find unique supers?
    out:println(set(c.$.supers.values()))
    // find the supers instances?
    out:println(set(c.supers.values()))

Which produces :

    new Super : 0
    Child : (110)
    S{ nClass JexlMain:Super }
    S{ Super : (0) }


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

## Multiple Inheritence , with Java and nJexl 

    import 'java.lang.System.out' as out
    import 'java.lang.String' as String
    // a super class  
    def Super{
        def __new__(me, svar=0){
            me.svar = svar
            out:printf("new %s : %s\n", me.$.name, me.svar )
        }
        def __str__(me){
            str:format("Super : (%d)", me.svar)
        }
    }
    // multiple inheritence from nJexl and Java 
    def Child : Super,String{
        def __new__(me, cvar=4, svar=2, my_str = 'I am a string' ){
            me.__anc__('Super',svar)
            me.__anc__('String', my_str )
            me.cvar = cvar 
            out:printf("new %s : %s\n", me.$.name, me.cvar )
        }
        def __str__(me){
            str:format("%s : (%d%d)", me.supers['String'] , me.cvar , me.svar)
        }
        def my_hash(me){
            me.svar + me.cvar + #|me.supers['String']|
        }
    }
    count = 0 
    child = new('Child')
    count += child.my_hash()
    out:println(child)
    child = new ('Child',5)
    count += child.my_hash()
    out:println(child)
    child = new ('Child',5,3)
    count += child.my_hash()
    out:println(child)
    child = new ('Child',5,3, "Hello, World")
    count += child.my_hash()
    out:println(child)
    l = child.length()
    count += l
    out:printf( "I can call String's methods! .length() ==> %d\n" ,l)
    out:println(count)
    count 

The result is as follows :

    new Super : 2
    new Child : 4
    I am a string : (42)
    new Super : 2
    new Child : 5
    I am a string : (52)
    new Super : 3
    new Child : 5
    I am a string : (53)
    new Super : 3
    new Child : 5
    Hello, World : (53)
    I can call String's methods! .length() ==> 12
    92

### Cross Referencing Classes from Other Files 
Suppose that we have to call a class from another source file
where it was defined.
To make it simplier, suppose that we have these definitions in another.jxl :

    // in another.jxl
    def XClass{
        def __new__(me,s=''){
            me.s = s
        }
        def __str__(me){
           str:format("%s", me.s)
        }
        def static_method(x,y){ x - y }
    }
    // a method 
    def my_random_method(a,b){ a + b }

And we want to call them from our main.jxl. How to call?

    import 'java.lang.System.out' as out
    // notice the import : no extension required "_" defines from current dir
    import '_/another' as AN  
    xs = new( AN.XClass , 'Hello Cross Ref Class!')
    out:println(xs)

This would print :

    Hello Cross Ref Class!

Now, suppose we need to call a static method of the XClass :

    out:println( AN.XClass.static_method(1,2) )

Would print :

    -1

One can call the method *my_random_method* also : 

     out:println(AN.my_random_method(1,2))

But a better bet is to call it :

    out:println(AN:my_random_method(1,2))

Which leads to cleaner, more readable approach : ":" means from a namespace.



