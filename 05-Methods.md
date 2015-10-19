# Methods

No matter how much you want to avoid them, methods are needed.
They are first class entities in here, in the realm of nJexl.
They are of course, to be passed by names, and are defined as such.

## Defining Methods 

"def" is used to define methods ( as well as class, which we would talk later ).

    import 'java.lang.System.out' as out
     
    def some_func(s){
        out:println(s)
        my:void_func() // my: ensures we always call this scripts void_func.
        true  ## makes it perl like
    }

    def void_func(){
        return false // standard return 
    }

    some_func("Hello, World!")


Note that curious "my" syntax. It basically tells nJexl that the method call should point to the current modules 
( read script files ) void_func() -- not any other random script files. As one can import modules from other locations, this becomes of very importance.
The following command can be used to run the script file : 

    $ java -jar njexl-0.1-SNAPSHOT.one-jar.jar ../samples/dummy.jexl 
    Hello, World!

It shows how it all finally works out. 
The above sample also shows - how a method can call another method.

## Global and Local Scope and Variables

All global variables are read-only locals. 
A variable declared as "var xxx" is a global variable where from local scope one can write.
While a simple assignment is local.

    import 'java.lang.System.out' as out
    var x = 0 
    def foo(){
       x = x + 1
    }
    foo()
    foo()
    out:println(x)

This would print 2. However, if you fail to put var, then :

    import 'java.lang.System.out' as out
    x = 0 
    def foo(){
       x = x + 1
    }
    foo()
    foo()
    out:println(x)

would print 0. The global variable comes in local, but does not get write back to global.


## Recursion is Divine 

As it is well said - to iterate is human, to recurse is divine, we take heart from it.
We showcase the factorial program : 

    import 'java.lang.System.out' as out
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
            n = n - 1  
        }
        return x
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


    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/factorial_sample.jexl 
    @[usage : ../samples/factorial_sample.jexl  <number> [-r : recursively]]

This is a pretty simplistic command line usage, and let's try it : 


    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/factorial_sample.jexl 5
    120
    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/factorial_sample.jexl 10
    3628800
    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/factorial_sample.jexl 100
    933262154439441526816992388562667004907159682643816214685929638952175999 // next line
    932299156089414639761565182862536979208272237582511852109168640000000000 // next line 
    00000000000000


Hope that it is right? It is. In fact the automatic widening of number is something that is a feature of nJexl. One need not worry about *bounds*, after all, mind is [limitless!](http://www.imdb.com/title/tt1219289/)

Now, it is high time checking the recursive code: 


    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/factorial_sample.jexl 410 -r
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


Good enough, I suppose. Beware of recursion in nJexl. As it is divine, it is not well implemented, and it would eventually fail you. It does not handle a depth of 420+ well. While I would like to work on optimising it, premature optimisation is the root of many evils, so ....


## Functional Support

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

    $ java -jar njexl-0.1-SNAPSHOT.jar ../samples/functional_sample.jexl 
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


## The Args constructs 


### The Anonymous Argument 

     __args__

is the construct. The purpose of it, is to be used to anonymously use the whole argument list ( actually Object array). Why they are useful? Mostly when I do not care about the arguments, but needs to process them, and names are last on my mind. A classic example is the one given at the start of the chapter : 


	// This does not take any param, so, a call to this 
	def my_max_function(){
	    // Use a function that use functional : e.g. list 
	    o = sqlmath(__args__)
	    return o[1]
	}


Here, the sqlmath() actually takes functional as input. and I want this sqlmath to process the arguments, rather than me. Hence, when I call : 

       x = my_max_function{ int($) }( '1', 2, '3', 4, '5')

The arguments are passed as is to the sqlmath, who handles all the anonymous function thing, and returns me the result.


### Default Arguments : Variable No of Args

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

## Argument overwriting 

Sometimes kind of smart people acts in stupid ways.
This feature is a classic example - shooting one in the feet.
Suppose one wants to call a method - with stipulated parameters, 
but can not pass call it, directly, how one passes the parameter then?

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

## First Class Citizen : Assignment to Variables

Methods are first class citizen, hence, one can assing a method to a variable.
Hence, this is perfectly legal :

    def z(){
       out:println("Yes!")
    }
    x = z // assignment of a function into a variable 
    x() // call that function using the variable

Thus one can have anonymous functions written as such :

    x = def (){ // this is an anonymous function 
        out:println("Yes!")
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


    import 'java.lang.System.out' as out

    def z(){
       out:println("Yes!")
       if ( #def me ){ 
           out:printf("me exists and me is %s\n",me)
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


### For the Jargon Guys : Closures in nJexl 

Closures are defined as This way in [WikiPedia](https://en.wikipedia.org/wiki/Closure_(computer_programming))
As the first class citizen - this is easy for us here:

    import 'java.lang.System.out' as out
    // this shows the nested function 
    def func(a){
        // here it is :
        r = def(b){
           out:printf("%s + %s ==> %s\n", a,b,a+b)
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

## Eventing 

Events are what triggers a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine) to change states. In functional paradigm, a state is encompassed by a function - or state change is encompassed by a function thereof.

Thus, an event is triggered when a function is executed.
Therefore, it is same to hook up before and after a function and call it eventing, 
which is essentially what it all means.


### Implementation 

In nJexl, functions are first class objects, hence, they carry their own execution information in bags, thus, one can add hooks before and after :

    import 'java.lang.System.out' as out

    def add(a,b){ out:println( a + b )}
    before = def(){ out:printf("Before! %s \n" , __args__ ) }
    after = def(){ out:printf("After! %s \n" , __args__ ) }
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

