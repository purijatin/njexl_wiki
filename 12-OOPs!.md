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


 


