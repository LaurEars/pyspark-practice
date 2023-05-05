# DBC files

DBC stands for Databricks Cloud. It's used to distribute code, dependencies, and configuration files.

## Creating DBC files

Use `pyspark.dbutils.DBUtils` to create zip file

```python
# import the dbutils module
from pyspark.dbutils import DBUtils

# create an instance of the dbutils module
dbutils = DBUtils(spark.sparkContext)

# create a list of files and directories to include in the DBC file
files = ['/FileStore/dependencies/my_module.py',
         '/FileStore/dependencies/my_package/',
         '/FileStore/resources/my_config.cfg']

# create a DBC file from the list of files and directories
dbutils.fs.createZip('/FileStore/my_app/my_app.dbc', files)
```

## Handling DBC files

How to open a DBC file and dynamically determine modules to import

```python
import zipfile

# open the DBC file as a zip file
with zipfile.ZipFile("/path_to/my_app.dbc", "r") as zip_file:

    # list the files in the DBC file
    files = zip_file.namelist()

    # finds one Python module - hardcode this if known
    python_module_file = None
    for file_name in files:
        if file_name.endswith(".py"):
            python_module_file = file_name
            break

    # dynamically import the Python module
    if python_module_file is not None:
        zip_file.extract(python_module_file)
        # remove ".py"
        module_name = python_module_file[:-3]
        module = __import__(module_name)
```

But if you're already in a spark session, you can use it like this

```python
from pyspark.sql import SparkSession

# create session
spark = SparkSession.builder.appName("my_app").getOrCreate()

# add the DBC file to the Python path
spark.sparkContext.addPyFile("/path_to/my_app.dbc")

# import module - need to know module name for this
from my_module import my_function

# use the module in your PySpark application
df = spark.read.csv("/path/to/my_data.csv")
result = my_function(df)

# stop the SparkSession
spark.stop()
```

## Submitting DBC files

When you finally submit the code, you need an `app.py` to be defined

```python
from my_module import my_function
from pyspark.sql import SparkSession

if __name__ == "__main__":
    spark = SparkSession.builder.appName("MyApp").getOrCreate()
    # grab csv if present
    df = spark.read.csv("path_to/data.csv", header=True, inferSchema=True)
    result = df.select(my_function(df["column"]))
    result.show()
    spark.stop()

```

```bash
spark-submit --py-files /path_to/my_app.dbc my_app.py
```

Instructions for loading dbc files (import)
[On the web UI](https://docs.databricks.com/notebooks/notebook-export-import.html)

[How to load pyspark locally](https://medium.com/swlh/pyspark-on-macos-installation-and-use-31f84ca61400)
