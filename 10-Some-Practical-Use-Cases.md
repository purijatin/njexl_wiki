# Practical Use Cases 

>Nothing is more practical than a good theory.
>In theory practice matches theory. 
>In practice, it does not.

Thus, we showcase how a good theory can be used practically.

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

### How Close are Two Lists?

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

But a better bet will be using join, which avoid generating all possible Tuples...

    (njexl)l1 = [0.1 , 2.0, 4.1 ]
    =>@[0.1, 2.0, 4.1]
    (njexl)l2 = [0.13 , 2.2, 3.98 ]
    =>@[0.13, 2.2, 3.98]
    (njexl)join{ #|$[0] - $[1]| < 0.2  }(l1,l2) 
    =>[[0.1, 0.13], [4.1, 3.98]]
    (njexl)join{ #|$[0] - $[1]| <= 0.2  }(l1,l2) 
    =>[[0.1, 0.13], [4.1, 3.98]]

And that should explain it!

Two lists are item by item almost same?
    
     (njexl)L2 = [0.21, 0.11 , 0.29]
     =>@[0.21, 0.11, 0.29]
     (njexl)L1 = [0.1, 0.2 , 0.3]
     =>@[0.1, 0.2, 0.3]
     (njexl)empty( select{ #| $ - L2[_]| < 0.001 }(L1) )  
     =>true

Item by item. That should do it!


## Number formatting and rounding. 

This shows the generic idea : 
   
     (njexl)str:format("%.4f", 0.235678d)
     =>0.2357

Now you need a generic formatting -- so : 
  
     (njexl)str:format("%%.%df",4)
     =>%.4f
     (njexl)str:format ( str:format("%%.%df",4) , 0.2345678)
     =>0.2346

This is a different sort of currying! 
Now, all of these can be easily accomplished in a line by:

     (njexl)str(0.2345678,4)
     =>0.2346

Hence, comparing doubles upto arbitrary precision values are easy.


## Summing them up : 

You want to add individual items for a list.

     (njexl)L1 = [0.1, 0.2 , 0.3]
     =>@[0.1, 0.2, 0.3]
     (njexl)sqlmath(L1)
     =>@[0.1, 0.3, 0.6000000000000001]

This returns you min,max, sum. In an array.
But it also takes anonymous function so : 

    (njexl)L1 = ['0.1', '0.2' , '0.3']
    =>@[0.1, 0.2, 0.3]
    (njexl)sqlmath{float($)}(L1)
    =>@[0.1, 0.3, 0.6000000000000001]


## On Manipulating Time

Suppose we need to find number of days between two dates.
So: 

    (njexl) date('20150101') 
    =>Thu Jan 01 00:00:00 IST 2015
    (njexl) [date('20150101') : date('20150209')].days // using a DateRange!
    =>39
    (njexl)td = [date('20150101') : date('20150209')]
    =>Thu Jan 01 00:00:00 IST 2015 : Mon Feb 09 00:00:00 IST 2015 : PT86400S
    (njexl)td.seconds
    =>3369600
    (njexl)td.hours
    =>936
    (njexl)td.minutes
    =>56160

# Some One Liners 

From here :  [10 scala one liners ](https://mkaz.com/2011/05/31/10-scala-one-liners-to-impress-your-friends/)
I thought that I should just monkey it. So I mon-keyed it : 

## Multiple Each Item in a List by 2

     (njexl)list{ 2*$ }([0:10].list() )
     =>[0, 2, 4, 6, 8, 10, 12, 14, 16, 18] 

## Sum a List of Numbers 

     (njexl)sqlmath ( list{ 2*$ }([0:10].list() ) )
     =>@[0, 18, 90] // the last one is the sum!

## Verify if Exists in a String 
 
     (njexl)line =  "Individuals are brilliant,  but people,  people are inherently stupid!"
     =>Individuals are brilliant,  but people,  people are inherently stupid!
     (njexl)bag = [ "but" , "stupid" , "are" , "people"  ]
     =>@[but, stupid, are, people]
     (njexl)index{ $ @ bag }(line.split( "\W" )) >= 0
     =>true
 
## Filter list of numbers
  
     (njexl)nos = [ 10, 20, 60, 10, 90, 34, 56, 91, 24 ]
     =>@[10, 20, 60, 10, 90, 34, 56, 91, 24]
     (njexl)partition{ $> 30 }(nos)
     =>@[[60, 90, 34, 56, 91], [10, 20, 10, 24]] 


## Find minimum (or maximum) in a List
    
    (njexl)sqlmath ( list{ 2*$ }([0:10].list() ) )
    =>@[0, 18, 90] // the first one is the min, second one is the max!
    
## Sieve of Eratosthenes

Calculating prime numbers using [Sieve of Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) : 

     import 'java.lang.System.out' as out 
	def soe( n ){
	    select {
	        x = $ // set the current iterate variable 
                // _$_ is the partial result as of now !
	        where ( index{ x % $ == 0 }( _$_ + 2 ) < 0 ){ $ = x }    
	    }([3:n+1].list()) + 2  // adding 2 in the end list of primes 
	}
	out:println( soe(31) )

        $ njexl soe.jexl 
        Script imported : JexlMain@/Users/noga/soe.jexl
        [3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 2]
        
 
So that should tell you about it.
 