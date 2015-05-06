# Language Features
We start with the proverbial - "Hello World".
This is easy :- 


      (njexl)import 'java.lang.System.out' as out
      =>class java.lang.System
      (njexl)out:println('Hello,World!')
      Hello,World!
      =>null



## Imports
This also tells you something interesting about the design of the language.
You can import POJO's - not only the O's but the static F's too - that is, we are essentially importing the 'out' field ( PrintStream ) of the 'java.lang.System' class. You can actually import System class too.
You can of course import another jexl script - and of course you have to only specify where the jexl file is,
i.e. full path. If you do not give full path ( .jexl can be omitted ) - the runtime treats it as relative to current directory. That is salient! These sort of design decision avoids the use of *STANDARD* libraries locations.

No more controlled location by sys admins. You are as free as one can get. 
Never the less that is one salient feature of the language.

### Calling Imported Functions  

Generally people love ".". I do too. They are kind of match all - catch all. But then one has to find - is that an imported module or an object or what? That is why all method calls of a module is with strictly ":".
That lets people know - yes, that is a function I am calling from an IMPORTED MODULE, it may inside be an object - but that does not matter. For example, wanna know my user name?

     (njexl)sys:getProperty("user.name")
     =>noga
     (njexl)sys:getProperty("user.dir")
     =>/Codes/Java/njexl/target

## Comments 
They are line : "//" or "##" or block "/*  */".
We would support "@" just like javadoc later.

## Variables
Everything and anything that does not match a strict keyword is a variable.
The strict keywords are :- 
if, else, for, while, @ , ":" , def : and we are done.

     (njexl)x = "i am a var"
      =>i am a var
     (njexl)x
     =>i am a var

### Names

Can be anything, but only starts with [@$a-z_A-Z]. That is important.
The specific variable types starting "$" and "@" gets used in implicit loop operations - which we would talk later.
You should not use a $ type variable in a loop.

### Scope

Variables are global in the script - unless they are declared within the method definitions.
That is a very crucial concept. No specific things are needed to call global variables inside a method like python.
Just use it.

## Statements 

Statements are separated by - well you guessed it : new lines. And ";". If you want to put lots of stuff in a single line ( saving space is still a premium ) : use ";". If not, use new line. Makes it work, makes it readable.
Now what about a statement that is too large to fit in the same line ? In that case use "..." to end that line. Yes, ellipsis. And then continue the next line as if nothing happened, that would concatenate the lines.

## Blocks
Like eternal rule "{ inside }" is a block. Space and tabs are bad idea to indent anything.
That would destroy compression if need be. I do not want it. Note that Blocks does not - and I repeat - does not nest variables - does not take them in or out of scope. That is currently a flaw in the design - which we may or may to fix later. Thus, { i = 0  {  j = 1 }  out:println(i+j) } works.

