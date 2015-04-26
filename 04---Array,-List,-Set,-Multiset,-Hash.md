# Collections 

There are two generic operations.

       (njexl)a = list()
       =>[]
       (njexl)empty(a)  // if a collection is empty 
       =>true 
       (njexl)size(a)  // what is the size of the collection 
       =>0 

Now then size() is a kind of bad way of representing things so we have #|expr| operator.

       (njexl)#|1.0-0.4|
       =>0.6
       (njexl)#|1.0-4|
       =>3.0
       (njexl)a = [1,2,3,4]
       =>@[1, 2, 3, 4]
       (njexl)#|a|
       =>4
       (njexl)#|"Hello!"|

       

Thus, #|expr| also is abs() function, which is important for Business Logic and Validation.


Also every collection is indexable, col[x] is valid for all of them. And that includes for Sets too.

## Array
The most easily used one is an array type. Everything inside it automatically becomes an object.
Thus : 

    (njexl)a = [1,2,3,4] 
    => @[1, 2, 3, 4]
    (njexl)a[0].getClass()
    =>class java.lang.Integer

As we can see arrays can be indexed by their offset. 
You can assign stuffs to arrays.

      (njexl)a = [1,2,'hi', "hello"]
      =>@[1, 2, hi, hello]
      (njexl)a[0]
      =>1
      (njexl)a[2]
      =>hi
      (njexl)a[2] = "bye"
      =>bye
      (njexl)a[2]
      =>bye

## Lists 
Interesting same is applicable for lists! One can easily convert an array to a list : 

      (njexl)a = list(a)
      =>[1, 2, bye, hello] 
      (njexl)a[0]
      =>1
      (njexl)a[2] = 'Australia 2015'
      =>Australia 2015
      (njexl)a
      =>[1, 2, Australia 2015, hello] 

### Anonymous Functions 

Sometimes you want to create a list, but then want to apply transform on each element.
Fear not : 

       (njexl)a
       =>[1, 2, Australia 2015, hello]
       (njexl)b = list{int($)}(a) // anything inside that { } gets applied over all the elements in the list 
       =>[1, 2, null, null]

Wait. That is a problem. 
None ordered those nulls!  So what should we do to filter out the elements who are not integers?
      
       (njexl)b = select{ where(int($)){ $ = int($) }}(a)
      =>[1, 2]

What's that? where is proverbial where in SQL. When the expression for when is true - the block gets executed.
Inside the block the $ = int($) assigning the value of the element back after transform!
How cool is that?

## Sets
Sets are lists where occurrences are not repeated.
Thus : 

       (njexl)l = list(1,2,2,3,4,4,5)
       =>[1, 2, 2, 3, 4, 4, 5]
       (njexl)s = set(l)
       =>S{ 1,2,3,4,5 }


Anonymous functions becomes crucial here, because the key is the element itself : 


       (njexl)l = list(1,"1  ", "1") 
       =>[1, 1  , 1]
       (njexl)s = set(l)
       =>S{ 1,1  ,1 }

Should not they be all 1 ? You bet they should be, in that case : 

     (njexl)s=set{int($)}(l)
     =>S{ 1 }

just works!  

## Milti Set
A multi set is essentially a list - where items can be repeated. But using that one can compare lists.
How so? Because we start treating list as Maps, the key is the element (by default ) - and the values are the list of such elements. This interpretation makes sense. 

Let's take a look around it : 


      (njexl)list(1,1,2,2,3,2,4)
      =>[1, 1, 2, 2, 3, 2, 4]
      (njexl)mset(1,1,2,2,3,2,4)
      =>{1=[1, 1], 2=[2, 2, 2], 3=[3], 4=[4]} 

Note that, it actually keyed down the elements and stored them as list!
But why it does that? That is because sometimes different objects can be *mapped* to same. For example : 


      (njexl)mset{ int($) } ( 1, 1.05, "1" , 2, 2.1 )
      =>{1=[1, 1.05, 1], 2=[2, 2.1]}


