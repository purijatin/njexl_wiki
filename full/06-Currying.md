# Von Neumann Architecture 

Von Neumann said *data is same as executable code*. 
Read more on : [Von Neumann Architecture](http://en.wikipedia.org/wiki/Von_Neumann_architecture)

Thus, one can execute arbitrary string, and call it code, if one may.
That brings in how functions are actually executed, or rather what are functions.

# Currying 

The theory really behind JVN-A is called [currying](http://en.wikipedia.org/wiki/Currying)

## Why on Earth?

Because it is cool. No, am just joking around. This is the most important thing that happened in the theory of computation - and thus in the whole darn history of software. Currying. Please understand it.

The tenet, as usual is - code is bad. Branching is part of code and thus, squarely bad.
Test automation is testing those branching of code hence cubely bad. Finally, test automation as tons of branches that makes it a ripe harvest of people sucking out money. That is x^4 bad. 'BAD'**4 if you are using nJexl.


I do not like it. I believe, if/else/branches and code are like wine, good results come only when used in moderation. So - enough said - what precisely is the need of it? Suppose you are to test, and mind you test, if the calculator application is adding stuff properly or not. Next test would be if they are subtracting stuff or not. Next would be if they multiply properly or not....and so on and so forth.

Basically we need to test something of the form we call a "binary operator" is working "fine" or not:

    operator_1 <operator> operator_2 

That is a general binary operator, yes. Now, how to test it? The test code would be, invariably messy ( not Mesi, who is God's parting Gift to Humanity, but just Messy ) :

    if ( operation == '+' ) {
        do_plus_check();
    } else if ( operation == '-' ) {
        do_minus_check();
    }
    // some more stupidity ...

And someone comes in, automates it in 2 days - and cheerleaders ( read : managers)  promote him/her.
Makes people like me angry.A bit too much, stone aged stuff.
Currying comes to help for those people who distaste these behaviors.

Basically Curry said : "All binary operators are of same category". 
By that what it means, if someone can abstract the operation out - and replace the operation with the symbol - and then someone can actually execute that resultant string as code ( remember JVMA?) you are done.

### Simplistic Example 

Take a problem of comparing two doubles to a fixed decimal place.
Thus: 

    (njexl)x=1.01125
    =>1.01125
    (njexl)y=1.0113
    =>1.0113
    (njexl)x == y
    =>false

And we have an issue. How about we need to compare these two doubles with till 4th digit of precision (rounding) ?
How it would work? Well, we can use String format : 

    (njexl)str:format("%.4f",x)
    =>1.0113
    (njexl)str:format("%.4f",y)
    =>1.0113
But wait, the idea of precision, that is ".4" should it not be a parameter? Thus, one needs to pass the precision
along, something like this : 

    (njexl)c_string = "%%.%df"  ## This is the original one 
    =>%%.%df
    (njexl)p_string = str:format(c_string,4) ## apply precision, makes the string into a function 
    =>%.4f
    (njexl)str:format(p_string,y)  ## apply a number, makes the function evaluate into a proper value
    =>1.0113  # and now we have the result!

All we really did, are bloated string substitution, and in the end, that produced what we need.
Thus in a single line, we have : 
   
    (njexl)str:format(str:format(c_string,4),x)
    =>1.0113
    (njexl)str:format(str:format(c_string,4),y)
    =>1.0113

Thus, currying boils down to replacing one parameter at a time from a string, till no parameters are left 
to be substituted, and when that happens, evaluate the final string as an expression/script.
This is a very profound idea.


### Currying Solves Operator Problem too!

So what does that all mean? That means this.
First take the string `x #{op} y`. Now, replace the nitty part called hot-curry : the #{} thing and you are done.
All you need to do is to execute it back again as a script - which nJexl neatly does. Thus...

    (njexl)c_string = `#{a} #{op} #{b}`
    =>#{a} #{op} #{b}
    (njexl)a=10
    =>10
    (njexl)c_string = `#{a} #{op} #{b}`
    =>10 #{op} #{b}
    (njexl)op='+'
    =>+
    (njexl)c_string = `#{a} #{op} #{b}`
    =>10 + #{b}
    (njexl)b=10
    =>10
    (njexl)c_string = `#{a} #{op} #{b}`
    =>20
 
A much cleaner way of doing this would be self-substitution : 

    (njexl)c_string = `#{c} #{o} #{d}`
    =>#{c} #{o} #{d}
    (njexl)c=20
    =>20
    (njexl)c_string=`#{c_string}`
    =>20 #{o} #{d}
    (njexl)o='*'
    =>*
    (njexl)c_string=`#{c_string}`
    =>20 * #{d}
    (njexl)d=1.2
    =>1.2
    (njexl)c_string=`#{c_string}`
    =>24.0


That should show it. Now you see, no more thousands of lines of stupid mind boggling automation script. 
We keep it simple - because we know we are stupid. Some more demonstration follows : 

     (njexl)op = '**'
     =>**
     (njexl)`x #{op} y`
     =>8.0
     (njexl)op = '=='
     =>==
     (njexl)`x #{op} y`
     =>false
     (njexl)op = '!='
     =>!=
     (njexl)`x #{op} y`
     =>true

That is the power - of 1960 computer science. No, not *computer* engineering, and heavens no, no software engineering. In any case, the great JVN knew it, and that is why there is a JVN architecture.


## Why Back-Tick?

I could have implemented the currying as part of the language - straight, not as a string processing.
But that would actually mean - processing the file 2 times, one for the standard notation, another for the operations overload. After all, who told you that the forms are limited?

You can have : 

    a #{op} b 
    func( a #{op} b )

and now, imagine that the #{op} can turn itself into an operator or a comma! What then?
Thus, it is theoretically impossible to 'guess' context of the operator. And thus, a better implementation is double reading by default, *ONLY* when one is forced to ask : dude, there is my back tick!
Note that, back tick returns a string, if nothing found or matched.

    (njexl)y=10
    =>10
    (njexl)x='hi'
    =>hi
    (njexl)`x.#{method}(y)`
    =>x.#{method}(y)
    (njexl)method='equals'
    =>equals
    (njexl)`x.#{method}(y)`
    =>false

You can call it - the executable (2) string. Or, you can call it a glorified macro processor.
Does not matter. Currying is essential in computer theory - and is more than essential 
if you are trying to avoid voluminous coding. 

## Currying and Method calling

Calling methods can be accomplished using currying. 
The "equals" in previous section shows it. However, the idea can go much deep.

    import 'java.lang.System.out' as out 

    def func_taking_other_function( func ){
       `#{func}( 'hello!' )`
    }

    my:func_taking_other_function('out:println')


This works, as expected.

    $ njexl ../samples/curry_sample.jexl
    hello!

## Curry expressions as Referencs

Let's see how the reference behaviour is in nJexl.

    (njexl)x = [1,2]
    =>@[1, 2]
    (njexl)y = { 'x' : x }
    =>{x=[I@5b37e0d2}
    (njexl)x = [1,3,4]
    =>@[1, 3, 4]
    (njexl)y.x // x is not updated 
    =>@[1, 2]

Suppose you want a different behaviour, and that can be achived using Pointers/References.
What you want is this :

    (njexl)x = [1,2]
    =>@[1, 2]
    (njexl)y = { 'x' : 'x'  }
    =>{x=x}
    (njexl)`#{y.x}` // access as in currying stuff
    =>@[1, 2]
    (njexl)x = [1,3,4]
    =>@[1, 3, 4]
    (njexl)`#{y.x}` // as currying stuff, always updated 
    =>@[1, 3, 4]

So, in effect I am using a dictionary to hold *name* of a variable, instead of having 
a hard variable reference, thus, when I am dereferencing it, I would get back the value
if such a value exists! 


## Reflection 
If currying is too hard to comprehend, then reflection is relatively simpler.
To start with we first need to find the name of the methods defined 
in a script :

    import 'java.lang.System.out' as out
    // import from another script 
    import  '_/src/lang/samples/dummy.jexl' as dummy
    //call a function  
    dummy:some_func("Hello, World!")
    // now find all functions in dummy?
    methods = dummy:methods()
    // printing methods
    out:println(methods)
    // now call same method:
    m_name = 'some_func'
    method = methods[m_name]
    method("Hello, Again!")

When one executes this, the results are what is expected :

    Hello, World!
    {some_func=ScriptMethod{ name='some_func', instance=false}, void_func=ScriptMethod{ name='void_func', instance=false}}
    Hello, Again!

So, iterating over methods of a script are easy.



 