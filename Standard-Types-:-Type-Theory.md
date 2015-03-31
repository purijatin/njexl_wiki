# Theory of Types 

What the design of jexl did - is type compression.
By that what we really mean is - it finds the minimal container type of an object while computing stuff.

    njexl>a = list(0,1,2)
    =>[0, 1, 2]
    njexl>a[0].getClass()
    =>class java.lang.Byte

This is kind of intriguing and saves memory.
But it is confusing - and hence - if you wan to compare and be sure - then you should always type cast.
    
     njexl>a = list{int($)}(0,1,2)
     =>[0, 1, 2]
     njexl>a[0].getClass()
     =>class java.lang.Integer

   
This happens *only* when one passes constant values in the parameters in a function.