## Loops
goto is not there. I am sorry for that. I love goto, but EwDijkstra did not. I do not know why.
In any case - fear not, we are Turing Complete by introducing the very new : 
while(condition){ statements } and for ( var : iterable ) { statements }.
Totally works.
![for loop with range ](http://s17.postimg.org/gbchlvycv/Screen_Shot_2015_03_30_at_11_17_19_am.png)

### range

That brings you to range. Range is good. You should use range. It is optimal - and thus at least 2 times faster than the python equivalent and may be more. No, I was joking - it is fast because JVM is eons faster than PVM.
I took gazillion time faster JVM and make it very slow - so now nJexl is *only* 2 times faster than that of any Python Script. The syntax of range is  : 
 * range(end)
 * range(end, start)
 * range(end, start, step)
and it returns long. Thus it would be wise not to use this over arrays without cross checking the size. 
You see, due to design of Java - the maximum size of any container is MAX_INT.

## Literals
These are the data values. 
### Numbers 
Anything between [0-9] and then +,- appended. No hex, please. I do not like them. May be later.
### Strings
Anything within '...' and "....". As usual "\" lets you escape.

### Arrays

Anything that is [ ]. Err, [ 'a' , 10, 'b'] is an array. They are actually Object[] type.
Everything is object in here. They are not modifiable. You can also generate an array by : array(1,2,3) etc. 

     (njexl)arr = [ "I" , "am", "an" , "Array" ]
     =>@[I, am, an, Array]
     (njexl)arr = array{ $ + ':' }(arr)
     =>@[I:, am:, an:, Array:]
    
What is that {} block? We will talk about in the section of Anonymous functions.

### Lists

Fear not, you can cast an array to list straight by : list(). That gives you a modifiable list.
     
      (njexl)l = list()
     =>[]
     (njexl)l = l + 'hi'
     =>[hi]
     (njexl)l
     =>[hi]
     (njexl)l.add('bye')
     =>true
     (njexl)l
     =>[hi, bye]


### Dictionaries and Hashes
{  key : value } is a dictionary. {:} is an empty dictionary.
They are always modifiable. You can create a dictionary by dict() and pass two lists, first one is key list, 
second one is value list. It would marry them up.


    (njexl){:}
    =>{}
    (njexl)d = {:}
    =>{}
    (njexl)d[0]=0
    =>0
    (njexl)d
    =>{0=0}


### Size and Empty

These are special functions.
size(C) gives you size of a container. Let it be array, list or hash.
Well, if you know it is an array - then ".length" also works.
So does C.size()
empty(C) tells you iff  C is null or empty.
     
     (njexl)d = {:}
     =>{}
     (njexl)size(d)
     =>0

### Accessor to Array, List, Hash, Object

C[x].
It is that simple. For an Array/List , it starts with C[0] and goes until you know when : C[size(C)-1].
For a hash, that becomes C[key]. 
Now, well, as we all know - Objects are nothing but Hashes ( from theory of computing ) - fields of an object can be used the very same way.
O[F] gives you the value of the field named 'F'.
Don't worry. O.F also works as expected.

## Return

return <expression>.
You must return something. No void, please. 
In fact, if you do not even say it - the outcome of the last executed statement is taken as return value.

## Methods 

defining methods is easy.
![Methods](http://s17.postimg.org/fzzmmv7an/Screen_Shot_2015_03_30_at_11_37_35_am.png)

It also introduces you to "true" "false" two constants. They are Boolean type.
Also note the interesting "my:". That is important. In a complete dynamic environment - if you do not specify the "my:" it might call another method from another module with the same name. So that does not happen - we have my.
No cross calling of functions.

## Operators
Generally the operations like "=" , "==" , "!" ,  "<" , ">" , "<=" , "=>" , "!=" have same meaning.
But they also have names like       "eq", "not",  "lt", "gt", "le", "ge", "ne" 
So are the bitwise operations like "|" , "&" , "^", "~".

Unlike it's parent Java, "==" means equals(). That is it. In fact it goes further.
The automatic intelligent analysis ensures that "1" == 1 works in case of integer comparisons.
That is very good for testing related activities.

The logical operators are "||" : "or"  and "&&" : "and" and "!" : "not".
The english and the symbolic both works.

## Set & List Operations.
Almost all the operators are overloaded to handle some tricky stuff - "+" can add lists.
In the same way "-" can do a set minus. It also can do a multi set minus popularly known as list minus.
The special operator "@" defines "in". That is : {  1 @ [ 1, 2, 3]  } would give you true. 
While {  0 @ [ 1, 2, 3]  } would give you false. At the same time : {  [2,1]  @ [ 1, 2, 3]  } would give you true.
Native support of set operations you see.
![List and Set Operations](http://s14.postimg.org/h3xznkagx/Screen_Shot_2015_03_30_at_12_52_53_pm.png) 

## Anonymous Functions and Lambda's.
Do not worry. We are not geeks. We are way short - and hence we actually use it in a way people understand what it is. Inside a for loop one generally runs a condition and does something extra. It would be good if the for loop becomes implicit and then one can only write the condition and the extra thing.

That thing is called anonymous function.
See line no 16. We are filtering  everything that is (>= 3) within the list specified.
For the set operations we are using int() of every value as the key.
 

## List operations
Various list operations are demonstrated - with some interesting power operations.
![List Operations](http://s29.postimg.org/ym437tton/Screen_Shot_2015_03_30_at_3_40_17_pm.png)
Note that the power operator "**" works on String as well as lists.
To reverse a string use <string>^-1. That should be awesome.
Now just like python : <string>*n catenated. Here,  <string>**n catenated the string n times.

## Why NO Exception Handling ?

I toyed with the idea, and then found that : 
[Why Exception Handling is Bad?](http://stackoverflow.com/questions/1736146/why-is-exception-handling-bad)
GO does not have exception handling.
And I believe it is OK not to have exceptions.

Code for everything, never ever eat up.
In particular - the multi part arguments and the return story - should ensure that there is really no exceptional thing that happens. After all, trying to build a fool-proof solution is never full-proof.

In any case there is this try function which acts as wrapper in case you want to make underlying Java object calls - which may end up throwing exceptions : 

	import 'java.lang.System.out' as out
	import 'java.lang.Integer' as Integer

	i = try{
	  Integer:parseInt(__args__[1])
	}()

	out:printf("%s\n",i)
 
When we run it using normal stuff : 

        $ njexl sample.jexl 10
        10

  
But when we run it with invalid argument : 

         $ njexl sample.jexl ccc
         java.lang.NumberFormatException: For input string: "ccc"

Thus, one may wrap indecent function calls into try{}() function block.
  