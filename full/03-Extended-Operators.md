# Extended Operators 

## Contents 

* [Overview](#overview)
* [Definition Coaleasing Operator](#definition-coaleasing-operator)
* [Arithmetic Operators](#arithmetic-operators)
* [Extension of Arithmetic Operators](#extension-of-arithmetic-operators)
* [Uncommon Operations on Equality](#uncommon-operations-on-equality)
* [Logical Operators](#logical-operators)
* [On Collections](#on-collections)
    * [Overlaps, Difference, Merger](overlaps-difference-merger)
    * [On the matter of Order](#on-the-matter-of-order)
    * [Operations over a Dictionary](#operations-over-a-dictionary)
        *[Division over a Dict](#division-over-a-dict)
* [Axiomatic Multiplication](#axiomatic-multiplication)
* [Axiomatic Exponentiation](#axiomatic-exponentiation)

## Overview
This is where we talk about all sort of operators, 
extended, and default and what not.

## Definition Coaleasing Operator

The special operator "??" is to be used as a quick short hand for :

     y = x??0
     if ( not #def(x) or x eq null ) {  y = 0 } else { y = x }


## Arithmetic Operators 
Generally we start with Arithmetic: 

    +,-,*, /     // do what they supposed to do.
    **           // does exponentiation 
    |,&,^, ~     // they are standard bitwise operators 

## Extension of Arithmetic Operators 
Based on what sort of stuff we are in, the operators change meaning. 
Not completely, but in a context sensitive way. 
    
    (njexl)l=list(1,2,3)
    =>[1, 2, 3]
    (njexl)l+4
    =>[1, 2, 3, 4] 

Hows this for a start? It simply get's better : 
     
    (njexl)s=set(1,2,3,3,4)
    =>S{ 1,2,3,4 }
    (njexl)s = s+5
    =>S{ 1,2,3,4,5 }
       
Take that. That comes in as default!
Now if you believe '+' acts in awesome ways - so does '-'.

     (njexl)s-1
     =>S{ 2, 3, 4 , 5 }

You basically got the idea I suppose?

[Back to Contents](#contents)


## Uncommon Operations on Equality

Software Testers should not write code. Not because they can not, because when you write code, who tests that code?
Hence, the idea is minimizing code to generate errors, and Java is a very sad language in this regard.
I mean seriously?    1000 != new Integer(1000)  ? That is not done, that is lame. 
nJexl is fabulous in this regard.

     (njexl)1==1
     =>true
     (njexl)1=='1'
     =>true
     (njexl)1=='1  '
     =>true

That is what is called common sense. Now, question why the heck string 1 is equal to integer 1?
Because a human mind thinks that way. However, '1   ' is no 1. Thus, if you do not like it, 
you can use the standard *Java*  (utterly stupid) technique :

     (njexl)'1'.equals(1) 
     =>false 
     (njexl)i=int(1)
     =>1
     (njexl)i.equals(1)
     =>true

Or, you can use the borrowed from Javascript "===" operator, thus : 

      (njexl)1==='1'
      =>false
      (njexl)1===1
      =>true

Which works as expected. 
If you really believe otherwise, take a look around this also to convince yourself: 

    (njexl)a=[1,2,3]
    =>@[1, 2, 3]
    (njexl)b=[2,3,1]
    =>@[2, 3, 1]
    (njexl)c=list(2,3,1)
    =>[2, 3, 1]
    (njexl)a == b and b == c
    =>true
    (njexl)b === c
    =>false
    (njexl)a===b
    =>true
    (njexl){1:2} == {1:2}
    =>true
    (njexl){1:2,3:4} == {3:4,1:2}
    =>true


Else trust in the force of nJexl you should.
Basically what can be accomplished by 50/60 lines of Java code - can be done in 2/3 lines of nJexl code.
Try writing a full proof code of list equality in Java to convince yourself.
Thus, ends the discussion of "==" and "===".
Now then, there are other operators, which we discuss next.

[Back to Contents](#contents)

## Logical Comparators 

    <, > , == , <=, >= , !=  // they do what they suppose to do. 

They are also named as 'lt','gt', 'eq', 'le', 'ge' , 'ne'.
Both form works. You choose. But, the fun is in the fact that they are overloaded too.

## On Collections 

     (njexl) a = list(1,2,3)
     =>[1, 2, 3]
     (njexl)b = list(1,2)
     =>[1, 2]
     (njexl)b<a
     =>true
 
Try beating that. It is hard to do so.
And no, not only on list. On sets too. In there, they have very precise meaning : 

 * A < B  implies A is a strict subset of B
 * A <= B  implies A is subset of B
 * A > B  implies B is a strict subset of A
 * A >= B  implies B is a subset of A
 * Obviously A == B is when the sets are equal.

And thus, the idea is simply:

    (njexl)[1,2] < [3,1,2]
    =>true
    (njexl)[1,2,3] < [3,1,2]
    =>false
    (njexl)[1,2,3] <= [3,1,2]
    =>true   


These operations are tenable for array, List, Hash or Set type.
List and arrays are mixable - while set is not.
   
     (njexl)list(1,2,3) ==  [3,1,2]
     =>true

For hashes, the comparison is funny but accurate: 

     (njexl){1:2} == {1:2}
     =>true
     (njexl){1:2} <= {1:2}
     =>true
     (njexl){1:2} > {1:2}
     =>false
     
Empty list, arrays and stuffs are always everyones sub-thing: 

     (njexl)[] < [1]
     =>true
     (njexl)[] < []
     =>false
     (njexl)[] <= []
     =>true
     (njexl)[] == []
     =>true
     (njexl){:} <  {1:2,2:4}
     =>true
     (njexl){:} <  {:}
     =>false
     (njexl){:} ==  {:}
     =>true


If two things are essentially non-comparable - that is, a is not a proper sub-thing of b, then : 

     (njexl)[3] <  [1]
     =>false
     (njexl)[3] >  [1]
     =>false
     (njexl)[3] >=  [1]
     =>false
     (njexl)[3] <=  [1]
     =>false
     (njexl){1:2, 2:3 } <= {1:2,3:4}
     =>false
     (njexl){1:2, 2:3 } == {1:2,3:4}
     =>false
     (njexl){1:2, 2:3 } == {1:2,2:4}
     =>false

But wait, that is not the only cool thing!
[Back to Contents](#contents)


### Overlaps, Difference, Merger 
In the colourful language of set theory they are called 

* Intersection 
     Can be easily done by : 

            (njexl) a = list(1,2,3)
            =>[1, 2, 3]
            (njexl)b = list(1,2)
            =>[1, 2]
            (njexl)a&b // AND is Intersection 
            =>[1, 2]

* Set Difference 
      Easily done : 
 
           (njexl)a-b // MINUS is set minus 
           =>[3]

* Set Symmetric Difference 
      Fun thing to do, if people understood it - there would be less testing on db tables : 

           (njexl)a^b    // XOR is Symmetric Difference 
           =>[3]
      
* Union 
          
           (njexl)a|b   // OR is Union
           =>[1,2,3]
 
These works on list and sets and dictionaries.

### On the matter of Order

   >"Order matters *NOT*, because nature *is chaotic*." 
   - Noga

Thus, 

    (njexl)a = list(1,2,3,4,5,6)
    =>[1, 2, 3, 4, 5, 6]
    (njexl)b = list(2,6,1)
    =>[2, 6, 1]
    (njexl)a - b
    =>[3, 4, 5]
    (njexl)a|b
    =>[1, 2, 3, 4, 5, 6]
    (njexl)a&b
    =>[1, 2, 6]

Thus, it works as it should. This is what normally known as re-usable code.
One guy writes it - and the others use it. No more random coding!

### Operations over a Dictionary 

Sometimes it is of important to do the same operations over dictionary.
Thus, we have :

    (njexl)d = {'a' : 0 , 'b' : 1 }
    =>{a=0, b=1}
    (njexl)d - 'a' // minus the key 
    =>{b=1}
    (njexl)d - { 'a' : 0 } // minus the dictionary proper 
    =>{b=1}
    (njexl)d - { 'a' : 2 } // try again, fails 
    =>{a=0, b=1}

Same with intersection and union :

    (njexl)d | { 'c' : 3 , 'b' : 0 }
    =>{a=0, b=(1,0), c=3}
    (njexl)d & { 'c' : 3 , 'b' : 1 }
    =>{b=1}


[Back to Contents](#contents)

#### Division over a Dict 

There is one interesting operation - division over a dict.
That is :

    (njexl)d = {'a' : 0 , 'b' : 1 , 'c' : 0 }
    =>{a=0, b=1, c=0}
    (njexl)d/0 // finds the keys which has a value 0.
    =>S{ a,c }


###  Axiomatic Multiplication 
People like this : 

    (njexl)2*2
    =>4

But at the same time, multiplication is an abstract operator on collection to generate product collection: 

    (njexl)a=list(0,1)
    =>[0, 1]
    (njexl)a*a
    =>[[0, 0], [0, 1], [1, 0], [1, 1]]

Wow, that is something! But that brings you to the next:

### Axiomatic Exponentiation 
Generally people likes it : 
     
    (njexl)2**10
    =>1024

But then, it is trivial. Hmm. Yea, what about this ?
      
    (njexl)"hi"**2
    =>hihi
    (njexl)"hi"**-1
    =>ih
    (njexl)"hi"**-2
    =>ihih

Essentially, this is of the form string to the power a number is standard regular expression form.    

And the list exponentiation : 
       
    (njexl)a=list(0,1)
    =>[0, 1]
    (njexl)a**3
    =>[[0, 0, 0], [0, 0, 1], [0, 1, 0], [0, 1, 1], [1, 0, 0], [1, 0, 1], [1, 1, 0], [1, 1, 1]]

Yes, you guessed it right - the join operation is '*'. 

[Back to Contents](#contents)
