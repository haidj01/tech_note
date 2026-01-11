1. Table Creation
```
from pyspark.sql.types import *
from delta.tables import *

delta_table = (
    DeltaTable.create(spark)
    .tableName("exampleDB.countries")
    .addColumn("id", dataType = LongType(), nullable=False)
    .addColumn("countries", dataType= StringType(), nullable=False)
    .addColumn("capital", dataType=StringType(), nullable=False )
    .execute()
)
```
2. Insert
```

data = [
    (1, "United Kingdom", "London")
    (2, "Canada", "Toronto")
]

schema = ["id","countries","capital"]

df = spark.createDataFrame(data, schema =schema)
df.write.format("delta").insertInto("exampleDB.countries")

```
3. Insert(Append)
```
data = [
    (3, "Korea", "Seoul")
]
schema = ["id", "countries","capital]
df = spark.createDataFrame(data, schema=schema)
df.write.format("delta").mode("append").saveAsTable("exampleDB.countries")
```

4. Insert(Overwrite)
```
data = [
    (4, "China", "Beijing")
]
schema = ["id", "countries","capital"]
df = spark.createDataFrame(data, schema=schema)
df.write.format("delta).mode("overwrite").saveAsTable("exampleDB.countries")
```

5. Select
```
from delta.tables import DeltaTable

delta_table = DeltaTable.forName(spark,"exampleDB.countries" )
delta_table.toDF().display()

```

6. Filter
```
from delta.tables import DeltaTable
delta_table = DeltaTable.forName(spark, "exampledb.countries")
delta_table.toDF().filter("capital='seoul'").show(truncate=False)
```

