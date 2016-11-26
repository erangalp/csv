# csv
CSV import / export for PHP. Provides convenience methods for reading CSV files into associative PHP arrays (for insertion into a database, or any other use), and for exporting PHP arrays into CSV files.

## Importing CSV data

A common scenario would be to read a CSV file that contains structure different than what we need for our purpose (for example, to fit into a database schema). Often, there are columns we don't need and columns that need to be renamed.

We'll read the CSV file and then transform it into the structure we want to use in PHP:

	<?php
	
	// Instance CSV class by passing the CSV file in the constructor
	$csv = new Csv('/var/www/html/uploads/report.csv');
	
	// Pass an array that declares which CSV columns to transform into PHP array values, based on the first 
	// CSV row which is assumed to be a titles row. All columns not declared in the array will be discarded
	
	$map = array(
		'First Name' => 'first_name',
		'Last Name' => 'Last Name',
		'Email' => 'email',
		'OrderDate' => 'created',
		'TotalAmt' => 'amount',
		'TransID' => 'transaction_id'
	);
	
	// Passing the array mapping will transform the CSV data into an associative PHP array
	$csv -> setAssociations($map);
	
	// Get the PHP data back
	$rows = $csv -> getRows();
	
	// At this point we can insert the data into the database, display it to user or anything else we'd like
	// to use it for.

## Explorting CSV data 

In this scenario we want to do the opposite from the previous example - transform a PHP array into CSV data with
certain labels for each column. We can export the CSV in 3 ways - 

* As a file on the server (for storage or later retrieval)
* Force an immediate download of the file in the browser (with no server storage)
* As a string containing the CSV data

	<?php
	$csv = new Csv();
	
	// Just for example purposes, we use the same structure we previously used to import the data.
	$map = array(
		'First Name' => 'first_name',
		'Last Name' => 'Last Name',
		'Email' => 'email',
		'OrderDate' => 'created',
		'TotalAmt' => 'amount',
		'TransID' => 'transaction_id'
	);
	
	// In this instance we load the association map without any data loaded, since we don't need to transform the data
	$csv -> setAssociations($map);
	
	// Then we load the data. $rows here is PHP array data, where each row in the array 
	// is an associative array containing row data. This is a common format in which 
	// data is retrieved from databases using PHP.
	
	$csv -> setRows($rows);
	
	// We output the CSV using one of the methods outlined above. In this case, the class will output
	// the file with forced download headers, triggering a download in the browser.
	$csv -> write(Csv::DOWNLOAD,'report.csv');
	