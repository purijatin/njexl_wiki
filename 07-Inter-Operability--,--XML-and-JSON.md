# Inter Operability 

## Using JSON

This is what JSON is : http://www.w3schools.com/json/
Typical JSON looks like : 

        $cat sample.json 
        {"employees":[
            {"firstName":"John", "lastName":"Doe"}, 
            {"firstName":"Anna", "lastName":"Smith"},
        {"firstName":"Peter", "lastName":"Jones"}
        ]}
        $

And now, thanks to already awesome way nJexl handles Hashes and Arrays, it is a perfectly valid Hash Object.
Thus, the standard : json() function works straight. 

        (njexl)j=json('sample.json')
        =>{employees=[Ljava.util.HashMap;@66048bfd}
        (njexl)j['employees']
        =>@[{firstName=John, lastName=Doe}, {firstName=Anna, lastName=Smith}, {firstName=Peter, lastName=Jones}]

Now obviously - One can access the fields : 

        (njexl)j['employees'][0]
        =>{firstName=John, lastName=Doe}
        (njexl)j['employees'][0]['firstName']
        =>John
        (njexl)j['employees'][0]['lastName']
        =>Doe

Awesome, right? You bet. No extra fancy stuff - nothing really. Less talk - and more work. The way the pioneers of computer science envisioned programming - which later the *software engineers* destroyed.


## The excess of Xml

Xml was, is, and always will be a very bad idea [ http://harmful.cat-v.org/software/xml/ ]
  > “XML combines the efficiency of text files with the readability of binary files” – unknown
But thanks to many *big* enterprise companies - it became a norm to be abused human intellect - the last are obviously Java EE usage in Hibernate, Springs and Struts. Notwithstanding the complexity and loss of precise network bandwidth - it is a very popular format. Thus - against my better judgement I could not avoid Xml.
And yes, it is hidden.

First let me show the xml : 


        <slideshow 
        title="Sample Slide Show"
        date="Date of publication"
        author="Yours Truly"
        >

        <!-- TITLE SLIDE -->
        <slide type="all">
          <title>Wake up to WonderWidgets!</title>
        </slide>

        <!-- OVERVIEW -->
        <slide type="all">
            <title>Overview</title>
            <item>Why <em>WonderWidgets</em> are great</item>
            <item/>
            <item>Who <em>buys</em> WonderWidgets</item>
        </slide>
        </slideshow>




In fact it is hidden in :  

        (njexl)import 'org.apache.commons.jexl2.extension.dataaccess.XmlMap' as xml
        =>class org.apache.commons.jexl2.extension.dataaccess.XmlMap
        (njexl)x = xml:file2xml('sample.xml')
        =>name='slideshow',text=''
        attr={null=Sample Slide Show}
        children=[name='slide',text=''
        attr={null=all}
        children=[name='title',text='Wake up to WonderWidgets!'
        attr={}
        children=[]
        ]
        , name='slide',text=''
         attr={null=all}
         children=[name='title',text='Overview'
         attr={}
         children=[]
        , name='item',text='Why  are great'
        attr={}
        children=[name='em',text='WonderWidgets'
        attr={}
        children=[]
        ]
        , name='item',text=''
        attr={}
        children=[]
        , name='item',text='Who  WonderWidgets'
        attr={}
        children=[name='em',text='buys'
        attr={}
        children=[]
        ]
        ]
        ]

Thus, any node can be accessed like objects straight away.






