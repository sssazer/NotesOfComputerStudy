# 1. Quick Start(WordCount)

Spark Application入口为SparkContext，任何一个应用首先需要构建SparkContext对象

1. 创建SparkConf对象（configuration），用于设置SparkApplication的基本配置信息

   `conf = SparkConf().setAppName("appName").setMaster("mode")`

2. 基于SparkConf对象创建SparkContext对象，SparkContext对象是整个spark程序的入口，相当于main函数

   `sc = SparkContext(conf=conf)`

**WordCount程序**

```python
# 计算文件中每个单词出现的次数
# local[*]表示该spark在local模式运行，根据CPU核心数自动创建线程数
# AppName自己指定
conf = SparkConf().setMaster("local[*]").setAppName("WordCount")
sc = SparkContext(conf=conf)

file_rdd = sc.textFile("./wordCountData.txt") # 读取文件
words_rdd = file_rdd.flatMap(lambda line:line.split(" ")) # 根据空格切割为单词
words_with_one_rdd = words_rdd.map(lambda x:(x,1))
result_rdd = word_with_one_rdd.reduceByKey(lambda a, b:a+b)

```

# 2. RDD_API

创建SparkContext对象之后，使用`SparkContext.textFile()`读取一个文件，会返回一个pyspark.RDD对象，这个对象中有很多方法可以用于处理数据。

`file_rdd = sc.textFile("filepath")`

## 2.1 Action on RDD

Actions compute a result based on an RDD

### 2.1.1 collect()

return a Python list that contains all the elements in the RDD

```python
file_rdd = sc.parallelize(range(10),5) # 将一个list转化为rdd
all_data = rdd.collect() # 将rdd转化为Python list
print(all_data) # [0,1,2,3,4,5,6,7,8,9]
```

### 2.1.2 take(n)

Take the first num elements of the RDD,return a list

```python
rdd = sc.parallelize(range(10),5)
part_data = rdd.take(4) # [0,1,2,3]
```

### 2.1.3 first()

return the first element in RDD

```python
rdd = sc.parallelize(range(10),5)
first_data = rdd.first() # 0
```

### 2.1.4 reduce(function)

reduce(归纳) the elements of the RDD using the specified function,return the aggregated result(return type depend on the reduce function)

```python
rdd = sc.parallelize(range(10),5)
rdd.reduce(lambda x,y:x + y) # 45 这个函数的意思是把rdd中所有数据相加
```

### 2.1.5 foreach(function)

applies a function to all elements of the RDD, return none

```python
rdd = sc.parallelize(range(10),5)
rdd.foreach(lambda x : print(x)) # 将rdd中的所有数据打印
```

### 2.1.6 count()

返回RDD中的数据行数

## 2.2 Transformation on RDD

Transformations construct a new RDD from a previous RDD

### 2.2.1 map(function)

return a new RDD by applying a function to each element of the original RDD

```python
rdd = sc.parallelize(range(10),5)
rdd.map(lambda x : x*2).collect() # [0,2,4,6,8,10,12,14,16,18]
```

### 2.2.2 flatMap(function)

return a new RDD by first applying a function to all elements of the original RDD, then flattening the results (from multiple dimension to one dimension)

```python
rdd = sc.parallelize(["hello world", "hello pyspark"])
rdd.map(lambda x: x.split(" ")).collect() # [['hello', 'world'], ['hello', 'pyspark']]
rdd.flatMap(lambda x: x.split("")).collect() # ['hello', 'world', 'hello', 'pyspark']
```

### 2.2.3 filter(function)

return a new RDD containing only the elements that satisfy a predicate (return true in filter function)

```python
rdd = sc.parallelize(range(10),5)
rdd.filter(lambda x : x % 2 == 0).collect() # [0, 2, 4, 6, 8]
rdd.filter(lambda x : x > 5).collect() # [6,7,8,9]
```

### 2.2.4 distinct()

return a new RDD containing the distinct elements in this RDD

```python
rdd = sc.parallelize([1,1,2,2,3])
new_rdd = rdd.distinct().collect() # [1,2,3]
```

### 2.2.5 subtract(otherRDD)

return each value in self that is not contained in other(返回当前rdd和参数中rdd的差集)

```python
rdd1 = sc.parallelize([1,2,3,4,5])
rdd2 = rdd1.subtract(sc.parallelize([1,2,3]))
rdd2.collect() ## [4,5]
```

### 2.2.6 sortBy(keyfunc)

sorts this RDD by given keyfunc

```python
rdd = sc.parallelize([(1,2,3),(3,2,2),(4,1,1)])
rdd.sortBy(lambda x:x[2]).collect() # 根据每个tuple中下标为2的元素排序
# [(4,1,1),(3,2,2),(1)]
```

# 3. Spark SQL

SparkSQL可以使用类似SQL的语言操作结构化数据，需要依赖两个概念：

- DataFrame：
- RDD Schema

要在Python中使用SparkSQL，需要做以下配置：

```python
# Import Spark Library
from pyspark import SparkConf, SparkContext, SQLContext
from pyspark.sql.functions import *
# runtime execution context for spark-submit
appName = "Spark SQL Demo"
conf = SparkConf().setAppName(appName)
sc = SparkContext(conf=conf)
sqlContext = SQLContext(sc)
```

## 3.1 Operation On Dataframe

以下代码均以该表为例

| Name   | age  |
| ------ | ---- |
| Alice  | 25   |
| Bob    | 40   |
| Camile | 13   |
| David  | 35   |

### 3.1.1 Creating Dataframe from RDD

创建Dataframe需要一个RDD（保存数据）和schema（声明表头和每列对应的数据类型）

**通过.csv文件创建DataFrame**

```python
rdd_person = sc.textFile("person.csv")
rdd_table = rdd_person.map(lambda line : line.split(";"))

# 定义schema
schema = types.StructType([
	types.StructField('name', types.StringType(), True),
    types.StructField('age', types.StringType(), True)
])

# 在该RDD上创建DataFrame
df_persons = sqlContext.createDataFrame(rdd_table, schema)
df_person.show() # 打印该dataframe
```

**通过文本类型文件创建DataFrame**

```python
df_txt = sqlContext.read.\
format("csv").\  # 指定读入的文件类型是.csv
option("delimiter"," ").\ # 每一行是表格中一行，以指定分隔符将一行分为多列
schema(schema).\ # 指定每列的列名和数据类型
load("filepath") # 指定要导入文件的路径
```

### 3.1.2 Extract info from DataFrame

### 3.1.3 SQL Query

做SQL Query之前要先给dataframe起个名字，这个名字就是表名，用于FROM后面

`df_persons.registerTempTable("persons")`

**直接用SQL语句做查询**

```python
result = sqlContext.sql("SELECT name FROM persons") # 返回一个包含查询结果的新的DataFrame
for elem in result.collect():
    print(elem.name)
```

**用pySqark API做查询**

[API Docs](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html)

以下函数均返回一个新的DataFrame，且可以链式使用

- DataFrame.select(*cols) 查询操作

  `df_persons.select("*")`

- DataFrame.groupby(*cols) 分组操作

  `df_persons.groupby("name")`

- DataFrame.join(other, [on how]) 多表连接

  默认是内连接

  `df.join(df1,'name)`

  `df.join(df1, ['name', 'age'])`

  外连接：

  `df.join(df2, df.name == df2.name, 'outer')`

  `df.join(df2, [df.name == df3.name, df.age == df3.age], 'outer')`

- 排序操作

  
