# Set operations in Hive: Union, Intersect, Size and Jaccard Index
## Add the UDF jar file
```
add jar hdfs:///hiveudfs/brickhouse.jar;
```

## Drop existing functions if exist, so that no conflicts
```
drop function if exists combine_unique;
drop function if exists array_intersect;
drop function if exists combine;
```

## If you want the functions loaded temporary in this session only
```
CREATE TEMPORARY FUNCTION combine AS 'brickhouse.udf.collect.CombineUDF';
CREATE TEMPORARY FUNCTION combine_unique AS 'brickhouse.udf.collect.CombineUniqueUDAF';
CREATE TEMPORARY FUNCTION array_intersect AS "brickhouse.udf.collect.ArrayIntersectUDF";
```

## Or you can make it permanently
```
CREATE FUNCTION combine AS 'brickhouse.udf.collect.CombineUDF';
CREATE FUNCTION combine_unique AS 'brickhouse.udf.collect.CombineUniqueUDAF';
CREATE FUNCTION array_intersect AS "brickhouse.udf.collect.ArrayIntersectUDF";
```
## This is my test query
```
set hive.cli.print.header=true;
select combine(array('a','b','c'), array('b','c','d')) as combine
, combine_unique(combine(array('a','b','c'), array('b','c','d'))) as union
, array_intersect(array('a','b','c'), array('b','c','d')) as intersect
, size(array_intersect(array('a','b','c'), array('b','c','d')))/size(combine_unique(combine(array('a','b','c'), array('b','c','d')))) as jaccard_similarity;
```


---
## Reference 
[blog of  Kittipat Kampa] (https://sites.google.com/site/kittipat/data-science/setoperationsinhiveunionintersectsizeandjaccardindex)

[Stack Overflow](http://stackoverflow.com/questions/21578477/array-intersect-hive?noredirect=1&lq=1)
