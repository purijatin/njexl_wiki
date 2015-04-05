# Von Neumann Architecture 

*data is same as executable code*. Read more on :  
http://en.wikipedia.org/wiki/Von_Neumann_architecture

# Currying 

The theory really behind JVN-A is called currying.
http://en.wikipedia.org/wiki/Currying

## Why on Earth?

Because it is cool. No, am just joking around. This is the most important thing that happened in the theory of computation - and thus in the hole darn history of software. Currying. Please understand it.

The tenet, as usual is - code is bad. Branching is part of code and thus, squarely bad.
Test automation is testing those branching of code hence cubely bad. Finally, test automation as tons of branches that makes it a ripe harvest of people sucking out money. That is x^4 bad. 'BAD'**4 if you are using nJexl.


I do not like it. I believe, if/else/branches and code are like wine, good results come only when used in moderation. So - enough said - what precisely is the need of it? Suppose you are to test, and mind you test, if the calculator application is adding stuff properly or not. Next test would be if they are subtracting stuff or not. Next would be  if they multiply properly or not....

Basically something of the form we call a binary operator is FINE or not.

      operator_1 <operator> operator_2 

That is a general binary operator, yes. Now, how to test it? The test code would be, invariably messy ( not Mesi, who is God's parting Gift to Humanity, but just Messy ) :

     if ( operation == '+' ) {
        do_plus_check();
     } else if ( operation == '-' ) {
        do_minus_check();
     }
     ....

And someone comes in, automates it in 2 days - and cheerleaders ( read : project managers)  promote him/her.
A bit too much, stone aged suff. Currying comes to help.

Curry said : "All binary operators are same category". By that what it means, if someone can abstract the operation out - and replace the operation with the symbol - and then someone can actually execute the string as code ( remember JVMA?) you are done.

### Example Currying 

So what does that all mean? That means this.
First take the string `x #{op} y`. Now, replace the nitty part called hot-curry : the #{} thing and you are done.
All you need to do is to execute it back again as a script - which nJexl neatly does. Thus...

     (njexl)x = 2
     =>2
     (njexl)y = 3 
     =>3
     (njexl)op = '+'
     =>+
     (njexl)`x #{op} y`
     =>5

That should show it. Now you see, no more thousands of lines of stupid mind boggling automation script. We keep it simple - because we know we are stupid.
Some more demonstration follows : 

     (njexl)op = '**'
     =>**
     (njexl)`x #{op} y`
     =>8.0
     (njexl)op = '=='
     =>==
     (njexl)`x #{op} y`
     =>false
     (njexl)op = '!='
     =>!=
     (njexl)`x #{op} y`
     =>true

That is the power - of 1960 computer science. No, not *computer* engineering, and heavens no, no software engineering. In any case, the great JVN knew it, and that is why there is a JVN architecture.






 