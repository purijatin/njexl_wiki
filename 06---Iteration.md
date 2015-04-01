#To Iterate is Human, to Recurse is divine.

We are human, and hence, we do iteration more.
Simplistic way to look at an iteration is :  

      import 'java.lang.System.out' as out
      i = 0 
      while ( i < 3 ){
	     out:println(i)
	     i = i + 1
      }

This of course produce the output :  

     0
     1
     2

But this is trivially boring.
If you are using "i" , it is meaningless to do all the housekeeping by yourself, and hence ... 

    for ( i  :  [0:3] ){
        out:println(i)
    }

Does produce the same output! It is kind of bad to be verbose. You think it is good to be verbose?
http://projects.haykranen.nl/java/ is for you then. We want to get things done, with minimal talk - and maximal work. 
But wait. Anything array like is iterable, anything list like is iterable. Anything set like, if they are sets created by nJexl - are iterable too. And even dictionaries are default iterable. 

    for ( i  :  [0,1,2] ){
        out:println(i)
    } 

This is fine. So is  : 

    for ( i  :  {0:'0' , 1:'1' , 2:'2' } ){
        out:println(i)
    } 

And finally this is fairly interesting : 

    for ( i  :  "Smartest might survive" ){
        out:print( str:format("%s ", i ) ) 
    } 
    out:println()

Produces the output : 

     S m a r t e s t   m i g h t   s u r v i v e 

And that basically means - it reads the individual characters of a string.
That should be awesome.








