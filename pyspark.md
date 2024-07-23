Sure! Here's a comprehensive PySpark cheat sheet to help you with data manipulation and analysis:

### Importing PySpark
```python
from pyspark.sql import SparkSession

# Create SparkSession
spark = SparkSession.builder \
    .appName("example") \
    .getOrCreate()
```

### Creating DataFrames
```python
# From a list of tuples
data = [("Alice", 25), ("Bob", 30), ("Charlie", 35)]
columns = ["name", "age"]
df = spark.createDataFrame(data, columns)

# From a CSV file
df = spark.read.csv('file.csv', header=True, inferSchema=True)

# From a JSON file
df = spark.read.json('file.json')
```

### Viewing Data
```python
# Show the first 20 rows
df.show()

# Show the first 5 rows
df.show(5)

# Show the schema
df.printSchema()

# Display DataFrame schema and summary
df.describe().show()
```

### Selecting Data
```python
# Select a single column
df.select("name").show()

# Select multiple columns
df.select("name", "age").show()

# Select rows by condition
df.filter(df.age > 25).show()

# Select rows by SQL expression
df.where("age > 25").show()
```

### Adding/Modifying Columns
```python
# Add a new column
df = df.withColumn("salary", df.age * 1000)

# Modify an existing column
df = df.withColumn("age", df.age + 1)
```

### Dropping Data
```python
# Drop columns
df = df.drop("salary")

# Drop rows with null values
df = df.na.drop()

# Fill null values
df = df.na.fill({"age": 0})
```

### Grouping and Aggregating Data
```python
# Group by a column and calculate the mean
df.groupBy("name").mean("age").show()

# Group by multiple columns
df.groupBy("name", "age").sum("salary").show()
```

### Joining DataFrames
```python
# Create another DataFrame
data2 = [("Alice", 50000), ("Bob", 60000)]
columns2 = ["name", "salary"]
df2 = spark.createDataFrame(data2, columns2)

# Inner join
df.join(df2, on="name", how="inner").show()

# Left join
df.join(df2, on="name", how="left").show()
```

### Working with Dates
```python
from pyspark.sql.functions import year, month, dayofmonth

# Convert a string column to date
df = df.withColumn("date", df["date"].cast("date"))

# Extract year, month, day
df = df.withColumn("year", year(df["date"]))
df = df.withColumn("month", month(df["date"]))
df = df.withColumn("day", dayofmonth(df["date"]))
```

### User-Defined Functions (UDFs)
```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Define a UDF
def to_upper(s):
    return s.upper()

# Register the UDF
to_upper_udf = udf(to_upper, StringType())

# Use the UDF
df = df.withColumn("name_upper", to_upper_udf(df.name))
```

### Saving DataFrames
```python
# Save DataFrame as CSV
df.write.csv('output.csv', header=True)

# Save DataFrame as JSON
df.write.json('output.json')

# Save DataFrame as Parquet
df.write.parquet('output.parquet')
```

### SQL Queries
```python
# Register DataFrame as a temporary table
df.createOrReplaceTempView("people")

# Run SQL query
sql_df = spark.sql("SELECT name, age FROM people WHERE age > 25")
sql_df.show()
```

### Basic Plotting with Pandas and Matplotlib
```python
import pandas as pd
import matplotlib.pyplot as plt

# Convert Spark DataFrame to Pandas DataFrame
pdf = df.toPandas()

# Plot using Pandas and Matplotlib
pdf.plot(kind='bar', x='name', y='age')
plt.show()
```

### Common Functions
```python
from pyspark.sql.functions import col, lit, expr

# Select specific columns
df.select(col("name"), col("age")).show()

# Add a new column with a constant value
df = df.withColumn("new_column", lit(100))

# Use expressions
df = df.withColumn("age_plus_one", expr("age + 1"))
```

This cheat sheet should provide you with a solid foundation for using PySpark for data analysis and manipulation!
