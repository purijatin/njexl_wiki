# The nJexl.testing Automation Framework

## Contents

* [Overview](#overview)
* [Data Driven Approach](#data-driven-approach)
    * [Object Repository](#object-repository)
    * [Data Sources and Sheets](#data-sources-and-sheets)
        * [Data Sheet to Script Connectivity](#data-sheet-to-script-connectivity)
* [UI Testing using Selenium](#ui-testing-using-selenium)
    * [Local Browser Support](#local-browser-support)
    * [Remote Browser Support](#remote-browser-support)
    * [Sample Production Usage](#sample-production-usage)
    * [Making Automation data driven](#making-automation-data-driven)
    * [Web Suite XML](#web-suite-xml)
    * [Sample BrowserStack configuration file](#sample-browserStack-configuration-file)
* [Web API Testing : REST](#web-api-testing--rest)
    * [The Jexl Script](#the-jexl-script)
    * [The Data Sheet](#the-data-sheet)
* [API Testing](#api-testing)
    * [Data Sheets](#data-sheets)
    * [Validators](#validators)
        * [Pre Validator](pre-validator)
        * [Post Validator](post-validator)
    * [Annotations Explained](#annotations-explained)
        * [NApiService](#napiservice)
        * [NApiServiceCreator](#napiservicecreator)
        * [NApiServiceInit](#napiserviceinit)
        * [NApi](#napi)
        * [NApiThread](#napithread)
        * [Performance](#performance)


## Overview

Software Testing is essentially limited to the following scenarios:

* (Non existent) Unit Test cases 
* API tests - a level above the unit tests 
* Integration tests
* Web Application ( Service Oriented Architecture ) services testing 
* UI Tests ( browser based )

nJexl.testing let's you test it all.
In this section we will see how to make your code instantaneously test-ready 
using njexl.testing.

To do so, either you need to have the executable jar, 
or you rather add the dependency njexl.testing in your project :
(The release version as of now is 0.2 )

```xml
<dependency>
  <groupId>com.github.nmondal</groupId>
  <artifactId>njexl.testing</artifactId>
  <version>0.2</version> <!-- or 0.3-SNAPSHOT -->
</dependency>
```

and use one of the test runners to call and run a suite from a standard
junit or testNG test case:

```java
public static TestSuiteRunner runner(String suiteFile) throws Exception {
        if ( suiteFile.endsWith(".api.xml")){
            return new WebServiceRunner(suiteFile);
        }
        return new WebSuiteRunner(suiteFile);
    }

 // get a runner...
    TestSuiteRunner runner = runner(suiteFile);
    // get on with the show...
    runner.run();
```
The suite file is the xml file - that has every information 
about the tests being performed.

[Back to Contents](#contents)

## Data Driven Approach 

Data should reside outside code.
One also needs to be very clear on - what precisely is data.

### Object Repository 

The standard poor industry standard practice of object repository is not correct.
Because UI objects are context sensitive, and same identifier here
will not remain same forever. So, while one can create an object repository,
in every sense of the world, it is discouraged. There is no point 
relocating to another data sheet - while looking for controls.

### Data Sources and Sheets

Data sources are source of data, they can either be:
 
  * directory containing tab delimited files as data sheets
  * excel files - with sheets 
  * url  - with HTML tables having data 
  * database - with individual tables have data  

#### Data Sheet to Script Connectivity 

Data sheets - or tables are the fundamental blocks.
They are columnar data - every column has a header.
They gets interpreted row by row, every row generates a 
[test vector](https://en.wikipedia.org/wiki/Test_vector).
The individual components can be accessed by the column header
as a variable in a script.

Thus, if the data table is this :


| A  | B  | C  |
|----|----|----|
| a1 | b1 | c1 |
| a2 | b2 | c2 |

[Back to Contents](#contents)

Then, the automatic variable "A" can be used to access the column in the particular row that 
is getting executed. This is automatic, and no coding is required, 
all you really have to do is to configure the data source properly in the suite.xml :

```xml
<!-- From where to load data -->
<dataSources>
    <!--  
        notice the "_", that means from
        the directory of the suite itself 
        load the xlsx file 
    -->
    <source name="sample"  location ="_/UIData.xlsx" />
</dataSources>
```    
and when using it by name "sample" :

```xml
<feature name="myTest" ds="sample" table="Data" script="selenium_demo" />
```

Thus, in the script selenium_demo.jexl - when it runs through the script, 
can access the columns by the variable named as column headers.

[Back to Contents](#contents)


## UI Testing using Selenium

Selenium RC was simplistic, and it worked. I firmly believe that the 

    verb  noun  arguments...

was good idea. nJexl.testing goes back to the root.
We have a Webdriver backed Selenium - extended to give benefits of the both world.
It drastically reduces the lines of code w/o compromising on staying the bleeding edge.
For those who really wants to know how Webdriver works - under the hood, it works
in the selenium RC way : https://code.google.com/p/selenium/wiki/JsonWireProtocol .

So, how a typical script looks like?
This :

	/*
	 Tests the polymer sample :
	 http://1.daily-stock-forecast.appspot.com
	*/
	import 'java.lang.System.out' as out 
	import 'java.lang.Thread' as Thread

	selenium.open("/") ;  ## Open the URL 
	// wait till the id appeared - and then click 
	@@selenium.click("search-dialog-button")
	// same, the @@ works like implicit waiters
	@@selenium.type("symbol_search" , "MSFT\n")
	// still wait - it is a demo!
	Thread.sleep(6000)


Now, then, that is it. Brings back the RC again.
For the coder in you - no, we do not like people coding at all.
We try to minimize the efforts - and put it back elsewhere.

[Back to Contents](#contents)

### Local Browser Support 

Clearly it locally supports everything that is supported by Selenium.
However, Out of Box supports are there for 
 
  * FireFox  
  * Chrome / Opera 
  * IE 
  * Safari.

To setup Chrome and Opera one needs to create environment variable, 
and put the driver path there. 
For Chrome the variable is "CHROME_DRIVER" .
For Opera ( Webkit based ) the variable is "OPERA_DRIVER".
When IE driver is in the path - it would simply run Internet Explorer too.

### Remote Browser Support 

I personally recommend BrowserStack. 
See the list of browsers and systems supported by them :
[BrowserStack Documentation](https://www.browserstack.com/automate/java)
That should get the ball rolling.

[Back to Contents](#contents)

### Sample Production Usage 
The Selenium object is custom made, [source](https://github.com/nmondal/njexl/blob/master/testing/src/main/java/com/noga/njexl/testing/ui/XSelenium.java) is as always open.
However, the cools stuff are embedded in, so probably not a good idea to copy paste it alone.

Here is a sample production code for a webapp testing I did.
See the blinding awesomeness ( in the style of Kung Fu Panda ) :


    import 'java.lang.System.out' as out
    /* 
        open url - selenium is aliased with XSelenium 
        See the source code 
    */
    selenium.open("/app/module/login/login.php")
    // login 
    @@selenium.type("id_tbx_uid", user)
    selenium.type("id_pwd_pwd", pass)
    selenium.click("name=submit")
    // assert a stupid stuff - that fails 
    assert:test{ selenium.isVisible("xxxx") }("This fails")
    // test login -- note that isElementPresent never throws error 
    assert:test( @@selenium.isElementPresent("menu") , "Successful Login")
    // what is this page? -- it may choose to... so 
    assert:test { selenium.title == 'Accounts' } ( "Login is in A/C Page" )
    // zoom out - an awesome feature of XSelenium 
    selenium.zoomOut(3)
    // These following elements must be there 
    options = json('ui.json')
    // pretty standard nested for ... 
    for ( x : options.keySet() ){
       loc = `link=#{x}` // standard variable substitution 
       // isVisible can always throw error, so guard it 
       assert:test { selenium.isVisible(loc) } ( x + " : is present" )
       x_inner = options[x] 
       selenium.mouseOver(loc)
       for ( ic : x_inner ){
           loc = `link=#{ic}`
           message = x + " > " + ic + " : is present"
           // guard it here too 
           assert:test { @@selenium.isVisible(loc) }( message )
       }
    }
    // load data table like a piece of cake : what more do you expect of a framework?
    (t,:e)  = selenium.table("0") // note the awesomeness -- with error check 
    // empty never throws exception 
    assert:test( empty(e) )("There is a data table in A/C Page" )
    // but t might be null, so...:
    assert:test{ not empty(t.rows) } ("There is data in A/C Page" )

As you can see, NO framework or language can get this done in less lines than this.

[Back to Contents](#contents)


### Making Automation data driven 

It is good to be driven by data, but before that you need to make sure 
that the scripts are ready to be data driven. How do you do that?

There is this nice little thing akin to pythons globals().
This is called :

     __current__ //  defines the current execution context 
     __current__.map // defines the variable map of the current context 

Thus, to append a variable to the current execution context would be :

     __current__.map['var_name' ] =  var_value 
     __current__.map.var_name  =  var_value // both are same !
     __current__.var_name  =  var_value // even this is the same as above !
     

Now, inside the script, one can use the variable :

     out:println(var_name)

which of course would use the vars value!
Such a demonstration can be taken to extreme - thus we can use :

     // this is first level of abstraction - before data source 
     data = { 'word' : "foobar" ,
              'name' : "foobar" ,
              'email' : "support@foobar.com" ,
              'phone' : "99999999" }

    // connect to automatic variables : caution, should not do it in prod 
    __current__.map.putAll(data)

    // and finally use it:
    @@selenium.type("habla_pre_chat_name_input" , name )
    selenium.type("habla_pre_chat_email_input" , email )
    selenium.type("habla_pre_chat_phone_input" , phone )

This is the last step before getting into fully productized test automation.
That would be done using external data sources, using something : implicit data sources.
That is a TestSuite abstraction, which follows next.

[Back to Contents](#contents)

### Web Suite XML

Against our better senses, we are going with XML. 
Typically, the suite explains the whole test :

```xml
<!--  
    browser : The browser to use for in case locally 
    remoteConfig : In case you want to use BrowserStack.com ;
    Then, that is the configuration file location :
    Supported file types are :
      [1] XML 
      [2] JSON 
      [3] Property Files  
-->
<testSuite version="4.2" browser="FIREFOX"  remoteConfig="samples/browserStackConfig.xml" >
    <!-- From where to load data -->
    <dataSources>
        <!--  
            notice the "_", that means from
            the directory of the suite itself 
            load the xlsx file 
        -->
        <source name="sample"  location ="_/UIData.xlsx" />
    </dataSources>
    <!--  The reports would be generated - 
    this is simple text stuff writing it to console
    type is the class name 
     -->
    <reporters>
        <reporter name="console" 
            type="com.noga.njexl.testing.reporting.SimpleTextReporter" />
    </reporters>
    <!-- 
        The actual testing stuff, 
        the url to hit, 
        directory of the script 
        log dir 
    -->
    <webApp name="DemoApp" build="4.2" scriptDir="_/" 
          url = "http://google.co.in" logs="reports/DemoApp"  >
        <!--
           They are broken into features, 
           relying on data source connection, data  table 
           and the script to execute, 
           which can call other scripts methods or Java code    
        -->  
        <feature name="Demo1" ds="sample" table="Data" 
            script="selenium_demo" owner="nmondal" enabled="true"/>
    </webApp>
</testSuite>
```

[Back to Contents](#contents)

### Sample BrowserStack configuration file 

A sample BrowserStack file would look like this :

```xml
<BSConfig>
    <user>your user name here</user>
    <key>your key in here</key>
    <browser>Opera</browser>
    <browserVersion>12.16</browserVersion>
    <os>Windows</os>
    <osVersion>7</osVersion>
</BSConfig>
```
Take a look around [BrowserStackDriver](https://github.com/nmondal/njexl/blob/master/testing/src/main/java/com/noga/njexl/testing/ui/BrowserStackDriver.java) to see what all other properties 
one can use from here. To know what all options are supported refer to :
[BrowserStack Documentation](https://www.browserstack.com/automate/java)

[Back to Contents](#contents)

## Web API Testing : REST 

To do so, one simply needs to have a suite customized to REST testing need:

```xml
<testSuite version="4.2">
    <dataSources>
        <source name="sample" location ="_/UIData.xlsx" />
    </dataSources>
    <reporters>
        <reporter name="console"
                  type="com.noga.njexl.testing.reporting.SimpleTextReporter" />
    </reporters>
    <webApp name="NSEApp" build="4.2" scriptDir="_/"
            url = "http://jsonplaceholder.typicode.com"
            logs="reports/json" method="GET" > <!-- specify the method GET/POST -->
        <feature name="GetPostByUser" base="posts" method="GET"
                 ds="sample" table="userId" afterScript="apiDemo.jxl"
                 owner="nmondal" enabled="true"/>
                 <!-- the script is the after test method -->
    </webApp>
</testSuite>
```

[Back to Contents](#contents)

#### The Jexl Script 

The corresponding jxl script can be :

	/*
	 The kind of data it parses is :
	 http://jsonplaceholder.typicode.com/posts
	*/

	import 'java.lang.System.out' as out
	// apparently this is how the result gets pushed 
	result = _o_
	//convert JSON to nJexl object : simple use of currying 
	d = `#{result}` 
	//print it - see that we are using data source column userId
	out:printf("userId: %s\n result: %s\n", userId, d)
	// this is good enough 
	return not empty(d)


#### The Data Sheet 

Here is how the data sheet "userId" looks like :

| userId  | B  | C  |
|----|----|----|
| 1 | b1 | c1 |
| 2 | b2 | c2 |

[Back to Contents](#contents)

## API Testing

nJexl lets you do api testing.
It is data driven, as you have expected, and as usual it works in this way :

```java
import com.noga.njexl.testing.api.Annotations.*;
import com.noga.njexl.testing.api.junit.JClassRunner;
import org.junit.runner.RunWith;

@RunWith(JClassRunner.class)
@NApiService(base = "samples/")
@NApiServiceCreator  // the defaults are good enough 
public class NApiAnnotationSample {

    @NApiServiceInit // tell that this constructor to initiate 
    public NApiAnnotationSample(){}
    
    // this should be pretty obvious 
    @NApi(use = false, dataSource = "UIData.xlsx", dataTable = "add" ,
            before = "pre.jexl", after = "post.jexl", 
            globals = {"op=+"} )
    @NApiThread
    public int add(int a, int b) {
        int r = a + b ;
        System.out.printf("%d + %d = %d \n", a, b, r );
        return r;
    }
    @NApi(dataSource = "UIData.xlsx", dataTable = "sub" ,
            before = "pre.jexl", after = "post.jexl" , 
            globals = { "op=-" } )
    // crucial - would use for performance        
    @NApiThread( dcd = true, performance = @Peformance())  
    public int subtract(int a, int b) {
        int r = a - b ;
        System.out.printf("%d - %d = %d \n", a, b, r );
        return r;
    }
}

```
[Back to Contents](#contents)

### Data Sheets 

For subtraction the data sheet is :

| a  | b  | 
|----|----|
| 10 | 4  | 
| 12 | 5  | 


### Validators

The pre and post jexls are simple.

#### Pre Validator 


    /**
      pre.jexl
      A demo of before method - how to say go no-go for a method
    */
    import 'java.lang.System.out' as out
    out:println("Pre Validator")
    return _cc_ != null // this is just a place holder


#### Post Validator 

    /**
      post.jexl 
      A demo of after method - how to test a method
    */
    import 'java.lang.System.out' as out
    // _cc_ stores the call container
    actual = _cc_.result
    // `` is the currying, allows you to operate a string as if it is a function
    expected = `_cc_.parameters[0] #{_g_.op} _cc_.parameters[1]`
    // note the use of _g_ to use per method global variables 
    // access arbitrary java object as namespace
    out:printf("Expected %s , Actual %s\n", expected, actual)
    // return can be made implicit
    expected == actual

[Back to Contents](#contents)

### Annotations Explained 

#### NApiService  
This is to decorate a class saying : it is ready to be tested using nJexl.testing.
It has an optional parameter - *base* which denotes the base directory for the class, 
i.e. where all the data files and script files would reside. 

Parameters are :

* use : if false, won't use this class : default true 

* base : the base dir 


#### NApiServiceCreator
This is to state, what sort of creator should instantiate an instance of a service object, 
i.e. the class which is to be tested.

It has multiple optional parameters :

* type : The class creator type (a Java Class Type) , 
         default is : ServiceCreatorFactory.SimpleCreator.class
* args : String[] , denoting arguments which would be parsed by JexlEngine to create 
         the arguments of the class creator


[Back to Contents](#contents)

#### NApiServiceInit 
This is a marker, only one is allowed to put in one of the constructors of the test class.
This denotes that it would use this constructor to construct a service object.

It has multiple optional parameters :

* bean : A shame from Spring, 
         denotes the bean name of the class in the spring context file. 
* args : String[] , denoting arguments which would be parsed by JexlEngine to create 
         the arguments of the class 
         In case we are using spring, it is the paths to the context xml file

#### NApi
This designates a service method to be tested. It has multiple parameters :

* use : if false, won't use this method : default true 

* dataSource :  The data location path, can be an URL, a directory, an Excel file

* dataTable :  The data file name, or table index, or the sheet name 

* before : The jUnit before method : this time nJexl script that would be invoked

* after : The jUnit after method : this time nJexl script that would be invoked

* globals : Any global variables one needs to pass to each of the test vectors 
      In fact it is a dictionary. In this dictionary, one specifies key : value pair as :
            
               key=value
 
      Then, the key can be accessed in the validators by :

               _g_.key 

      Which is how it works.

[Back to Contents](#contents)

#### NApiThread

Optional decorator to any method - stating it is for threaded ( stress/load/perf ) testing.
It has multiple optional parameters :

* use : Shall we use the threading ? Default true.
        If you want to turn it off, set it to false. 
        
* numThreads : Number of threads to spawn - each acting as a different client.

* spawnTime : The delay to incur between spawning threads. 
        Spawning, takes time. Even you are bacterial colony. Which we are.

* numCallPerThread : Per thread, how many times one needs to call the method 

* pacingTime : Time delay between two subsequent call in a thread.

* dcd : Stands for Different Call Different ( data row ). 
       It has a default value of false. If one sets this to true, 
       then every call made in threading mode would have used different data row.
       This is very important for application caching their data per process.
       

* performance : Another annotation to do performance testing.
                Defaults to no performance testing 

[Back to Contents](#contents)

#### Performance 
This does the performance testing.

* use : When set to true, the performance characteristics are reported. Default is true.
               And tests aborts if the experimental percentile  does not match the
               supplied lessThan percentile value.

* percentile : Classic percentile, global for all the test vectors.
              This is a short integer, between 1 to 99.
              For example to get median you should use :

              @Performance(percentile = 50 )
                

* lessThan : The value of the percentile,global for all the test vectors.
              You can change that by adding a column %PERCENTILE% in the test data sheet. 
              Defaults to 10 sec.             

[Back to Contents](#contents)
