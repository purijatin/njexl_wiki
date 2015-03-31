# Extended Operators 

## Arithmetic Operators 

    +,-,*, /    // do what they supposed to do.
    **  // does exponentiation 
    |,&,^, ~ // they are standard bitwise operators 

## Extension of Arithmetic Operators 
   Based on what sort of stuff we are in, the operators change meaning. Not completely, but in a context sensitive way. 
    
      njexl>l=list(1,2,3)
      =>[1, 2, 3]
      njexl>l+4
      =>[1, 2, 3, 4] 


Hows this for a start? It simply get's better : 
     
       njexl>s=set(1,2,3,3,4)
       =>S{ 1,2,3,4 }
       njexl>s+5
       =>[1, 2, 3, 4, 5]
       njexl>s+5
       =>[1, 2, 3, 4, 5]

Take that. That comes in as default!
Now if you believe '+' acts in awesome ways - so does '-'.

     njexl>s-1
     =>[1, 2, 3, 4]

You basically got the idea, did not you?

## Uncommon Operations

Software Testers should not write code. Not because they can not, because when you write code, who tests that code?
Hence, the idea is minimising code to generate errors, and Java is a very sad language in this regard.
I mean seriously?    1000 != new Integer(1000)  ? That is not done. 
nJexl is fabulous in this regard.

     njexl>1==1
     =>true
     njexl>1=='1'
     =>true
     njexl>1=='1  '
     =>false

That is what is called common sense. Now, question why the heck string 1 is equal to integer 1?
Because a human mind thinks that way. Clearly '1   ' is no 1. Thus, if you do not like it, you can use the standard java technique :

     njexl>'1'.equals(1) 
     =>false 
Which works as expected. Else trust in the force of nJexl you should.
Basically what can be accomplished by 100 lines of Java code - can be done in 2/3 lines of nJexl code.
That was indeed the design goal. Thus, ends the discussion of "==".
Now then, there are other operators : 

## Logical Comparators 

    <, > , == , <=, >= , !=  // they do what they suppose to do. 

They are also named as 'lt','gt', 'eq', 'le', 'ge' , 'ne'.
Both form works. You choose. But, the fun is they are overloaded too.

## On Collections 

     njexl> a = list(1,2,3)
     =>[1, 2, 3]
     njexl>b = list(1,2)
     =>[1, 2]
     njexl>b<a
     =>true
 
Try beating that. It is hard to do so.
And no, not only on list. On sets too. In there, they have very precise meaning : 

  * A < B  implies A is a strict subset of B
  * A <= B  implies A is subset of B
  * A > B  implies B is a strict subset of A
  * A >= B  implies B is a subset of A
  * Obviously A == B is when the sets are equal.

But wait, that is not the only cool thing!

### Overlaps, Difference, Merger 
In the colourful language of set theory they are called 

* Intersection 
     Can be easily done by : 

           njexl> a = list(1,2,3)
           =>[1, 2, 3]
           njexl>b = list(1,2)
           =>[1, 2]
           njexl>a&b // AND is Intersection 
           =>[1, 2]

* Set Difference 
      Easily done : 
 
           njexl>a-b // MINUS is set minus 
           =>[3]

* Set Symmetric Difference 
      Fun thing to do, if people understood it - there would be less testing on db tables : 

           njexl>a^b    // XOR is Symmetric Difference 
           =>[3]
      
* Union 
          
           njexl>a|b   // OR is Union
           =>[1,2,3]
 
These works on list and sets.

## On the matter of Order
*"Order matters Not" - Noga *
Thus, 

        njexl>a = list(1,2,3,4,5,6)
        =>[1, 2, 3, 4, 5, 6]
        njexl>b = list(2,6,1)
        =>[2, 6, 1]
        njexl>a - b
        =>[3, 4, 5]
        njexl>a|b
        =>[1, 2, 3, 4, 5, 6]
        njexl>a&b
        =>[1, 2, 6]

Thus, it works as it should work. That is what normally known as re-usable code.
One guy writes it - and the others use it. No more random coding!

## The idea of General Multiplication 
Pople like this : 

      njexl>2*2
      =>4

But at the same time, multiplication is a revered operator : 

      njexl>a=list(0,1)
      =>[0, 1]
      njexl>a*a
      =>[[0, 0], [0, 1], [1, 0], [1, 1]]

Wow, that is something! But that brings you to the next:


## The beauty of Exponentiation 
Generally people likes it : 
     
      njexl>2**10
      =>1024.0

But then, it is trivial. Hmm. Yea, what about this ?
      
      njexl>"hi"**2
      =>hihi
      njexl>"hi"**-1
      =>ih
      njexl>"hi"**-2
      =>ihih

And the list exponentiation : 
       
      njexl>a=list(0,1)
      =>[0, 1]
      njexl>a**3
      =>[[0, 0, 0], [0, 0, 1], [0, 1, 0], [0, 1, 1], [1, 0, 0], [1, 0, 1], [1, 1, 0], [1, 1, 1]]

Yes, you guessed it right - the join operation is '*'. 
   