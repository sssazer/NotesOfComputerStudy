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



