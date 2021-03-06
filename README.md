## sqlt - A minimalistic SQL template reander

### Why ?

SQL is fantastic tool for data processing, either ETL or generate report.

As a data engineer, I enjoyed using Spark RDD API to for BigData processing, but I also found that a lot of things can also be easily done using Spark-SQL, or BiqQuery by using SQL. 

The recent Spark-SQL did quite a bit code optimization that could optimize and speeds up the query. 

As DSL SQL code has smaller footprint and describe the problem better, and quite often it runs faster than newbie's and less eleborated code of using RDD API.

As a bonus, your code can easier migrate to BigQuery if it needs to.

This tool intends to bridging the gap the SQL is not really good having parameters, by allowing you to provide parameter from command-line and render them with your SQL template files into actual SQL file your want to execute.

You can speicify multiple SQL statements:

```
./sqlt.py 'SELECT * from table_{a};' 'SELECT * from table_{b};' -x a=AAA b=BBB
```
It will output:

```SQL
---------------------------------------------------------
-- generated by sqle (https://github.com/tly1980/sqle) --
---------------------------------------------------------
-- with xargs: a=AAA; b=BBB

-- sql_1 from: <FROM ARGS>
SELECT * from table_AAA;

-- sql_2 from: <FROM ARGS>
SELECT * from table_BBB;
```

`sqlt` command-line interface tries to strike easy to use and being poweful at sametime.

Or multiple files, 

```
./sqlt.py ./examples/1.sql ./examples/2.sql  -x A=s3://AA/A B=s3://BB/B C=hdfs://CC/C
```

Output:

```SQL
---------------------------------------------------------
-- generated by sqle (https://github.com/tly1980/sqle) --
---------------------------------------------------------
-- with xargs: A=s3://AA/A; B=s3://BB/B; C=hdfs://CC/C

-- sql_1 from: ./examples/1.sql
CREATE temporary table TBL_A
USING com.databricks.spark.csv
OPTIONS (path "s3://AA/A", header "true");

CREATE temporary table TBL_B
USING com.databricks.spark.csv
OPTIONS (path "s3://BB/B", header "true");

-- sql_2 from: ./examples/2.sql
CREATE temporary table TBL_C
USING com.databricks.spark.csv
OPTIONS (path "hdfs://CC/C", header "true");
```

Or you can simply specify a folder, it would just crawl through it

```
./sqlt.py ./examples/  -x A=s3://AA/A B=s3://BB/B C=hdfs://CC/C
```

Output:

```SQL
---------------------------------------------------------
-- generated by sqle (https://github.com/tly1980/sqle) --
---------------------------------------------------------
-- with xargs: A=s3://AA/A; B=s3://BB/B; C=hdfs://CC/C

-- sql_1 from: ./examples/1.sql
CREATE temporary table TBL_A
USING com.databricks.spark.csv
OPTIONS (path "s3://AA/A", header "true");

CREATE temporary table TBL_B
USING com.databricks.spark.csv
OPTIONS (path "s3://BB/B", header "true");

-- sql_2 from: ./examples/2.sql
CREATE temporary table TBL_C
USING com.databricks.spark.csv
OPTIONS (path "hdfs://CC/C", header "true");
```

## Installation

`sqlt.py` has no dependencies with other third party lib, so you can simply just download the [sqlt.py](https://raw.githubusercontent.com/tly1980/sqlt/master/sqlt.py) file. Please don't forget chmod.
I developed it in python 2.7 environment, and it should run with 2.6 (although I haven't test it), which introduced [string-formatting]( https://docs.python.org/2/library/string.html#string-formatting )




