# Predicate Logic 

It can be found here : http://en.wikipedia.org/wiki/First-order_logic

But in here we would discuss how the strategy helps in formulating business and tremendously reduces formulation of test cases. 

### Verify that a list is sorted.

There you go : 

      njexl>l = list( 0 , 1, 2, 3, 4, 5, 6, 6, 6, 7, 8 )
      =>[0, 1, 2, 3, 4, 5, 6, 6, 6, 7, 8]
      njexl>empty( select{ _ > 0 and $ < $$[_-1] }(l) )
      =>true

The "_" is the current index of the implicit loop, while "$" is the current variable. "$$" is the argument context 
passed, which is the list "l" here.
All we are trying to test if any element is out of order, select that element.
As no element is selected - we are sure that the list is in order - i.e. sorted.

## Every Element of a List  : P(e)

In predicate formulation we represent properties as computable functions.
Generally that is : 

![General Predicate Form ](http://latex.codecogs.com/gif.latex?%5Cfn_phv%20%5Cforall%20x%20%5Cin%20L%20%5C%3B%20%3B%5C%3B%20P%28x%29%20%3A%20TRUE)

In this formulation - suppose we want to test if every element of a list is equal to integer 1.
Thus, P(x) becomes "x=1" and 

![Predicate Form ](http://latex.codecogs.com/gif.latex?%5Cfn_phv%20%5Cforall%20x%20%5Cin%20L%20%5C%3B%20%3B%5C%3B%20x%20%3D%201)


There would be two ways to do it : 


      njexl>l = [1,1,1,1,1,1]    
      =>[I@1c655221
      njexl>s = set(l)              
      =>S{ 1 }
      njexl>int(1) @ s and !empty(s)
      =>true

Then, the other way : 

      njexl>empty( select{ int($) != 1 }(l) )
      =>true

Both works. 



 




