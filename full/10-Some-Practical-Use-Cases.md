# Practical Use Cases 

>Nothing is more practical than a good theory.
>In theory practice matches theory. 
>In practice, it does not.

Thus, we showcase how a good theory can be used practically.

## Restful API Calls 

First one is shooting data back in JSON formt :

    /* Example 1 : How darn easy it is to run restful */
    _url_ = 'http://jsonplaceholder.typicode.com'
    data = 'posts/1'
    response = read( str:format("%s/%s", _url_ , data ) )
    ro = json(response)
    write(ro)

This bugger sends data back in XML formt :

    /*  Example 2 : How darn easy is to parameteraized stuff */
    _url_ = 'http://www.thomas-bayer.com/sqlrest'
    data = 'CUSTOMER/1'
    response = read( str:format("%s/%s", _url_ , data ) )
    ro = xml(response)
    write(ro)

Finally, I could not avoid showing this, this probably is cool :

    /* Example 3, JSON with parameter passing */
    _url_ = 'https://httpbin.org/get'
    params = { 'foo' : 'bar' , 'complexity' : 'sucks'  }
    data = str{  str:format( "%s=%s" , $.key, $.value )  }( params.entrySet() , '&' )
    response = read( str:format("%s?%s", _url_ , data ) )
    ro = json(response)
    write(ro)

It does tell you how easy it is to get things done, agreed?


## List Comprehension Examples 

We start with : is a particular item in a list with property P, exist?
Or rather formally, does an element x exist in List L, such that P(x) is true?
Now a practical example, do we have an item in this list such that x > 5 ?

    (njexl)L = [1,2,3,4,5,6,8,9]
    =>@[1, 2, 3, 4, 5, 6, 8, 9]
    (njexl)index{ $ > 5 }(L) >= 0 
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

## Permutation & Combination 

A very exotic computation on lists are permutation and combination over it.
The list permutation is a slightly tougher issue - thus we tackle the set permutation 
and combination.  

As usual, we look at the functional formulation of it, for finding Permutation up to 2 
elements : P(n,2) what we need to do is to join the list and check if the tuple (a,b) 
are not the same : 

    (njexl)l = list(1,2,3,4)
    =>[1, 2, 3, 4]
    (njexl)join{ $[0] != $[1] }(l,l)
    =>[[1, 2], [1, 3], [1, 4], [2, 1], [2, 3], 
       [2, 4], [3, 1], [3, 2], [3, 4], [4, 1], 
        [4, 2], [4, 3]]

Let's try to make it generalize for item count up to r :

    (njexl)join{ #|set($)| == #|$| }(l,l)
    =>[[1, 2], [1, 3], [1, 4], [2, 1], [2, 3], 
       [2, 4], [3, 1], [3, 2], [3, 4], [4, 1], 
         [4, 2], [4, 3]]

But one can see the last args are still hard coded, to remove that we need - the final script :

    import 'java.lang.System.out' as out

    // get the list 
    l = list(1,2,3,4)
    // generate the argument 
    x = list{ l }( [0:2].list() )
    out:printf("list : \n%s\n", x)
    // generate the permutation   
    p = join{ #|set($)| == #|$| }(__args__=x)
    out:printf("permutations : \n%s\n", p)
    // generate the combination : ideally should select over permutations 
    c = join{ #|set($)| == #|$| and 
        index{ _ > 0 and  $$[_-1] > $ }($) < 0 
        }(__args__=x)
    out:printf("combinations: \n%s\n", c )
    // return for validation 
    return [p,c]

Note the interesting "__args__=x" syntax. That let's you overwrite the argument of the function 
by the parameter you are passing. Thus, 

    func(a,b)

has the same effect has :

     func( __args__ = [a,b] )

Which, is by the way - not cool.
But wait, in the case of parameterizing combination / permutation - they surely are!

### N Sum Problem 
Given a number, and a list, is there a sub list such that the total of the sub list 
is equals to the number or not? This is what my friend hit, and wanted bonkers.
So, here is the nJexl equivalent solution :

    def possible( n, l ){
       // does it exist one level?
       found = ( index(n,l) >= 0 )
       // pretty clear 
       if ( found ) { return true }
       // start with nCc : 
       c = 2 
       // go deep in rabbit hole 
       while ( c <= #|l| ){
          x = array{ l } ( [0:c].list() )
          now = set()
          r = join{
              ms = set($)
              // did i hit this before?
              if ( ms =~ now ){ return false }
              // need only combination 
              if ( #|ms| != #|$|  ){ return false }
              // yes, a combination 
              now += ms
              // add them up 
              t = sqlmath($) 
              // compare and find if it is ok?
              t[2] == n 
          }(__args__ = x)
          found = not empty(r)
          if ( found ) { 
            write( r )
            return true 
          }
          c += 1
       }
       return false 
    }

As we can see, the possible() function works, if there is no repetition.
I would let you think, for allowing repetition what needs to be done!
NOTE : It would do something with list equals.

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

From here  [10 scala one liners ](https://mkaz.com/2011/05/31/10-scala-one-liners-to-impress-your-friends/) ; plenty of so called *awesome* stuff?
I thought that I should just monkey it. So I mon-keyed it : 

## Multiple Each Item in a List by 2

     (njexl)list{ 2*$ }([0:10].list() )
     =>[0, 2, 4, 6, 8, 10, 12, 14, 16, 18] 

## Sum a List of Numbers 

     (njexl)sqlmath ( list{ 2*$ }([0:10].list() ) )
     =>@[0, 18, 90] // the last one is the sum!
     (njexl)lfold { _$_ += 2*$  } ( [0:10].list() , 0 ) // (l|r)fold works too
     =>90 

## Verify if Exists in a String 
 
     (njexl)line =  "Individuals are brilliant,  but people,  people are inherently stupid!"
     =>Individuals are brilliant,  but people,  people are inherently stupid!
     (njexl)bag = [ "but" ,  "people" , "are" , "stupid" ]
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
    (njexl)rfold { _$_ = _$_>$ ? $:_$_  } ( [0:10].list() , 10 )
    =>0

## Sieve of Eratosthenes

Calculating prime numbers using 
[Sieve of Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) : 

	def soe( n ){
	    select {
	        x = $ // set the current iterate variable 
                // _$_ is the partial result as of now !
	        where ( index{ x % $ == 0 }( _$_ + 2 ) < 0 ){ $ = x }    
	    }([3:n+1].list()) + 2  // adding 2 in the end list of primes 
    }
    write( soe(31) )

When we run it :

    $ njexl soe.jexl 
    Script imported : JexlMain@/Users/noga/soe.jexl
    [3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 2]
        
 
## Result of Competitive Exam 

Some multiple choice exams has the rule that if you are correct you would be awarded a P, 
and if you fail it would be M ( a negative no). If you do not answer it, you get a 0.
How do you write a scoring algorithm for such an exam?

Here is how :

    P = 2.0 
    M = -0.5 
    actual_soln = "aabccbdaba"
    soln = "a bcd d b "
    // now with this ...
    (njexl)(sqlmath( 
             list{  where( empty($) ){ return 0 }  
                    where( $ == actual_soln[_] ){ return P }  
                    M  
                  } ( soln.toCharArray ) )
                  )[2] 

And the result comes out to be:

    =>7.5    

So that should tell you about it.
 