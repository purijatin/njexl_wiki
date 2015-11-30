# Data Comparison 

## Contents
* [Overview](#overview)
* [Loading Data File](#loading-data-file)
    * [Delimiter](#delimiter)
* [Tables Comparison](#tables-comparison)
* [Transforming Matrices](transforming-matrices)
    * [Sub Matrix](#sub-matrix)
    * [Select Function](#select-function)
        * [Select](#select)
        * [Project](#project)


## Overview

In general - this comparison is easy. Let's demonstrate how it works.


## Loading data file  

Suppose I have a file like this : 

	cat test.tsv
	Number	First Name	Last Name	Points	Extra
	1	Eve	Jackson	94	
	2	John	Doe	80	x
	3	Adam	Johnson	67	
	4	Jill	Smith	50	xx


To load the file in data matrix : 

		(njexl)m1 = matrix('../samples/test.tsv')
		=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >

#### Delimiter

Note that the matrix function takes the delimiter also, default is *tab* or '\t', while
one can specify the delimiter :

	matrix('../samples/test.tsv', '\t')


## Tables Comparison 

Suppose we want to find the diff of the same file with itself : 

	(njexl)m1 = matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)m2 = matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)diff = m1.diff(m1,m2)
	=>false : < [] [] []>

This false basically says that the diff is *false*, i.e. the matrices are not different.
Now, let's change the matrices a bit.

## Transforming Matrices  

### Sub Matrix

To do so - you use sub() function.

     (njexl)ms = m1.sub(0,3)
     =>< S{ Number,Points } , [[1, 94], [2, 80], [3, 67], [4, 50]] >

That tells you - it is selecting columns with indices.
You can obviously use the select syntax : 

     (njexl)ms = m1.sub([0:4])
     =>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >


The RangeIterator [a:b] is allowed to, as expected. You can specify names of the columns.

     (njexl)ms = m1.sub('Number', 'Points')
     =>< S{ Number,Points } , [[1, 94], [2, 80], [3, 67], [4, 50]] >

But wait. There is no fun in it, w/o transform! Let's transform this now : 

     (njexl)ms = ms.sub{where(true){ $.Points = $.Points *10 } }()
     =>< S{ Number,Points } , [[1, 940], [2, 800], [3, 670], [4, 500]] >

That should do it.

### Select Function

Generic idea comes from database [*select* statement](http://www.w3schools.com/sql/sql_select.asp).
See more at [Relational Algebra](https://en.wikipedia.org/wiki/Relational_algebra)

#### Select 

Select works seamlessly with matrices.

      (njexl)m1.select{  "e" @ $['First Name']  }() 
      =>[[1, Eve, Jackson, 94]]

That should show it!

#### Project 

Project means selecting the columns. This can be done with either column index :

    (njexl)m.select(0,1,2)
    =>[[1, Eve, Jackson], [2, John, Doe], [3, Adam, Johnson], [4, Jill, Smith]]
    (njexl)m.select(0,1,2,3)
    =>[[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]]

Or can be done with column name :

    (njexl)m.select('First Name', 'Last Name', 'Points' )
    =>[[Eve, Jackson, 94], [John, Doe, 80], [Adam, Johnson, 67], [Jill, Smith, 50]]

