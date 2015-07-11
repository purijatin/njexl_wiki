# Original Language Features 

## Comments 
They are line : "//" or "##" or block "/*  */".
We would support "@" just like javadoc later.
Thus : 
     
      /* this is a multi
        line comment */

      // while this is a single line comment 
      ## and so is this.


## Identifiers 
   
  variables Must start with a-z, A-Z, _ or $. Can then be followed by 0-9, a-z, A-Z, _ or $. e.g.
  Valid:

      var1,_a99,$1
  
  Invalid: 

      9v,!a99,1$
  
  Variable names are case-sensitive, e.g. var1 and Var1 are different variables.
  NOTE: JEXL does not support variables with hyphens in them, e.g.
  
    commons-logging // invalid variable name (hyphenated)

is not a valid variable, but instead is treated as a subtraction of the variable logging from the variable commons!
JEXL also supports ant-style variables, 
the following is a valid variable name:

    my.dotted.var

N.B. the following keywords are reserved, and cannot be used as a variable name or property when using the dot operator: 

    or and eq ne lt gt le ge div mod not null true false new var return 

For example, the following is invalid:

    my.new.dotted.var // invalid ('new' is keyword)

In such cases, quoted identifiers or the [ ] operator can be used, for example:

    my.'new'.dotted.var
    my['new'].dotted.var


## Scripts 

A script in Jexl is made up of zero or more statements. Scripts can be read from a String, File or URL.
They can be created with named parameters which allow a later evaluation to be performed with arguments.
A script returns the last expression evaluated by default.
Using the return keyword, a script will return the expression that follows (or null).
Local variables Can be defined using the var keyword; their identifying rules are the same as contextual variables.

Basic declaration: 

    var x;

Declaration with assignment: 

    var theAnswer = 42;

Invalid declaration: 
    
    var x.y;

Their scope is the entire script scope and they take precedence in resolution over contextual variables. 
When scripts are created with named parameters, those behave as local variables. 
Local variables can not use ant-style naming, only one identifier.

### Statements  

A statement can be the empty statement, the semicolon (;) , block, assignment or an expression. 
Statements are optionally terminated with a semicolon.

### Block 

A block is simply multiple statements inside curly braces ({, }).
Assignment  Assigns the value of a variable (my.var = 'a value') using a JexlContext as initial resolver. Both beans and ant-ish variables assignment are supported.

### Method calls  

Calls a method of an object, e.g.

    "hello world".hashCode()

will call the hashCode method of the "hello world" String.
In case of multiple arguments and overloading, Jexl will make the best effort to find the most appropriate non ambiguous method to call.

### Literals

#### Integer Literals  

1 or more digits from 0 to 9, eg 42.

#### Float Literals :

   1 or more digits from 0 to 9, followed by a decimal point and then one or more digits from 0 to 9, optionally followed by f or F, eg 42.0 or 42.0f.

#### Long Literals:

   1 or more digits from 0 to 9 suffixed with l or L , eg 42l.

#### Double Literals: 
   
   1 or more digits from 0 to 9, followed by a decimal point and then one or more digits from 0 to 9 suffixed with d or D , eg 42.0d.

#### Big Integer Literals:

  1 or more digits from 0 to 9 suffixed with b or B , eg 42B.

#### Big Decimal Literals :

  1 or more digits from 0 to 9, followed by a decimal point and then one or more digits from 0 to 9 suffixed with h or H (for Huge ala OGNL)) , eg 42.0H.

#### Natural literals : 

  octal and hex support  Natural numbers (i.e. Integer, Long, BigInteger) can also be expressed as octal or hexadecimal using the same format as Java. i.e. prefix the number with 0 for octal, and prefix with 0x or 0X for hexadecimal. For example 010 or 0x10.

#### Real literals :

 exponent support  Real numbers (i.e. Float, Double, BigDecimal) can also be expressed using standard Java exponent notation. i.e. suffix the number with e or E followed by the sign + or - followed by one or more decimal digits. For example 42.0E-1D or 42.0E+3B.

#### String literals: 

Can start and end with either ' or " delimiters, e.g.

    "Hello world"

and

    'Hello world'

are equivalent.

The escape character is \ (backslash); it only escapes the string delimiter.

#### Boolean literals:

  The literals true and false can be used, e.g.

     val1 == true

