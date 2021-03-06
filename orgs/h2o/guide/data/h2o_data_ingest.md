# H2O Data Ingest #

### [Supported file format](#supportedfileformat) ###
H2O supports the following different file types:
 - CSV (delimited) files
 - ORC
 - SVMLight
 - ARFF
 - XLS
 - XLSX
 - Avro version 1.8.0 (without multifile parsing or column type modification)
 - Parquet (Some of the complex nested parquet files may not be read properly)

### [Supported Data Types](#supporteddatatypes) ###
H2O supports the following data types:
- Numeric
- String
- Enum
- Time
- UUID

Note: H2O will also mark some of the data as invalid or unknown if could not set any of the above data type.

### [Supported Data Source](#supporteddatasources) ###
H2O supports the following data source natively:
- Local File System
- Web based files hosted on https/https
- S3 (Supports both s3 and s3n)
- HDFS 
- JDBC (SQL source connected with JDBC driver 0.42 or higher )

H2O also supports the following data sources with some external libraries:
- Google Cloud Store Object Store files system using Google’s cloud storage connector (gcs-connector-latest-hadoop2.jar)
- IBM Swift Object Storage using IBM HDFS Driver (hadoop-openstack.jar)
- Alluxio data storage source using Alluxio client library (alluxio-core-client-*-jar-with-dependencies.jar)

### [File(s) Import in H2O](#filesimportinH2O) ##

H2O has on single API to ingest all supported kinds of data format from all supported data source. In the following API, the path can be any of the following:
- Local file system i.e.
  - C:\>file_location
  - /Users/avkashchauhan/learn/customers/prostate.csv
- File at your HDFS
  - hdfs://cloudera.hadoop.com:/user/avkash/prostate.csv
- File at HTTP/HTTPS
 - http://s3.amazonaws.com/h2o-public-test-data/smalldata/flow_examples/arrhythmia.csv.gz
- Files at S3 location
 - s3n://mybucket/myDir
- Files are Azure blob storage:
 - wasb://mybucket@myaccount.blob.core.windows.net/myDir 

#### Here is how you will use the file import API in various ways: ####

```
FLOW
importFiles
# R
h2o.importFiles(path="path to your file", destination_frame = "", parse = TRUE, header = NA,
  sep = "", col.names = NULL, col.types = NULL, na.strings = NULL,
  decrypt_tool = NULL)
# Python
h2o.import_files(path="path to your files", destination_frame=None, parse=True, header=0, sep=None, col_names=None, col_types=None, na_strings=None, pattern=None)
```


### [Ingesting data in H2O](#IngestingDataInH2O) ###
FLOW
 - In the FLOW cell type "importFiles" and select "SHIFT + ENTER" / Or click "RUN" button at the top MENU
 - In the open cell insert the file name
 - You can add multiple files with same data format into same cell
 - The Actual PARSE action is two step
 - First Step: Once all the files are selected you select "Parse all files" which just reads the header and other info
 - Second Step: The actual parse action happens and files are read from the source and loaded into H2O memory
 - This is how all the above steps looks like:
 ![](https://github.com/Avkash/mldl/blob/master/images/flow-file-ingest.png?raw=true)

Python
```  
import h2o
h2o.import_file(path=None, destination_frame=None, parse=True, header=0, sep=None, col_names=None, col_types=None, na_strings=None, pattern=None)
``` 
R
```  
library(h2o)
## -- You can pass a file in the path of this API
h2o.importFile(path, destination_frame = "", parse = TRUE, header = NA, sep = "", col.names = NULL, col.types = NULL, na.strings = NULL, decrypt_tool = NULL)

## -- You can pass a folder in the path of this API
h2o.importFolder(path, pattern = "", destination_frame = "", parse = TRUE,  header = NA, sep = "", col.names = NULL, col.types = NULL,  na.strings = NULL, decrypt_tool = NULL)
```      
Scala
```   
val h2oContext = H2OContext.getOrCreate(sc)
import h2oContext._
import h2oContext.implicits._
import java.io.File
val prostateData = new H2OFrame(new File("/Users/avkashchauhan/src/github.com/h2oai/sparkling-water/examples/smalldata/prostate.csv"))
```

### [Importing Multiple Files](#ImportingMultipleFiles) ###
 
#### FLOW ####
- First you will call importFiles command in the FLOW cell.
- In the open input box, you can add multiple files
- Select all the files you want to add into single data frame
- You can set the dataframe key into ID input box
- Now parse the files as you perform file input operation

#### Python: ####
You can use same h2o.import_file() API where you pass folder as path and patten to select type of files choosen from the given path. 
```
h2o.import_file(path="/Users/avkashchauhan/learn/customers/",  pattern="prostate_/*.csv" )
##
Note: Above API will select all the files name prostate_*.csv from the give path. 
```

#### R ####
```
h2o.importFolder(path="/Users/avkashchauhan/learn/customers/",  pattern="prostate_/*.csv" )
##
Note: Above API will select all the files name prostate_*.csv from the give path. 

```
#### Scala ####
You just need to Scala/Java support file filter to select the files from the folder and then create an H2O Dataframe.
```
val h2oContext = H2OContext.getOrCreate(sc)
import h2oContext._
import h2oContext.implicits._
import java.io.File
val prostateData = new H2OFrame(new File("/Users/avkashchauhan/learn/customers/prostate_*.csv"))
##-----------
NEED AN EXAMPLE
##------------
```

   
### [Uploading a File](#UploadingFileInH2O) ###
Sometime you may want to upload a file from local files system to H2O memory and for that reason you will use H2O file upload functionalties. 

#### FLOW ####
You just need to select the "Upload File" option from the "Data" menu in FLOW as shown below and select the file from local file system:
![](https://github.com/Avkash/mldl/blob/master/images/flow-upload-file.png?raw=true)
#### R ####
```
h2o.uploadFile(path, destination_frame = "", parse = TRUE, header = NA,  sep = "", col.names = NULL, col.types = NULL, na.strings = NULL,  progressBar = FALSE, parse_type = NULL, decrypt_tool = NULL)
```

#### Python ####
```
h2o.upload_file(path, destination_frame=None, header=0, sep=None, col_names=None, col_types=None, na_strings=None)
```
#### Scala ####
You can use Scala/Java API to read files from local file system and then create H2O frame in memory.



