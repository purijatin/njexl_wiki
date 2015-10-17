# To Iterate is Human, to Recurse is divine.

We are human, and hence, we do iteration more.
Simplistic way to look at an iteration is :  

    import 'java.lang.System.out' as out
    i = 0 
    while ( i < 3 ){
       out:println(i)
       i = i + 1
    }

This of course produce the output :  

     0
     1
     2

But this is trivially boring.
If you are using "i" , it is meaningless to do all the housekeeping by yourself, and hence ... 

    for ( i  :  [0:3] ){
        out:println(i)
    }

Does produce the same output! You think it is good to be verbose?
[Enterprisification](http://projects.haykranen.nl/java/) is for you then. 
It is kind of bad to be verbose, according to me.
We want to get things done, with minimal talk - and maximal work. 
But wait. Anything array like is iterable, anything list like is iterable. Anything set like, if they are sets created by nJexl - are iterable too. And even dictionaries are default iterable. 

    for ( i  :  [0,1,2] ){
        out:println(i)
    } 

This is fine. So is  : 

    for ( i  :  {0:'0' , 1:'1' , 2:'2' } ){
        out:println(i)
    } 

And finally this is fairly interesting : 

    for ( i  :  "Smartest might survive" ){
        out:print( str:format("%s ", i ) ) 
    } 
    out:println()

Produces the output : 

     S m a r t e s t   m i g h t   s u r v i v e 

And that basically means - it reads the individual characters of a string.
That should be awesome.

We also support the standard *for* stuff :

    for ( i = 0 ; i < 10 ; i = i + 1 ){
        out:println(i)
    } 


Now, let's take a real cool example, and start calculating factorial.
    
    import 'java.lang.System.out' as out
    n = 200 
    x = 1 
    while ( n > 1 ){
        x = x*n
        n = n - 1  
    }
    out:println(x)

And the output comes : 
    
    788657867364790503552363213932185062295135977687173263294742533244359449
    963403342920304284011984623904177212138919638830257642790242637105061926
    624952829931113462857270763317237396988943922445621451664240254033291864
    131227428294853277524242407573903240321257405579568660226031904170324062
    351700858796178922222789623703897374720000000000000000000000000000000000
    000000000000000

      
## Other Worldly Matters : Continue and Break

We do have "continue" and "break". Which works as expected : 

    import 'java.lang.System.out' as out

    // the standard one 
    out.println('first while loop')
    
    i = 0 
    while ( i < 10 ){
      i = i + 1 
      if ( i % 3 == 0 ){ continue }
      out.println(i)
    }

    out.println('Now for loop')
    
    for ( i : [1:11]){
       out.println(i)
       if ( i % 4 == 0 ){ break }
    }

Which generates the output : 

    first while loop
    1
    2
    4
    5
    7
    8
    10
    Now for loop
    1
    2
    3
    4

Which justifies their existence.

### Extension of Break and Continue 
Generally people use "break" and "continue" with the following pattern :

    if ( condition ){ break }
    if ( condition ){ continue }

Thus, lot's of code can be avoided if we use the extension of break and continue :

    break ( condition ){ statements ...  }
    continue ( condition ){ statements ...   }

It should be read as :
 > break|continue when condition is true, *only* after executing the statements ... 


Hence, the loop can be well written as : 

    for ( i : [1:11]){
       out.println(i)
       break ( i % 4 == 0 )
    }

Which would work, as well as :

    i = 0 
    while ( i < 10 ){
      i = i + 1 
      continue ( i % 3 == 0 ) 
      out.println(i)
    }

The implicit word is  break *when* condition with expression value.
It is not easy to understand when we need to return something on break, 
but anonymous function calls are a perfect example :

    (njexl)list{ break($ > 10){ $ } ; $ }([1,2,3,10,11,13,19])
    =>[1, 2, 3, 10, 11]

Same with continue :

    (njexl)list{ continue($ < 10) ;  $  }([1,2,3,10,11,13,19])
    =>[10, 11, 13, 19]

And thus, it is more generic that one thinks.
Finally, the best possible example is that of join :-
Find two items from a list such that they add up to a number n :

    (njexl)join{  break( $[0] + $[1] == 4 )  }([ 0,1,2,3],[0,2,3] )
    =>[[1, 3]]

While the return result from the statements from *break* gets added 
to the collection being generated, for *continue* that does not happen.
Hence, one needs to be careful with that.

    import 'java.lang.System.out' as out

    s = '11,12,31,34,78,90'
    tmp = ''
    l = select{ 
        continue ( $ != ',' ){  tmp = tmp + $ } 
        $ = int(tmp) ; tmp = '' ; true  }(s.toCharArray() )
    l = l + int(tmp) 
    out:println(l)

This would produce :
    
    [11, 12, 31, 34, 78, 90]


### The Infamous GOTO

nJexl supports GOTO : the [harmful one](http://david.tribble.com/text/goto.html). 
The syntax is simple :

     goto  #label_id  [condition] 
     // many random code lines 
     #label_id
     // here is some useful stuff 
 
The condition is optional, that is if one eliminates it, then, true is assumed.
The following example demonstrates it :

    import 'java.lang.System.out' as out

    goto #label  
    out:println("xxxx")

    #label
    out:println("yyy")

This prints :

    >yyy

The following rules are applied for the goto statements:

* The label must be in the main script, not in any other blocks ( can not be used in functions body ) 
* Labels are identifiers
* goto can, then, be however called from anywhere to a global script label
* You may use same label as variable identifier, at your own risk
* You should never use it, unless you are automatically generating code

And that should sum it up!


# Avoiding Iteration, Using Predicate Logic 

General idea can be found here : [Predicate Logic](http://en.wikipedia.org/wiki/First-order_logic)
But in here we would discuss how the strategy of not using iteration, 
helps in formulating business and tremendously reduces formulation of test cases. 

We will start with a simple idea of a linear search, to find out, if an element is present 
in a list which matches a certain criterion. Thus:

## There exist at least an element : [EXIST e in L : P(e) is TRUE]

This is the form : 

![Exists Form](http://latex.codecogs.com/gif.latex?%5Cfn_phv%20%5Cexists%20x%20%5Cin%20L%20%5C%3B%20%3B%5C%3B%20P%28x%29%20%3A%20TRUE)

A general idea would be finding elements. That is the work of *select* function.
Does any element exist in the list which is 1? 

    (njexl)s = list([1,2,3,3,4,5])
    =>[1, 2, 3, 3, 4, 5]
    (njexl)s.contains(1)
    =>true


At the same time  : 
      
    (njexl)s = list([1,2,3,3,4,5])
    =>[1, 2, 3, 3, 4, 5]
    (njexl)1 @ s 
    =>true

## Index, Item , Context 

The "_" is the index. The "$" is the item. The whole list is passed as "context" as is accessible 
using "$$". But for what? In the predicate formulation - there is an implicit loop. 
One needs to understand the looping:
       
    l = list()
    for ( i : [ 1 , 2, 3, 4, 5 ] ){
      if ( i >= 2 and i <= 4 ){
        l = l + i
        }
    }
    // use l here.

This what actually filters/select things between [2,4].
But think about it. One can see that there is a for loop. Then there is an if block.
If the condition is true, then do something. This can be succinctly written as : 

    (njexl)a = [ 1, 2, 3, 4, 5 ]
    =>@[1, 2, 3, 4, 5]
    (njexl)select{ $ >= 2 and $<= 4 }(a)
    =>[2, 3, 4]

Now then - where should we use the context as in "$$"? Or rather the index "_" ?
We will give a classic example for this : 

### Verify that a list is sorted.

The idea would be doing this :  
       
    i = 0 
    sorted = true 
    while ( i < size(a)  ){
      if ( i > 0 &&  a[i] < a[i-1] ){
        //fail!
        sorted = false
        break; 
        }
      i = i+1
    }      
    if ( sorted ){ /*  this is where I know a is sorted. */ }

There you go : 

    (njexl)a = list( 0 , 1, 2, 3, 4, 5, 6, 6, 6, 7, 8 )
    =>[0, 1, 2, 3, 4, 5, 6, 6, 6, 7, 8]
    (njexl)empty( select{ _ > 0 and $ < $$[_-1] }(a) )
    =>true


The "_" is the current index of the implicit loop, while "$" is the current variable. "$$" is the argument context 
passed, which is the list "a" here.

All we are trying to test if any element is out of order, select that element.
As no element is selected - we are sure that the list is in order - i.e. sorted.

But the same thing can be done faster by the next one we would discuss.
That is first find, and exit when find.

## First Find 
Suppose you wan to find the element and the index of the element where some predicate P(e) is true.
Specifically, suppose I want to find the first element which is greater than 10 in a list.
What to do? This : 

    (njexl)a = [ 1, 2, 3, 30, 40 , -1]
    =>@[1, 2, 3, 30, 40, -1]
    (njexl)index{$>10}(a)
    =>3

If it would not find it, it would return -1.

     (njexl)index{ $ < 0 }(a)
     =>5
     (njexl)index{ $ == 0 }(a)
     =>-1

This is at most O(n), and on the average O(n/2), because it immediately terminates the loop after a match.
Index also works w/o anonymous function block, in that case the first element is taken as the item to be found, 
while the rest comprise of the list.

    (njexl)index(0,1,2,3,4,5,0)
    =>5
    (njexl)index(0,1,2,3,4,5,9)
    =>-1
    (njexl)index(1,a)
    =>0

Using index() then, finding if a list is in order or not is much easier : 

    (njexl)a=[1,2,3,4,5]
    =>@[1, 2, 3, 4, 5]
    (njexl)index{ _ > 0 and $ < $$[_-1] }(a) < 0 
    =>true
    (njexl)a=[1,2,3,4,5,2]
    =>@[1, 2, 3, 4, 5, 2]
    (njexl)index{ _ > 0 and $ < $$[_-1] }(a) < 0 
    =>false


## For Every Element of a List  : [FORALL e in L ] P(e) is TRUE

In predicate formulation we represent properties as computable functions.
Generally that is : 

![For all Form ](http://latex.codecogs.com/gif.latex?%5Cfn_phv%20%5Cforall%20x%20%5Cin%20L%20%5C%3B%20%3B%5C%3B%20P%28x%29%20%3A%20TRUE)

In this formulation - suppose we want to test if every element of a list is equal to integer 1.
Thus, P(x) becomes "x=1" and 

![Predicate Form of x = 1 ](http://latex.codecogs.com/gif.latex?%5Cfn_phv%20%5Cforall%20x%20%5Cin%20L%20%5C%3B%20%3B%5C%3B%20x%20%3D%201)


There would be two ways to do it : 

    (njexl)l = [1,1,1,1,1,1]    
    =>@[1,1,1,1,1,1]
    (njexl)s = set(l)              
    =>S{ 1 }
    (njexl)int(1) @ s and !empty(s)
    =>true

Then, the other way : 

    (njexl)empty( select{ int($) != 1 }(l) )
    =>true

Both works. 

## The Axiom of Choice : Multiplicative Axiom : The Join Operation

The idea is http://en.wikipedia.org/wiki/Axiom_of_choice : I can take product of sets ( lists ).
This is trivial in nJexl : 


    (njexl)a = list([1,2,2.01])
    =>[1, 2, 2.01]
    (njexl)b = list(0,1)
    =>[0, 1]
    (njexl)a*b
    =>[[1, 0], [1, 1], [2, 0], [2, 1], [2.01, 0], [2.01, 1]]

Fine, but we need pairs which are all different, thus : 

    (njexl)join{ $[0] != int($[1]) }(a,b)
    =>[[1, 0], [2, 0], [2, 1], [2.01, 0], [2.01, 1]]

 That is join for you. Clearly there is an implicit WHERE clause, within than block {}.
And yes, you can access individual tuples with $[n] which becomes a 0 based index.
The multiplication operator "*" does not have it, but "join" has it.

So, let's use this to do some fun stuff, find all pairs (x,y) where (x < y). 
Darn easy :

    (njexl)a = list("abc","def","xyz", "klm")           
    =>[abc, def, xyz, klm]
    (njexl)join{ $[0] < $[1] }(a,a)
    =>[[abc, def], [abc, xyz], [abc, klm], [def, xyz], [def, klm], [klm, xyz]]  


That should do it. You see, the whole idea is about code-less-ness.
As a professional tester, I understand that coding is bad, and hence I ensured none had to code.

## Collection Conversions 

### List to Dictionary 

Suppose we want to make a list of complex objects into a dictionary, such that f(obj) is the key
while "obj" is the value. How are we supposed to do it?
Well, there is a way: 

    (njexl)x = [ date('20001111') , date('20101010'), date('20150101') ] 
    =>@[Sat Nov 11 00:00:00 IST 2000, Sun Oct 10 00:00:00 IST 2010, Thu Jan 01 00:00:00 IST 2015]
    (njexl)y = dict{ [ $.getMonth() , $ ]  }(x)
    =>{0=Thu Jan 01 00:00:00 IST 2015, 9=Sun Oct 10 00:00:00 IST 2010, 10=Sat Nov 11 00:00:00 IST 2000}

Thus, we see that conversions can be done without resorting to any iteration.

### List to List 

Converting a list to another is known as [List Comprehension](http://en.wikipedia.org/wiki/List_comprehension).
From previous example, it is easy to do this : 

    (njexl)y = list{  $.getMonth()  }(x)
    =>[10, 9, 0]

There you go, straight. But real stuff would be conversion through a list, which is : 

    (njexl)x = [ "1", 2.00, '1.01', 3.01 ]
    =>@[1, 2.0, 1.01, 3.01]
    (njexl)y = list{ byte($) }(x)
    =>[1, 2, 1, 3]

One can of course, select, convert and make a list :

    (njexl)y = select{ where( byte($) > 1){ $ = byte($) }  }(x)
    =>[2, 3]


### Dictionary to List 

A dictionary is just a collection, so of course one can iterate over one - and generate a list.
There is a direct Java method : 

    (njexl)x = {1:2, 3:4, 5:6}
    =>{1=2, 3=4, 5=6}
    (njexl)x.values()
    =>[2, 4, 6]
    (njexl)x.keySet()
    =>[1, 3, 5]

Thus, obvious special modifications can be done, in either way:
     
    (njexl)y = list{ x[$] ** 2 }(x.keySet())
    =>[4, 16, 36]

Or, rather, 

    (njexl)y = list{ $ ** 2 }(x.values())
    =>[4, 16, 36]