#### Null literal:  
 
  The null value is represented as in java using the literal null, e.g.

    val1 == null

#### Array literal: 

  A [ followed by one or more expressions separated by , and ending with ], e.g.

     [ 1, 2, "three" ]

This syntax creates an Object[].
JEXL will attempt to strongly type the array; if all entries are of the same class or if all entries are Number instance, the array literal will be an MyClass[] in the former case, a Number[] in the latter case.
Furthermore, if all entries in the array literal are of the same class and that class has an equivalent primitive type, the array returned will be a primitive array. e.g. [1, 2, 3] will be interpreted as int[].


#### Map literal: 

  A { followed by one or more sets of key : value pairs separated by , and ending with }, e.g.

     { "one" : 1, "two" : 2, "three" : 3, "more": "many more" }

This syntax creates a HashMap<Object,Object>.

## Functions


### empty 

  Returns true if the expression following is either:

  * null
  * An empty string
  * An array of length zero
  * A collection of size zero
  * An empty map

Sample use is :

    empty(var1)

### size  

Returns the information about the expression:

* Length of an array
* Size of a List
* Size of a Map
* Size of a Set
* Length of a string

As an example :

    size("Hello")

returns 5.

### new 

Creates a new instance using a fully-qualified class name or Class:

    new("java.lang.Double", 10)

returns 10.0.

Note that the first argument of new can be a variable or any expression evaluating as a String or Class; the rest of the arguments are used as arguments to the constructor for the class considered.
In case of multiple constructors, Jexl will make the best effort to find the most appropriate non ambiguous constructor to call.
ns:function A JexlEngine can register objects or classes used as function namespaces. This can allow expressions like:

    math:cosinus(23.0)


### General Operators

#### Boolean and :

The usual && operator can be used as well as the word and, e.g.

     cond1 and cond2
and

     cond1 && cond2

are equivalent.

#### Boolean or:

  The usual || operator can be used as well as the word or, e.g.

     cond1 or cond2

and
 
     cond1 || cond2

are equivalent.

#### Boolean not:

 The usual ! operator can be used as well as the word not, e.g.

    !cond1

and

     not cond1

are equivalent.

#### Bitwise and :

The usual & operator is used, e.g.

    33 & 4
    0010 0001 & 0000 0100 = 0.

#### Bitwise or:

  The usual | operator is used, e.g.

    33 | 4
    0010 0001 | 0000 0100 = 0010 0101 = 37


#### Bitwise xor:

 The usual ^ operator is used, e.g.

     33 ^ 4
     0010 0001 ^ 0000 0100 = 0010 0100 = 37.

#### Bitwise complement:  

 The usual ~ operator is used, e.g.

    ~33
    ~0010 0001 = 1101 1110 = -34.

#### Ternary conditional ?:  

The usual ternary conditional operator condition ? if_true : if_false operator can be used as well as the abbreviation value ?: if_false which returns the value if its evaluation is defined, non-null and non-false, e.g.


     val1 ? val1 : val2

and

     val1 ?: val2 

are equivalent.

NOTE: The condition will evaluate to false when it refers to an undefined variable or null for all JexlEngine flag combinations. 
This allows explicit syntactic leniency and treats the condition 'if undefined or null or false' the same way in all cases.

#### Equality:

  The usual == operator can be used as well as the abbreviation eq. For example

     val1 == val2

and

     val1 eq val2

are equivalent.


for null literal, is only ever equal to null, that is if you compare null to any non-null value, the result is false.
Equality uses the java equals method.


#### Inequality:

  The usual != operator can be used as well as the abbreviation ne. For example

    val1 != val2

and

    val1 ne val2

are equivalent.


#### Less Than:

 The usual < operator can be used as well as the abbreviation lt. For example

     val1 < val2

and

     val1 lt val2

are equivalent.

#### Less Than Or Equal To: 

  The usual <= operator can be used as well as the abbreviation le. For example

     val1 <= val2
and

     val1 le val2

are equivalent.

#### Greater Than:

  The usual > operator can be used as well as the abbreviation gt. For example

    val1 > val2

and

    val1 gt val2

are equivalent.

#### Greater Than Or Equal To :

 The usual >= operator can be used as well as the abbreviation ge. For example

    val1 >= val2

and

     val1 ge val2

are equivalent.