Where do I need it? Yes, we need it in case of SQL Group By - and more interestingly when we compare 2 lists which can be keyed!

        (njexl)ms1 = mset{ int($) } ( 1, 1.05, "1" , 2, 2.1 )                      
        =>{1=[1, 1.05, 1], 2=[2, 2.1]}
        (njexl)ms2 = mset{ int($) } ( 1, 1.05, "1" , 2 )          
        =>{1=[1, 1.05, 1], 2=[2]}
        (njexl)set:mset_diff(ms1,ms2)
        =>{1=[I@53e25b76, 2=[I@73a8dfcc}
        (njexl)d = set:mset_diff(ms1,ms2)
        =>{1=[I@ea30797, 2=[I@7e774085}
        (njexl)list(d[1])  
        =>[3, 3]
        (njexl)list(d[2])
       =>[2, 1]


It basically saying that for the key 1, both left and right multisets are having same number of values (3). 
But, for the key 2, left and right multisets are having different values , left : 2 and right : 1.
Of course all list operations are treated as multiset operations.

## Relation between Lists and Sets 

Those can be found by : 

    (njexl)set:set_relation([1],[2])
    =>INDEPENDENT
    (njexl)set:set_relation([1,2],[2,3])
    =>OVERLAP
    (njexl)set:set_relation([1,2,3],[1,2,3])
    =>EQUAL
    (njexl)set:list_relation([1,2],[2,3])
    =>OVERLAP



## Hashes  
They are list of key value pairs.
They are accessible by syntax value = hash[key] 

      (njexl)cwc = { 2011 : 'Ind' , 2015 : 'Aus' }
      =>{2011=Ind, 2015=Aus}
      (njexl)cwc[2011]
      =>Ind
      (njexl)cwc[1981]
      =>null
 
Empty dictionaries can be created : 
      
      (njexl)ed = {:}
      =>{}
      (njexl)empty(ed)
      =>true
      (njexl)size(ed)
      =>0

And you can start putting things in it : 

     (njexl)ed[0]=0
     =>0
     (njexl)ed
     =>{0=0}
 
Whatmore, you can take two lists and make a dictionary out of it : 

      (njexl)cwc = dict([2011,2015],['Ind','Aus'])
      =>{2011=Ind, 2015=Aus}
      (njexl)cwc[2015]
      =>Aus


The syntax container[x] is overloaded. It works for every container type, including Sets which are made by nJexl. 
       
       s = set(1,2,3,4)
       =>S{ 1,2,3,4 }
       (njexl)s[0]
       =>1

At the same time - it is also a short hand for property!
For example : 

       (njexl)import 'java.lang.System' as SYS
       =>class java.lang.System
       (njexl)SYS['out']
       =>java.io.PrintStream@58d25a40

How cool is that? At the same time - we can start using it even : 

       (njexl)SYS['out'].println("Hello, Awesome nJexl!")
       Hello, Awesome nJexl!
       =>null


Thus, we can access any field of any class instance as this O['field name'] syntax!


### The String operation on Collection
Anything that is complex is a collection. Be it list, be it Hash, be it an Object.
In any case, sometimes we need to serialise back the object in question -- preferably so that it becomes a tuple.
How so? This demonstrates how : 


     (njexl)d={'a': 'A' , 'b' : 'B' }
     =>{a=A, b=B}
     (njexl)str{ [ $.a , $.b  ]}(d)
     =>A,B
     (njexl)str{ [ $.a , $.b  ]}(d,'#')
     =>A#B


This way, one can take arbitrary  object and can get a serialised form out of it.
Notice the use of "[" and "]" to make the functional expression as an *array*. Thus, we actually can take any list - and serialise it straight : 


    (njexl)str{ [ 1,2,3 ]}(0)
    =>1,2,3
    (njexl)str([1,2,3])
    =>1,2,3

Now, interestingly, we can use any string to use as delimiter : 

    (njexl)str([1,2,3],"&&")
    =>1&&2&&3
 
That should be good!

### Linearising Tuple 
The usage of such a craft is to reduce dimension of multi dimensional arrays, and lists.
This is a handy example : 


     (njexl)D = [ {'a': 'A1' , 'b' : 'B1' } , { 'a' : 'A2' , 'b' : 'B2' } ]
     =>@[{a=A1, b=B1}, {a=A2, b=B2}]
     (njexl)x = set{  str{ [ $.a , $.b] }($, '#')  }(D) ## Use the str to linearise the individual rows 
     =>S{ A1#B1,A2#B2 } ## Here, a 2-d array became single D array!




