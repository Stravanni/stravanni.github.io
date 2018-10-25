---------------------------------
New APIs
---------------------------------


Each service will have an implementation of the `executeService` interface.

Within each "api" we will also inset all the functions for transforming the output in the right format.


```python
def executeService(json_path):
	'''
	# call `readConfiguration(json_path)`
	# generates variables from `param` in the json
	# calls the script of the service passing the right parameters
	# takes care of the output
	'''
```


#### Read configuration (json file)

faheas.json
```json
{
	"inputPath" : "/data/fahes/in/",
	"outputPath" : "/data/fahes/out/",
	"param" : {}
}
```

imputedb.json
```json
{
	"inputPath" : "/data/fahes/out/",
	"outputPath" : "/data/imputedb/out/",
	"param" : {
				"table": "myTable",
				"query": "select * from myTable",
				"alpha": "1"
				}
}
```
etc.

#### Probelm with the format

Ideally, the script of each service should produce the right format:

- when possible we can modify the script/code of the service
- alternatively, we will just write additional scripts for converting the data in the right format
	- e.g.: The output of Fahes will be: input with null value where indicated by the current output of Fahes.

```python
def transformOutput(curentOutputOfTheService):
	# trnsformation
	return newFormattedOutput
```

- **format specification**: finally we agree

---------------------------------
Old APIs
---------------------------------

#### Fahes

- def `read_csv_directory(dir_name)`
- def `execute_fahes(source, out_path, debug=0)`
- def `execute_fahes_files(input_sources, output_location, debug = 0)`
- def `callFahes(tab_ref, tab_full_name, output_dir, debug)`

#### ImputeDB

- def `get_csv_paths(src)`
- def `get_postgres_paths(src)`
- def `put_csv_output(dst)`
- def `put_postgres_output(dst)`
- def `execute_imputedb(src, dst, query, alpha)`
- def `execute_imputedb_file(src_json, dst_json, query, alpha)`

#### PKDuck

- def `execute_pkduck(input_json, output_json, columns, tau)`
- def `execute_pkduck_file(input_json_file, output_json_file, columns, tau)`


#### DeepER

- def `write_clustered_csv(c_data, matches_file)`
- def `readCsv(csvFile, column, data)`
- def `createClusters(table1, table2, prediction_file, matches_file)`
- def `execute_deeper(source, table1, table2, number_of_pairs, destination, predictionsFileName)`

#### Golden Record
not considered for now

#### Aurum
not integrated yet.
If it provides a set of csv files (and metadata) it can be seen a source 


