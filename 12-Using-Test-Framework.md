# The nJexl.testing Automation Framework

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

```xml
<dependency>
  <groupId>com.github.nmondal</groupId>
  <artifactId>njexl.testing</artifactId>
  <version>0.1</version>
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


## The Data Driven Approach 

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

### The Web Suite XML

Against our better senses, we are going with XML. 
Typically, the suite explains the whole test :

```xml
<!--  The browser to use for  -->
<testSuite version="4.2" browser="FIREFOX">
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

### Web API Testing : REST 

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

### The Jexl Script 

The corresponding jxl script can be :

	/*
	 The kind of data it parses is :
	 http://live-nse.herokuapp.com/?symbol=INFY
	*/

	import 'java.lang.System.out' as out
	// apparently this is how the result gets pushed 
	result = _o_
	//convert JSON to Dictionary : use currying 
	d = `#{result}` 
	//print it - see that we are using data source column userId
	out:printf("userId: %s\n result: %s\n", userId, d)
	// this is good enough 
	return not empty(d)


### The Data Sheet 

Here is how the data sheet "userId" looks like :

| userId  | B  | C  |
|----|----|----|
| 1 | b1 | c1 |
| 2 | b2 | c2 |

 