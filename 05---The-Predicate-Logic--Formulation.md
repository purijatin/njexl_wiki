# Predicate Logic 

It can be found here : http://en.wikipedia.org/wiki/First-order_logic

But in here we would discuss how the strategy helps in formulating business and tremendously reduces formulation of test cases. 

## There exist at least an element : [EXIST e in L : P(e) ]

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

We will give another classic example for this : 

### Verify that a list is sorted.
There you go : 

      (njexl)l = list( 0 , 1, 2, 3, 4, 5, 6, 6, 6, 7, 8 )
      =>[0, 1, 2, 3, 4, 5, 6, 6, 6, 7, 8]
      (njexl)empty( select{ _ > 0 and $ < $$[_-1] }(l) )
      =>true

The "_" is the current index of the implicit loop, while "$" is the current variable. "$$" is the argument context 
passed, which is the list "l" here.
All we are trying to test if any element is out of order, select that element.
As no element is selected - we are sure that the list is in order - i.e. sorted.

## For Every Element of a List  : [FORALL e in L ] P(e)

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

## The Axiom of Choice : Multiplicative Axiom 

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

