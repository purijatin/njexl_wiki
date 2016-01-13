# Practical Use Cases 

## Contents

* [Overview](#overview)
* [Restful API Calls](#restful-api-calls)
* [List Comprehension Examples](#list-comprehension-examples)
    * [How Close are Two Lists?](#how-close-are-two-lists)
* [Permutation and Combination](#permutation-and-combination)
    * [Anagrams](#anagrams)
    * [N Sum Problem](#n-sum-problem)
* [Number formatting and rounding](#number-formatting-and-rounding)
* [Summing them up](#summing-them-up)
* [On Manipulating Time](#on-manipulating-time)
* [Result of Competitive Exam](#result-of-competitive-exam)
* [Verify Filtering](#verify-filtering)
* [Some One Liners](#some-one-liners)
    * [Multiply Each Item in a List by 2](#multiply-each-item-in-a-list-by-2)
    * [Sum a List of Numbers](#sum-a-list-of-numbers)
    * [Verify if Exists in a String](#verify-if-exists-in-a-string)
    * [Filter list of numbers](#filter-list-of-numbers)
    * [Find extreemum in a List](#find-extreemum-in-a-list)
    * [Sieve of Eratosthenes](#sieve-of-eratosthenes)
    * [Filtering Collections](#filtering-collections)
* [Some More Sample Programs](https://github.com/nmondal/njexl/wiki/0-An-Easy-Tutorial#sample-programs)

## Overview

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

[Back to Contents](#contents)


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

[Back to Contents](#contents)

## Permutation and Combination 

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

Note the interesting "\_\_args\_\_=x" syntax. That let's you overwrite the argument of the function 
by the parameter you are passing. Thus, 

    func(a,b)

has the same effect has :

     func( __args__ = [a,b] )

Which, is by the way - not cool.
But wait, in the case of parameterizing combination / permutation - they surely are!

[Back to Contents](#contents)

### Anagrams

Wikipedia defines as [such](https://en.wikipedia.org/wiki/Anagram) :

>An anagram is a type of word play, the result of rearranging the letters of a word or phrase to produce a new word or phrase, using all the original letters exactly once; for example, the word anagram can be rearranged into nag-a-ram. 

Therefore, suppose we have a word "w". We need to find how many different anagrams of this word exist. 

#### Hard Solution

Given a dictionary, we :

* Permutate the letters of the word to create multiple unique words
* See if that word exist in the dictionary

This is easily done by :

    (njexl)w='noga'
    =>noga
    (njexl)l = w.toCharArray()
    =>@[n, o, g, a]
    (njexl)join{ t = str($,'') ;  where ( not (  t @ _$_ ) and $ == l ){ $ = t }  }(l,l,l,l)
    =>[noga, noag, ngoa, ngao, naog, nago, onga, onag, ogna, ogan, oang, oagn, gnoa, gnao, gona, goan, gano, gaon, anog, ango, aong, aogn, agno, agon]

You just need to add the dictionary check.

#### Easy Solution

Supposing we have a dictionary 'words.txt' :

    (njexl)system('wc -l /BigPackages/words.txt')
      354985 /BigPackages/words.txt
    =>0

Now we need to follow this algorithm:

##### Pre Process

* create a dictionary 
* for each word in the English dictionary   
* sort the word by letters, and create  another word by concatenating the sorted letters 
* if this key is new, append this new key, with value as a list containing the word 
* if this key already exist, get the value list for the key, append this word to it

Now that we are done prec processing, we can find anagrams, the list of words ( value )
are the anagrams!

    (njexl)words = lines ('/BigPackages/words.txt' )
    .... // too many lines 
    (njexl)d = lfold{ k = str( sorta( $.toCharArray() ),'') ; if ( k @ _$_ ){ _$_[k] += $  } else { _$_[k] = list($) } ; _$_  }(words,{:})
    ...// too many lines 


##### Find Anagrams 

* get a word from user 
* sort the letters of the word by creating the key
* fetch the list of words corresponding to the key which are the anagrams

This now becomes :

    (njexl)word = 'repeat'
    =>repeat
    (njexl)d[ str(sorta(word.toCharArray()),'')]
    =>[repeat, retape]



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

[Back to Contents](#contents)

## Number formatting and rounding

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

## Summing them up 

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

[Back to Contents](#contents)

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

[Back to Contents](#contents)

## Verify Filtering

Suppose there is a grid - with filtering support placed over columns.
The filters are of type :

 * Equals 
 * Less than 
 * Greater than 
 * In Range between min, Max 

Let's take the first problem it pose - that of :

> If any item in the column after filtering is not equal to the value set in the filter, it failed

This can easily be solved using :

    test_pass = index{ $ != filter_value }( filtered_values ) < 0 

and this would be the first case. Similarly, for the second :

    test_pass = index{ $  >= filter_value }( filtered_values ) < 0 

and for the 3rd :

    test_pass = index{ $  <= filter_value }( filtered_values ) < 0 

so, the question is, can we make it generic? Yes, we can, observe that from [currying](06-currying)
we have the idea of this :

    op_dict = {  'Equals' : '!=' , 'LessThan' : '>='  , 'GreaterThan' : '<=' }
    op = op_dict[filter_operation]
    test_pass = index{ `$ #{op} filter_value` }( filtered_values ) < 0 

But, how do we solve the last one, the range?
Clearly we can implement the range as :

    test_pass = index{ $  > filter_max_value or $ < filter_min_value }( filtered_values ) < 0 

Another faster way of saying the same thing is :

    test_pass = {  
        #(m,M) = minmax(filtered_values)  
        M <= filter_max_value and m >= filter_min_value
    } 
Observe that, we have a ternary operator here, *within*. 
That pose a problem, because we can not generalize this with the other binary *index* operations.
The first three are of the form : operation( item, value), while the last one is operation(item, value1, value2).

We can obviously solve this problem via :

    op = op_dict[filter_operation]
    if ( op != null ){
        test_pass = index{ `$ #{op} filter_value` }( filtered_values ) < 0 
        }else{
        #(m,M) = minmax(filtered_values)  
        test_pass = ( M <= filter_max_value and m >= filter_min_value )
    }

Which is not that bad, but not good either, declarative style succinctly tries to avoid
if-else-but. Hence, a newer idea can be put to use :


    def bin(l,v){  index{ `$ #{op.0} v` }(l) < 0   }
    def range(l,v1,v2){ #(m,M) = minmax(l) ; M <= v1 and m>= v2 ; }
    op_dict = {  'Equals' : [ '!=' , bin ], 
                 'LessThan' : [ '>=' , bin ] , 
                 'GreaterThan' : [ '<=' , bin ] 
                 'Range' : [ '' , range ]  }
    //... now use it ?
    op = op_dict[filter_operation]
    test_pass = (op.1)( filtered_values , value1, value2 )

And that is totally declarative, and solves our problem. This is a practical use of *closure* 
and currying.

[Back to Contents](#contents)

## Some One Liners 

From here  [10 scala one liners ](https://mkaz.com/2011/05/31/10-scala-one-liners-to-impress-your-friends/) ; plenty of so called *awesome* stuff?
I thought that I should just monkey it. So I mon-keyed it : 

#### Multiply Each Item in a List by 2

     (njexl)list{ 2*$ }([0:10])
     =>[0, 2, 4, 6, 8, 10, 12, 14, 16, 18] 

#### Sum a List of Numbers 

     (njexl)sqlmath ( list{ 2*$ }([0:10].list() ) )
     =>@[0, 18, 90] // the last one is the sum!
     (njexl)lfold { _$_ += 2*$  } ( [0:10].list() , 0 ) // (l|r)fold works too
     =>90 

#### Verify if Exists in a String 
 
     (njexl)line =  "Individuals are brilliant,  but people,  people are inherently stupid!"
     =>Individuals are brilliant,  but people,  people are inherently stupid!
     (njexl)bag = [ "but" ,  "people" , "are" , "stupid" ]
     =>@[but, stupid, are, people]
     (njexl)index{ $ @ bag }(line.split( "\W" )) >= 0
     =>true

#### Filter list of numbers
  
     (njexl)nos = [ 10, 20, 60, 10, 90, 34, 56, 91, 24 ]
     =>@[10, 20, 60, 10, 90, 34, 56, 91, 24]
     (njexl)partition{ $> 30 }(nos)
     =>@[[60, 90, 34, 56, 91], [10, 20, 10, 24]] 

#### Find extreemum in a List

    (njexl)sqlmath ( list{ 2*$ }( [0:10] ) )
    =>@[0, 18, 90] // the first one is the min, second one is the max!
    (njexl)rfold { _$_ = _$_>$ ? $:_$_  } ( [0:10].list() , 10 )
    =>0

#### Sieve of Eratosthenes

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
        

[Back to Contents](#contents)

## Filtering Collections

Observe that any webpage that does filtering has two primary components :

* A colelction of items on top of which filters should be applied
* A set of filters which are to be applied in tandem over the items

A general idea then of creating filters would be as this :

     def filter(item){  /* check something and return boolean */ }
     filters = [ filter1 , filter2, ... , filter_n ]

And now, to apply the filters :

     filtered_list = list()
     for ( i : items ){
         include = true 
         for ( filter : filters ){
             pass = filter(i)
             if ( !pass ){
                 include = false
                 break 
             }
         }
         if ( include ){
            filtered_list += i 
         }
     }

This is somewhat functional. But a much better way to write the same code would be :

     select{  _x_ = $ ; index{  !$(_x_) }(filters) < 0   }(items)

This, solves the problem in the same go. Now an example :

    (njexl)l = [1,3,9, 10, 16, 4, 25, 99 ]
    =>@[1, 3, 9, 10, 16, 4, 25, 99]
    (njexl)def odd(x){ x%2 != 0 }
    =>ScriptMethod{ name='odd', instance=false}
    (njexl)def is_square(x){ for ( i : [1:x/2 + 2 ] ){ if ( i** 2 == x ) return true  } ; false }
    =>ScriptMethod{ name='is_square', instance=false}
    (njexl)filters = [ odd, is_square ]
    =>@[ScriptMethod{ name='odd', instance=false}, ScriptMethod{ name='is_square', instance=false}]
    (njexl)select{ i = $ ; index{ !$(i) }(filters) < 0 }(l)
    =>[1, 9, 25]

This finds the odd, square of a number integers from the list of integers already passed!

[Back to Contents](#contents)
