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








