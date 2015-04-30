# Practical Usage 

>Nothing is more practical than a good theory.
>In theory practice matches theory. In practice, it does not.

Thus, we showcase how a good theory, can be used in practical usage.

## List Comprehension Examples 

We start with : is a particular item in a list with property P, exist?
Or rather formally, does an element x exist in List L, such that P(x) is true?
Now a practical example, do we have an item in this list such that x > 5 ?

     (njexl)L = [1,2,3,4,5,6,8,9]
     =>@[1, 2, 3, 4, 5, 6, 8, 9]
     (njexl)index{ $ > 5 }(L) > 0 
     =>true

Now, is all element of a list having some property P?
That is, is all element of a list is non zero?

    #|select{ $ < 0 }(L)| == 0 

Take that for expressiveness.
Ok, now some tough one. 
Given two lists, L1(x), L2(y) find all pairs such that P(x,y) holds true.
To do so, take that |x-y| < epsilon.
Here you go : 

     (njexl)L1 = [0.1, 0.2 , 0.3]
     =>@[0.1, 0.2, 0.3]
     (njexl)L2 = [0.21, 0.11 , 0.29]
     =>@[0.21, 0.11, 0.29]
     (njexl)select{ #|$[0] -$[1]| < 0.01 }(L1*L2) // showcasing how the idea evolved : 
     =>[[0.1, 0.11], [0.2, 0.21]]
     (njexl)#|select{ #|$[0] -$[1]| < 0.01 }(L1*L2)| > 0
     =>true

Two lists are item by item almost same?
    
     (njexl)L2 = [0.21, 0.11 , 0.29]
     =>@[0.21, 0.11, 0.29]
     (njexl)L1 = [0.1, 0.2 , 0.3]
     =>@[0.1, 0.2, 0.3]
     (njexl)empty( select{ #| $ - L2[_]| < 0.001 }(L1) )  
     =>true

Item by item. That should do it!


## Number formatting and rounding. 




