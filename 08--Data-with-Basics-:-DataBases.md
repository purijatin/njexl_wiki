# Database 

Databases comprise of bulk of enterprise applications. The idea of any sort of testing or development is to ensure that there is seamless database connectivity.
Thus, it is of importance that we support DB connectivity. As expected - we do have seamless database connectivity.

To connect to db - we need a bunch of informations - if you are a typical Java guy you would do it this way : 


         Class.forName("org.postgresql.Driver");//ensure I have the driver 
         Connection connection = null;
             connection = DriverManager.getConnection(
            "jdbc:postgresql://hostname:port/dbname","username", "password");
         // url, user, pass 

Now then, this is boring - and pretty much configuration incentive job. nJexl makes it seamless.
To start with - you need to have one single thing - a JSON file ( a default one is named as db.json). 
The default one looks like this : 

         {
      "noga": {
        "dbName": "noga",
        "url": "jdbc:postgresql://localhost:5432/noga",
        "driverClass" : "org.postgresql.Driver",
        "user": "noga",
        "pass": ""
      },
      "some2": {
        "dbName": "dummy",
        "url": "dummy",
        "driverClass" : "class",
        "user": "u",
        "pass": "p"
      },
      "some3": {
        "dbName": "dummy",
        "url": "dummy",
        "driverClass" : "class",
        "user": "u",
        "pass": "p"
      }
    }

So basically you get the idea. Every connection has an ID kind of thing - the one we would use is called "noga".
And then someone needs to init the DB connectivity. Those are done like this : 

      import 'com.noga.njexl.extension.dataaccess.DBManager' as db  
      // which connection profile ? 
      db:init('../samples/db.json')
      Successfully Loaded DB Config
      {some2=some2 : [dummy,class] @dummy : u, some3=some3 : [dummy,class] @dummy : u, noga=noga : [noga,org.postgresql.Driver] @jdbc:postgresql://localhost:5432/noga : noga}
      =>true

Now, what sort of table we need to do a query one?  Here is the table  

![Table ](http://s30.postimg.org/r4jeclkip/Screen_Shot_2015_04_03_at_3_53_42_pm.png)

And the query you specify this way : 

     // query is darn easy : result is a data matrix object 
     (njexl)matrix =  db:results("noga", "select * from noga_demo")
     =>< S{ Id,Name } , [[1, Noga], [2, Jaideep ], [3, Partha], [4, Chanda], [5, Shweta], [6, Waseem]] >


Note that the "noga" signifies the key specified in the db JSON file. It basically says, get me a connection using the JSON block specified -- i.e. the postgress connection. 

## The DataMatrix 

Now, the matrix returned is a very interesting object it is - in fact a matrix object, where in turn you can write sql like queries! 

           // which lets you further subquery if need be 
           qr = matrix.select{ _ > 1 and _ < 4 }( "Name" ) // specify by list of column names 
           =>[[Partha], [Chanda]]
           (njexl)qr = matrix.select{ _ > 1 and _ < 4 }( 0 ) // specify by list of column indices 
           =>[[3], [4]]
           (njexl)qr = matrix.select{ _ > 1 and _ < 4 }( 0 , "Name" ) // you can mix - so...
            =>[[3, Partha], [4, Chanda]]

Thus, the data matrix is capable of way cool stuffs.
Whatmore, one can individually see the columns and rows : 

         (njexl)matrix.rows
         =>[[1, Noga], [2, Jaideep ], [3, Partha], [4, Chanda], [5, Shweta], [6, Waseem]]
         (njexl)matrix.columns
          =>S{ Id,Name }

To get a mapping between what column is what column index - we used the Awesome ListSet : 

       (njexl)matrix.columns.get(0)
       =>Id
       (njexl)matrix.columns.get(1)
       =>Name
       (njexl)matrix.columns.indexOf("Name")
       =>1
       (njexl)matrix.columns.indexOf("xxx")
       =>-1

Now, every row can be accessed by tuples. 
This is the thing - http://en.wikipedia.org/wiki/Tuple_relational_calculus 
This is what it means : 

       (njexl)matrix.tuple(0)
       =>< [ 0->'Id' 1->'Name'] @[ 1 , Noga ] >
       (njexl)(matrix.tuple(0))["Id"]
       =>1

And that is why in the query - instead of standard sql where column_name ... gets replaced with $["column_name"].
But better, in nJexl a["xx"] is same as a.xx ! 
Thus, we can have : 

      (njexl)qr = matrix.select{ $["Name"] == "Noga" }()
      =>[[1, Noga]] 
      (njexl)qr = matrix.select{ $.Name == "Noga" }()
      =>[[1, Noga]] 

That should be a pretty interesting thing, no?
The classic LIKE operation becomes "@" operation : 

      (njexl)qr = matrix.select{ "e" @ $.Name  }()
      =>[[2, Jaideep ], [5, Shweta], [6, Waseem]]

Its imply gets better - because nJexl supports regular expressions! Thus  : 

      (njexl)"abcdef" =~ "abc.*"
      =>true
      (njexl)qr = matrix.select{ $.Name =~ ".*a$" }() // ends with "a".
      =>[[1, Noga], [3, Partha], [4, Chanda], [5, Shweta]]

Pretty awesome, right?












