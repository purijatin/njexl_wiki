# Table to Table comparison 

In general - this comparison is easy. Let's demonstrate how it works.

## Loading data file as DataMatrix 

Suppose I have a file like this : 

	$ cat ../samples/test.tsv
	Number	First Name	Last Name	Points
	1	Eve	Jackson	94
	2	John	Doe	80
	3	Adam	Johnson	67
	4	Jill	Smith	50
	$

To load the file in data matrix : 

     		(njexl)import 'com.noga.njexl.lang.extension.dataaccess.DataMatrix' as matrix
		=>class com.noga.njexl.lang.extension.dataaccess.DataMatrix
		(njexl)matrix
		=>class com.noga.njexl.lang.extension.dataaccess.DataMatrix
		(njexl)m1 = matrix:file2matrix('../samples/test.tsv')
		=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >

## Comparing with Other Tables 

Suppose we want to find the diff of the same file with itself : 

	(njexl)m1 = matrix:file2matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)m2 = matrix:file2matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)diff = matrix:diff(m1,m2)
	=>false : < [] [] []>

This false basically says that the diff is *false*, i.e. the matrices are not different.
Now, let's change the matrices a bit.

## Sub Matrices & Transforming Matrices  

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

## Select on Matrices 

Select works seamlessly with matrices.





