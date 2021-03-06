# Methods

## Contents
 * [Overview](#overview)
 * [Defining Methods](#defining-methods)
 * [Global and Local Scope and Variables](#global-and-local-scope-and-variables)
 * [Borrowing From Others](#borrowing-from-others)
       * [Script as a Method](#script-as-a-method)
 * [Recursion is Divine](#recursion-is-divine)
 * [Functional Programming](#functional-programming)
 * [Argument Passing](#the-args-constructs)
       * [Parameters](#parameters)
       * [Anonymous Argument](#anonymous-argument)
       * [Default Arguments](#default-arguments)
       * [Arguments to a Script](#arguments-to-a-script)
 * [Argument overwriting](#argument-overwriting)
 * [Assignment to Variables](#assignment-to-variables)
 * [Closures](#closures)
 * [Lambdas](#lambdas)
       * [Function Composition](#function-composition)
       * [Operators on Functions](#operators-on-functions)
       * [A nice way to Recursion](#a-nice-way-to-recursion)
 * [Eventing](#eventing)
       * [Implementation](#implementation)


## Overview

No matter how much you want to avoid them, methods are needed.
They are first-class entities in here, in the realm of nJexl.
They are of course, to be passed by names, or as variables 
and are defined to work as such. 
Formally in computer science a method can be defined by two different ways :

* [Lambda Calculus](https://en.wikipedia.org/wiki/Lambda_calculus)
* [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine)

Both are identical in power and freely convertible to one another.
That generated the [Church–Turing thesis](https://en.wikipedia.org/wiki/Church–Turing_thesis)
and is responsible for what is known as [Turing Completeness](https://en.wikipedia.org/wiki/Turing_completeness). In semi formal way, a language is Turing Complete if it allows :

* Arithmetic ( a + b )
* Logic ( a and b )
* Conditional Branching ( if ( a ) { return b } ) 
* Possibly infinite Memory ( in some sense ) 

[Back to Contents](#contents)

## Defining Methods 

*def* is used to define methods ( as well as class, which we would talk later ).

    // dummy.jexl
    def some_func(s){
        write(s)
        my:void_func() // my: ensures we always call this scripts void_func.
        true  ## makes it perl like
    }
    def void_func(){
        return false // standard return 
    }
    some_func("Hello, World!")


Note that curious "my" syntax. It basically tells nJexl that the method call should point to the current modules ( read script files ) void_func() -- not any other random script files. As one can import modules from other locations, this becomes of very importance.
The following command can be used to run the script file : 

    $ njexl dummy.jexl 
    Hello, World!

It shows how it all finally works out. 
The above sample also shows - how a method can call another method.

[Back to Contents](#contents)

## Global and Local Scope and Variables

All global variables are read-only locals. 
A variable declared as "var xxx" is a global variable where from local scope one can write.
While a simple assignment is local.

    var x = 0 // script global variable 
    def foo(){
       x += 1 // global update 
    }
    foo()
    foo()
    write(x)

This would print 2. However, if you fail to put var, then :

    x = 0 
    def foo(){
       x += 1 // use the global name to create a local var!
    }
    foo()
    foo()
    write(x)

would print 0. The global variable comes in local copy, 
but does not get write back to the original global one.

[Back to Contents](#contents)

## Borrowing From Others

It is sometime necessary to call methods defined in some other scripts. 
Unlike other languages that bask on the nesting of name spaces, I choose to create one with 0 nesting.
That means import statements must be uniquely mapped to an alias.

So, given I have this script : something.jxl : 

    def my_function(arg){
        write( arg )
    }
    my_function( "Hello, World")

And we need to call this method my\_function from another script. It would be created as such :

    // as in [I]mported [N]ame[S]pace
    import 'something.jxl' as INS 
    INS:my_function("Hi, There!")

Running this script would produce :

    $njexl tmp.jxl
    Hi, There!

### Script as a Method 

Given we have imported the script, can we actually use the whole script as a function?
Yes, we can, that would be easy with the \_\_me\_\_ function - which is a reserved function name:

    // as in [I]mported [N]ame[S]pace
    import 'something.jxl' as INS 
    INS:my_function("Hi, There!")
    // use __me__ to tell : run the whole script as function 
    INS:__me__()

Running this runs the imported jxl script as if it is a function.

[Back to Contents](#contents)

## Recursion is Divine 

As it is well said - to iterate is human, to recurse is divine, we take heart from it.
We showcase the factorial program : 

    /*
      The iterative version of the factorial
    */
    def factorial_iterative ( n ){
        if ( n <= 0 ){
            return 1
        }
        x = 1
        while ( n > 0 ){
            x = x * n
            n -= 1  
        }
        x // implicit return 
    }
    /*
       Recursive version of the factorial
    */
    def factorial_recursive(n){
        if ( n <= 0 ) {
            return 1
        } else {
            return n * my:factorial_recursive(n-1)
        }
    }
    size(__args__) > 1 or bye ( 'usage : ' + __args__[0] + '  <number> [-r : recursively]')
    if ( __args__.length == 2 ){
        y = factorial_iterative( __args__[1] )
    }else {
       y = factorial_recursive( __args__[1] )
    }
    out:println(y)

As usual, this also showcase the curious *args* construct. This is basically the arguments array passed to a method, or a script. Every method gets it's own *args*. If we try to run it, we get : 


    $ njexl factorial_sample.jexl 
    @[usage : ../samples/factorial_sample.jexl  <number> [-r : recursively]]

This is a pretty simplistic command line usage, and let's try it : 


    $ njexl factorial_sample 5
    120
    $ njexl factorial_sample 10
    3628800
    $ njexl factorial_sample 100
    933262154439441526816992388562667004907159682643816214685929638952175999 // next line
    932299156089414639761565182862536979208272237582511852109168640000000000 // next line 
    00000000000000


You can ommit the file name extension.
Default it would automatically look for 

     \.[jJ]([eE])?[xX][lL] // as a regex 

Hope that it is right? It is. In fact the automatic widening of number is something that is a feature of nJexl. One need not worry about *bounds*, after all, mind is [limitless!](http://www.imdb.com/title/tt1219289/)

Now, it is high time checking the recursive code: 

    $ njexl factorial_sample 410 -r
    7695091858866763366263438655668964498487195079235007091374790577795004189593254064
    4009028242721648765009379053940741552827255513576622685582156113545461753231364849
    9197738578570523818012538201385063090285424460199611481778391852796001105110093584
    4831851802529313770545060733557338996270927637881408403132889214625920594103521799
    8575436585792511310143770111437627499248635309480207239685907153937095328733446732
    2044770178851891029535737455704039401977329561807871916002416427278810253133027645
    3705091645613714661048537464122183580361871785567982867458423107016937009806098658
    2022601209290501398623321211544460014684526564737612076150781406607975373443115744
    7096491967424173308544693050340724076287605525142136305125706099435934563941672391
    4109104306653379839415514734121325618099566360897414758400000000000000000000000000
    000000000000000000000000000000000000000000000000000000000000000000000000000


Good enough, I suppose. Beware of recursion in nJexl. As it is divine, it is not well implemented by a human like me, and thus like all divine stuff, it would eventually fail you. It does not handle a depth of 420+ well. While I would like to work on optimising it, *premature optimisation is the root of many evils* , so ....

[Back to Contents](#contents)

## Functional Programming

Functions are first class members of the language. Thus, one can use anonymous function with any function of your choice, and you can pass functions to other functions, rather easily, using strings.
Here you go : 

	/*
	   Demonstrates functional arg passing in Jexl functions 
	*/
	import 'java.lang.System.out' as out
	import '_/samples/dummy.jexl' as dummy
	// This does not take any param, so, a call to this 
	def my_max_function(){
	    // Use a function that use functional : e.g. list 
	    o = sqlmath(__args__)
	    return o[1]
	}
	def func_taking_other_function( func ){
	   y = func( 10, 1, 2 )
	   out:println(y)
	}
	// in here would result in var args
	out.println(str(__args__))
	x = my_max_function{ int($) }( '1', 2, '3', 4, '5')
	out.println(str(__args__))
	out:println(x)
	// current script
	func_taking_other_function( 'my_max_function' )
	// other script
	func_taking_other_function( 'dummy:some_func' )
	out.println(str(__args__))


Notice that we are showcasing that args return backs to original - after function execution is over.

    $ njexl  functional_sample.jexl 
    Script imported : dummy@/Codes/Java/njexl/samples/dummy.jexl
    Ignoring re-import of [out] from [java.lang.System.out] 
    ../samples/functional_sample.jexl
    ../samples/functional_sample.jexl
    5
    10
    10
    true
    ../samples/functional_sample.jexl


Basically, it is calling methods "my_max_function" and "dummy:some_func" by name, and one can pass them as such, as string. What it also demonstrates that the ability take anonymous parameters for any function call. 

[Back to Contents](#contents)

## The Args constructs 

In nJexl, any function can take arbitrary no. of parameters. 
This is the topic for this section.

### parameters

One can define a function that takes parameters :

     def function(a){
        write(a)
     } 
     // call it :
     function(10) // prints 10
     function("hello") // prints hello

But it is totally fine not passing the required parameters, which would be 
automatically assigned the value *null* :

    function() // prints null

One can pass more parameters that it was defined to the function, w/o any weired set of behavior:

    function("Hello" , "How" , "are" , "you" ) // prints Hello

How to access the extra parameters passed which are not bound to any name are the discussion topic of next section.

### Anonymous Argument 

     __args__

is the construct. The purpose of it, is to be used to anonymously use the whole argument list ( actually Object array). Why they are useful? Mostly when I do not care about the arguments, but needs to process them, and names are last on my mind. 

To get into the grind, let's define :

     def function(){
        write( str(__args__, "##") )
     } 

and now, we use it :

    function("Hello" , "How" , "are" , "you" ) // prints Hello##How##are##you

Thus, we can access unnamed parameters using this.

A classic example is the one given at the start of the chapter : 


	// This does not take any param, so, a call to this 
	def my_max_function(){
	    // Use a function that use functional : e.g. list 
	    o = sqlmath(__args__)
	    return o[1]
	}


Here, the sqlmath() actually takes functional as input. and I want this sqlmath to process the arguments, rather than me. Hence, when I call : 

       x = my_max_function{ int($) }( '1', 2, '3', 4, '5')

The arguments are passed as is to the sqlmath, who handles all the anonymous function thing, and returns me the result.

[Back to Contents](#contents)

### Default Arguments 

Essentially, while *args* gets all the arguments anonymously, standard way works, as is shown.
Also, one can pass default arguments : 

	/*
	 Showcases the named parameters 
	*/
	import 'java.lang.System.out' as out 

	def some_method(param1='1',param2='2'){
	     out:printf('p1 is : %s\tp2 is %s\n', param1,param2)
	}

	// should take defaults 
	some_method()
	// only the fist parameter
	some_method(2)
	// both the positional paramater
	some_method(3,4)
	// specific parameter : 1 
	some_method(param2='42')
	// Parameter 2 
	some_method(param1='42')
	// parameters in reverse order 
	some_method(param2='42', param1='24')


The result would be, as expected:

     p1 is : 1	p2 is 2
     p1 is : 2	p2 is 2
     p1 is : 3	p2 is 4
     p1 is : 1	p2 is 42
     p1 is : 42	p2 is 2
     p1 is : 24	p2 is 42

Which basically sums up how the args should be used. Note that, one should not mix named args passing with unnamed positional arguments. That is bad, is not tolerated.

Every function takes variable length args, and thus - null values get's assigned to parameters which are not passed. 
People should be careful.

[Back to Contents](#contents)

### Arguments to a Script
As one can call an imported script as a function, the \_\_args\_\_ construct is supported there too.
Given we have a script :

    // something.jxl
    def my_function(arg){
        write( str( arg ) )
    }
    // Python equivalent of run as main 
    if ( #def __args__ ){
      my_function( __args__ )
    }

Now, if we want to call the script with parameters from another script :

    // as in [I]mported [N]ame[S]pace
    import 'something.jxl' as INS 
    INS:__me__(4,2)

Of course the result would be :

    njexl tmp.jxl
    42

And gladly, that is the ultimate answer.

[Back to Contents](#contents)

## Argument overwriting 

Sometimes kind of smart people acts in stupid ways.
This feature is a classic example - shooting one in the feet.
Suppose one wants to call a method - with stipulated parameters, 
but can not pass it, directly. How one passes the parameters then?

    import 'java.lang.System.out' as out
    def gte(a,b){
       a <= b  
    }
    // standard way - kind of ok 
    out:printf ( "%d < %d ? %s\n", 1, 2 , gte(1,2) )
    a = [1,2]
    // same with __args__ overwriting : cool! 
    out:printf ( "%d < %d ? %s\n", a[0], a[1] , gte(__args__ = a) )

Of course the result is this :

    1 < 2 ? true
    1 < 2 ? true

That shows you what all can be done with this.
Note that, once you overwrite the args, no other parameter can be passed at all.

[Back to Contents](#contents)

## Assignment to Variables

> This is Constitutional Right for First Class Citizens, being assigned as variables.

Methods are first class citizen, hence, one can pass a method to a variable.
Hence, this is perfectly legal :

    def z(){
       write("Yes! I am booked now!")
    }
    x = z // assignment of a function into a variable 
    x() // call that function using the variable

Thus one can have anonymous functions written w/o a name but binding to a variable :

    x = def (){ // this is an anonymous function 
        write("Yes!")
    }
    x() // call the function 

One can also assign a function to a dictionary to give it a *class* like feel as in JavaScript:

    (njexl)df = { 'sum' : _ = def(a,b){a+b} , 'sub' : _ = def(a,b){a-b} } 
    =>{sub=ScriptMethod{ name='', instance=false}, sum=ScriptMethod{ name='', instance=false}}

And thus, one can call it appropriately:

    (njexl)f = df.sum
    =>ScriptMethod{ name='', instance=false}
    (njexl)f(4,2)
    =>6

If a method belongs to a collection, i.e Set, List, or Hash, then when it gets invoked, 
through the collections accessor, it would have the *me* reference.


    def z(){
       write("Yes!")
       if ( #def me ){ 
           write("me exists and me is %s\n",me)
       }
    }
    d = { 'fp' : z }
    d.fp()

Which produces the output :

    $ njexl tmp.jxl 
    Yes!
    {fp=ScriptMethod{ name='z', instance=false}}
    nogamacpro:njexl noga$ njexl tmp.jxl 
    Yes!
    me exists and me is {fp=ScriptMethod{ name='z', instance=false}}

[Back to Contents](#contents)

### Closures 

This is for the Jargon Guys, only. 
If you need to use closures, then you are probably not doing the right thing, 
in as much as - none would understand your code now.

Closures are defined as This way in [WikiPedia](https://en.wikipedia.org/wiki/Closure_(computer_programming))
As the first class citizen - this is easy for us here:

    // this shows the nested function 
    def func(a){
        // here it is :
        r = def(b){
           write("%s + %s ==> %s\n", a,b,a+b)
        }
        return r // returning a function 
    }
    // get the partial function returned 
    x = func(4)
    // now, call the partial function 
    x(2)
    //finally, the answer to life, universe and everything :
    x = func("4")
    x("2")

This generates what it suppsoed to do : 

    4 + 2 ==> 6
    4 + 2 ==> 42

[Back to Contents](#contents)

## Lambdas 

From the theory perspective, lambdas are defined in [Lambda Calculus](https://en.wikipedia.org/wiki/Lambda_calculus). As usual, the jargon guys made a fuss out of it, but as of now, 
we are using lambdas all along, for example :

     list{ $** 2 }(1,2,3,4)

The { $**2 } is a lambda. The parameter is implicit albeit, because it is meaningful that way.
However, sometimes we need to pass named parameters.
Suppose I want to create a 
[function composition](https://en.wikipedia.org/wiki/Function_composition), first step 
would be to apply it to a single function :

     def  apply ( param , a_function ){
         a_function(param)
     }

So, suppose we want to now apply aribitrary function :

     apply( 10, def(a){ a** 2 }  )

And now, we just created a lambda! The result of such an application would make
apply function returing 100.

[Back to Contents](#contents)

Now, we can move on to :

### Function Composition 

    def compose (param,  a_function , b_function ){
        // first apply a to param, then apply b to the result 
        // b of a 
        b_function ( a_function( param ) )
     }

Now the application :

     compose( 10, def(a){ a ** 2 } ,  def(b){ b - 58 } )

As usual, the result would be 42!

Now, composition can be taken to extreme ... this is possible due to the arbitrary 
length argument passing :

    def compose (){
        p = __args__  ; n = size( p ) 
        n >= 2 or bye('min 2 args are required!') 
        i = p[0] ; p = p[[1:n]]
        lfold{  $(_$_) }( p, i) 
     }

Formally this is a potential infinite function composition!
Thus, this call :

    // note the nameless-ness :)
    r = compose(6, def (a){ a** 2} , def (b){ b + 6 } )
    write(r)

generates, the [answer to life, universe, and everything](https://www.google.co.in/search?client=safari&rls=en&q=the+answer+to+life+the+universe+and+everything&ie=UTF-8&oe=UTF-8&gfe_rd=cr&ei=EBhCVrPlAvPI8AeFm4GADA), as expected.

[Back to Contents](#contents)

### Operators on Functions 

Functions support two operators :

*  The '\*' operator for function composition 
*  The  '\*\*' operator for exponentiation ( fixed point iteration )

Let's have a demonstration :

    // predecessor function 
    def p(){ int( __args__[0] ) - 1 }
    // successor function 
    def s(){  int( __args__[0] ) + 1 }
    // now use them 
    write( list( s(0) , s(1) , s(2) ) )
    write( list( p(3) , p(2) , p(1) ) )
    I = s * p // identity function !
    write( list( I(0) , I(1) , I(2) ) )
    // power?
    add_ten = s**10 
    write( add_ten(0))
    write( add_ten(10))

The result is :

    [1, 2, 3]
    [2, 1, 0]
    [0, 1, 2]
    10
    20

[Back to Contents](#contents)

### A nice way to Recursion  

A fancy way fix recursion is with this operators, for example, 
here is factorial :

    def f(){ n = __args__[0] + 1 ; r = n * int ( __args__[1] ) ; [ n, r ] }
    n = 5 // factorial 5 ?
    factorial_n = f**n 
    r = factorial_n(0,1)
    write ( r[1] ) // prints 120 

And here is fibonacci sequence :

    def f( ) { p = __args__ ; n = p[0] + p[1] ; [ p[1] , n ] }
    n = 5 
    fibonacci_n = f**n 
    r = fibonacci_n(0,1)
    write ( r[1] ) // prints 8 

[Back to Contents](#contents)

## Eventing 

Events are what triggers a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine) to change states. In functional paradigm, a state is encompassed by a function - or state change is encompassed by a function thereof.

Thus, an event is triggered when a function is executed.
Therefore, it is same to hook up before and after a function and call it eventing, 
which is essentially what it all means.


### Implementation 

In nJexl, functions are first class objects, hence, they carry their own execution information in bags, thus, one can add hooks before and after :

    def add(a,b){ write( a + b )}
    before = def(){ write("Before! %s \n" , __args__ ) }
    after = def(){ write("After! %s \n" , __args__ ) }
    add.before.add( before )
    add.after.add( after )
    add(3,4)
    add("Hi " , "Hello!", "Extra param - ignored")    


The defined *add* method ( or rather any method ) has bags of *before* hooks, 
and *after* hooks. Adding a method to the set of methods deemed to execute before/after
ensures a proper state based handling of behaviour :

    Before! __before__ | ScriptMethod{ name='add', instance=false} | @[3, 4] 
    7
    After! __after__ | ScriptMethod{ name='add', instance=false} | @[3, 4] 
    nogamacpro:njexl noga$ njexl tmp.jxl 
    Before! __before__ | ScriptMethod{ name='add', instance=false} | @[3, 4] 
    7
    After! __after__ | ScriptMethod{ name='add', instance=false} | @[3, 4] 
    Before! __before__ | ScriptMethod{ name='add', instance=false} | @[Hi , Hello!, Extra param - ignored] 
    Hi Hello!
    After! __after__ | ScriptMethod{ name='add', instance=false} | @[Hi , Hello!, Extra param - ignored] 

Other sort of eventing will be discussed when we would be discussing Objects.  

This concludes the generic method level knowledge base.

[Back to Contents](#contents)