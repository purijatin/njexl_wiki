# Collections 

There are two generic operations.

       njexl>a = list()
       =>[]
       njexl>empty(a)  // if a collection is empty 
       =>true 
       njexl>size(a)  // what is the size of the collection 
       =>0 

Also every collection is indexable, col[x] is valid for all of them. And that includes for Sets too.

## Array
The most easily used one is an array type. Everything inside it automatically becomes an object.
Thus : 

    njexl>a = [1,2,3,4] 
    =>[I@45fe3ee3
    njexl>a[0].getClass()
    =>class java.lang.Integer

As we can see arrays can be indexed by their offset. 
You can assign stuffs to arrays.

      njexl>a = [1,2,'hi', "hello"]
      =>[Ljava.lang.Object;@b1a58a3
      njexl>a[0]
      =>1
      njexl>a[2]
      =>hi
      njexl>a[2] = "bye"
      =>bye
      njexl>a[2]
      =>bye

## Lists 
Interesting same is applicable for lists! One can easily convert an array to a list : 

      njexl>a = list(a)
      =>[1, 2, bye, hello] 
      njexl>a[0]
      =>1
      njexl>a[2] = 'Australia 2015'
      =>Australia 2015
      njexl>a
      =>[1, 2, Australia 2015, hello] 

### Anonymous Functions 

Sometimes you want to create a list, but then want to apply transform on each element.
Fear not : 

       njexl>a
       =>[1, 2, Australia 2015, hello]
       njexl>b = list{int($)}(a) // anything inside that { } gets applied over all the elements in the list 
       =>[1, 2, null, null]

Wait. That is a problem. 
None ordered those nulls!  So what should we do to filter out the elements who are not integers?
      
       njexl>b = select{ where(int($)){ $ = int($) }}(a)
      =>[1, 2]

What's that? where is proverbial where in SQL. When the expression for when is true - the block gets executed.
Inside the block the $ = int($) assigning the value of the element back after transform!
How cool is that?

## Sets
Sets are lists where occurrences are not repeated.
Thus : 

       njexl>l = list(1,2,2,3,4,4,5)
       =>[1, 2, 2, 3, 4, 4, 5]
       njexl>s = set(l)
       =>S{ 1,2,3,4,5 }


Anonymous functions becomes crucial here, because the key is the element itself : 


       njexl>l = list(1,"1  ", "1") 
       =>[1, 1  , 1]
       njexl>s = set(l)
       =>S{ 1,1  ,1 }

Should not they be all 1 ? You bet they should be, in that case : 

     njexl>s=set{int($)}(l)
     =>S{ 1 }

just works!  