#### (item) In  or Match=~ :

   The syntactically Perl inspired =~ operator can be used to check that a string matches a regular expression (expressed either a Java String or a java.util.regex.Pattern). For example "abcdef" =~ "abc.* returns true. It also checks whether any collection, set or map (on keys) contains a value or not; in that case, it behaves as an "in" operator. Note that it also applies to arrays as well as "duck-typed" collection, i.e classes exposing a "contains" method. 

      "a" =~ ["a","b","c","d","e",f"] // returns true.


#### (item) Not-In or Not-Match!~ :

The syntactically Perl inspired !~ operator can be used to check that a string does not match a regular expression (expressed either a Java String or a java.util.regex.Pattern). For example "abcdef" !~ "abc.* returns false. It also checks whether any collection, set or map (on keys) does not contain a value; in that case, it behaves as "not in" operator. Note that it also applies to arrays as well as "duck-typed" collection, ie classes exposing a "contains" method. 

     "a" !~ ["a","b","c","d","e",f"] //returns true.

#### Addition:  

  The usual + operator is used. For example

     val1 + val2

#### Subtraction:
  
  The usual - operator is used. For example

    val1 - val2

#### Multiplication:

  The usual * operator is used. For example

    val1 * val2

#### Division:  

   The usual / operator is used, or one can use the div operator. For example

     val1 / val2

or

     val1 div val2

#### Modulus (or remainder):

  The % operator is used. An alternative is the mod operator. For example

     5 mod 2

gives 1 and is equivalent to

     5 % 2

#### Negation:  
 
 The unary - operator is used. For example

     -12

#### Array access:  

  Array elements may be accessed using either square brackets or a dotted numeral, e.g.

     arr1[0]

and

     arr1.0

are equivalent!

#### HashMap access:

  Map elements are accessed using square brackets, e.g.

      map[0]; map['name']; map[var];

Note that

    map['7']

and
     map[7]

refer to different elements.
Map elements with a numeric key may also be accessed using a dotted numeral, e.g.

    map[0]

and

    map.0

are equivalent.

###Conditionals: 

#### if :  
 
   Classic, if/else statement, e.g.

    if ((x * 2) == 5) {
        y = 1;
    } else {
        y = 2;
    }


#### for:  
 
 Loop through items of an Array, Collection, Map, Iterator or Enumeration, e.g.

    for(item : list) {
        x = x + item; 
    }

Where item and list are variables.

#### while: 

Loop until a condition is satisfied, e.g.

    while (x lt 10) {
        x = x + 2; 
    }


# New Jexl Language Features
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




### Scope of Variables

Variables are global in the script - unless they are declared within the method definitions.
That is a very crucial concept. No specific things are needed to call global variables inside a method like python.
Just use it. To check if a variable *var_name* is defined within scope or not, use #def var_name :

    (njexl)#def x
    =>false
    (njexl)x = 10
    =>10
    (njexl)#def x
    =>true

The specific variable types starting "$" and "@" gets used in implicit loop operations - which we would talk later.
You should not use a $ type variable in a loop.


## Statements 

Statements are separated by - well you guessed it : new lines, or ";". If you want to put lots of stuff in a single line ( saving space is still a premium ) : use ";". If not, use new line. Makes it work, makes it readable.
Now what about a statement that is too large to fit in the same line ? In that case use "\" to end that line. 
Yes, escaping new line. And then continue the next line as if nothing happened, that would concatenate the lines.
I understand that is a bit too much, so for normal operation like concatenating a string or and or or, it is taken care of already, so none has to do a "+ \".
Thus, 
  
    import  'java.lang.System.out' as out

    s = true || 
         false   
    out:println(s)

    s = true and  
         false   
    out:println(s)


    s = " "  + 
       "aaa aa "  + 
       "xxxx" + 
       "z"
    out:println(s)

    s = 10 -   
         2
    out:println(s)

works as expected.

Special consideration is also being given to comma : "," so that : 

    out:printf("%s,%s,%s\n", 1 ,  // note the new line after ","
     2, 3) // works 

works as expected too!

## Blocks
Like eternal rule "{ inside }" is a block. Space and tabs are bad idea to indent anything.
That would destroy compression if need be. I do not want it. Note that Blocks does not - and I repeat - does not nest variables - does not take them in or out of scope. That is currently a flaw in the design - which we may or may to fix later. Thus, 

    (njexl){ i = 0 ; {  j = 1 }  out:println(i+j) } // works.
    1
    =>null

as one would have expected.

## Loops
goto is not there. I am sorry for that. I love goto, but EwDijkstra did not. I do not know why.
In any case - fear not, we are Turing Complete by introducing the very new : 
while(condition){ statements } and for ( var : iterable ) { statements }.
Totally works.
       
    import 'java.lang.System.out' as out

    my_arr = [1,2,3,4]
    for ( i : my_arr ){
        out:println(i)
    }


### Range

That brings you to range. Range is good. You should use range. It is optimal - and thus at least 2 times faster than the python equivalent and may be more. No, I was joking - it is fast because JVM is eons faster than PVM.
I took gazillion time faster JVM and make it very slow - so now nJexl is *only* 2 times faster than that of any Python Script. The syntax of range is  : 
 * range(end)
 * range(end, start)
 * range(end, start, step)
and it returns long. Thus it would be wise not to use this over arrays without cross checking the size. 
You see, due to design of Java - the maximum size of any container is MAX_INT.


### Strings
Anything within '...' and "....". As usual "\" lets you escape. There is another specific one, called curried Literals,
which are delimited by ` and `. The back-tick operator, borrowed from Perl and Python.

### Arrays

Anything that is [ ]. Err, [ 'a' , 10, 'b'] is an array. They are actually Object[] type.
Everything is object in here. They are not modifiable. You can also generate an array by : array(1,2,3) etc. 

    (njexl)arr = [ "I" , "am", "an" , "Array" ]
    =>@[I, am, an, Array]
    njexl)arr = array{ $ + ':' }(arr)
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


## Return

return <expression>.
You must return something. No void, please. 
In fact, if you do not even say it - the outcome of the last executed statement is taken as return value.

## Methods 

defining methods are easy.
  
	import 'java.lang.System.out' as out
	def some_func(s){
	    out:println(s)
	    my:void_func()
	    return true
	}
	def void_func(){
	    return false
	}
	some_func("Hello, World!")


It also introduces you to "true" "false" two constants. They are Boolean types named after John Boole.
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
The special operator "@" defines "in" it extends "=~" . That is : 

    1 @ [ 1, 2, 3]   // would give you true.
    0 @ [ 1, 2, 3]   //  would give you false. 
    [2,1]  @ [ 1, 2, 3]  // would give you true.

Native support of set operations you see. Moreover, there is no @ analog in Java. 
The closest of x @ y is y.contains(x) in some sense. 
But then clearly one needs to do null check, string type check etc.


## Anonymous Functions and Lambda's.

Do not worry. We are not geeks. We are way too practical - and hence we actually use it in a way people understand what it is. Inside any for-loop one generally runs a condition and does something extra. It would be good if the for loop becomes implicit and then one can only write the condition and the extra thing.

That thing is called anonymous function.

    // classic sql --> this way. 
    y = select{ where ( $ > 2 ) { $=$*10 } }( 1, 2, 3 ) 
       
We are filtering  everything that is (>= 2) within the list specified.
For the set operations we are using int() of every value as the key.
 

## List operations
Various list operations are demonstrated - with some interesting power operations.

Note that the power operator "**" works on String as well as lists.
To reverse a string use <string>^-1. That should be awesome.
Now just like python : <string>*n catenated. Here,  <string>**n catenated the string n times.

## Threading support
nJexl has full threading support! That is to say, one can simply create thread by calling the function :
thread() with suitable parameters which to be passed to the anonymous function block.

     t = thread{  out:println("I am a thread!") } ()

Creates a thread - which just calls the out:println() call.
More of this in the threading section.


## Why NO Exception Handling ?

I toyed with the idea, and then found that : 
[Why Exception Handling is Bad?](http://stackoverflow.com/questions/1736146/why-is-exception-handling-bad)
GO does not have exception handling.
And I believe it is OK not to have exceptions.

Code for everything, never ever eat up.
In particular - the multi part arguments and the return story - should ensure that there is really no exceptional thing that happens. After all, trying to build a fool-proof solution is never full-proof.

Just like GO, however, there is a concept of returning tuples.
We will discuss this in [Exceptional Cases and Handling Them](https://github.com/nmondal/njexl/wiki/07-Utility-Functions#truly-exceptional-cases).

  