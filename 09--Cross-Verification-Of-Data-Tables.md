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

     		(njexl)import 'org.apache.commons.jexl2.extension.dataaccess.DataMatrix' as matrix
		=>class org.apache.commons.jexl2.extension.dataaccess.DataMatrix
		(njexl)matrix
		=>class org.apache.commons.jexl2.extension.dataaccess.DataMatrix
		(njexl)m1 = matrix:file2matrix('../samples/test.tsv')
		=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >

Suppose we want to find the diff of the same file with itself : 

	(njexl)m1 = matrix:file2matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)m2 = matrix:file2matrix('../samples/test.tsv')
	=>< S{ Number,First Name,Last Name,Points } , [[1, Eve, Jackson, 94], [2, John, Doe, 80], [3, Adam, Johnson, 67], [4, Jill, Smith, 50]] >
	(njexl)diff = matrix:diff(m1,m2)
	=>false : < [] [] []>

This false basically says that the diff is *false*, i.e. the matrices are not different.
Now, let's change the matrices a bit.




