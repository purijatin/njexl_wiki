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





