# Language Features
We start with the proverbial - "Hello World".
This is easy :- 
![Hello, World](http://picpaste.com/8e3179b51eb8cb93a071ba754621e056.png)

## Imports
This also tells you something interesting about the design of the language.
You can import POJO's - not only the O's but the static F's too - that is, we are essentially importing the 'out' field ( PrintStream ) of the 'java.lang.System' class. You can actually import System class too.
You can of course import another jexl script - and of course you have to only specify where the jexl file is,
i.e. full path. If you do not give full path ( .jexl can be omitted ) - the runtime treats it as relative to current directory. That is salient! These sort of design decision avoids the use of *STANDARD* libraries locations.

No more controlled location by sys admins. You are as free as one can get. 
Never the less that is one salient feature of the language.

### Calling Imported Functions  
Generally people love ".". I do too. They are kind of match all - catch all. But then one has to find - is that an imported module or an object or what? That is why all method calls of a module is with strictly ":".
That lets people know - yes, that is a function I am calling from an IMPORTED MODULE, it may inside be an object - but that does not matter.

## Variables
Everything and anything that does not match a strict keyword is a variable.
The strict keywords are :- 
if, else, for, while, @ , ":" , def : and we are done.

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
while(condition){ statements } and for ( var : utterable ) { statements }.
Totally works.
![for loop with range ](http://picpaste.com/5691d43fd64bfdb908095530fe3d411d.png)

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

### Lists
Fear not, you can cast an array to list straight by : list(). That gives you a modifiable list.

### Dictionaries and Hashes
{  key : value } is a dictionary. {:} is an empty dictionary.
They are always modifiable. You can create a dictionary by dict() and pass two lists, first one is key list, 
second one is value list. It would marry them up.

### Size and Empty
These are special functions.
size(C) gives you size of a container. Let it be array, list or hash.
Well, if you know it is an array - then ".length" also works.
So does C.size()
empty(C) tells you iff  C is null or empty.

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
![Methods](http://picpaste.com/2fe40f9b6dd0bfa72197136050a2a96b.png)

It also introduces you to "true" "false" two constants. They are Boolean type.
Also note the interesting "my:". That is important. In a complete dynamic environment - if you do not specify the "my:" it might call another method from another module with the same name. So that does not happen - we have my.
No cross calling of functions.





 
  

  