-----------------------------
chapter 11 分组操作
-----------------------------

#### apply函数
函数说明：

apply(matrix, 1/2（维度）, 处理函数)
参数１：　matrix: 表示该参数只能为矩阵，若传递的是其他数据类型，则会被转为矩阵形式
参数２：　指应用函数的维度，１表示对行进行处理，２表示对列进行处理
参数３：　处理数据的函数，如sum, mean
``` R
theMatrix <- matrix(1:9, nrow = 3)
#       [,1] [,2] [,3]
# [1,]    1    4    7
# [2,]    2    5    8
# [3,]    3    6    9

apply(theMatrix, 1, sum) # 对行进行累加运算
# [1] 12 15 18

apply(theMatrix, 2, sum)　# 对列进行累加运算
# [1]  6 15 24
```
使用<code>rowSums</code><code>colSums</code>方法实现与上面这段代码等价的操作
``` R
# same as
rowSums(theMatrix)
# [1] 12 15 18
colSums(theMatrix)
# [1]  6 15 24
```
当矩阵中含有缺失值时(NA)，可通过设置额外参数参数<code>na.rm</code>来处理含缺失值的矩阵，如
``` R
theMatrix[2, 1] <- NA
apply(theMatrix, 1, sum)
apply(theMatrix, 1, sum, na.rm = TRUE)
``` 
设置<code>na.rm = TRUE</code>后处理函数会过滤掉ＮＡ值

#### lapply和sapply
##### lapply
函数说明：

lapply(vector/list, func)
参数１：　可以是向量或列表
参数２：　处理函数（如：加减乘除）

lapply的工作原理是将处理函数应用到一个列表的所有元素，并将结果作为列表返回,如：
``` R
theList <- list(A = matrix(1:9, 3), B = 1:8, c = matrix(1:4, 2))
lapply(theList, sum)
# result: 
# $A
# [1] 45
# 
# $B
# [1] 36
# 
# $c
# [1] 10
```
##### sapply
sapply函数和lapply函数的区别在返回值类型不同，sapply函数返回值类型为向量
``` R
sapply(theList, sum)

# A  B  c 
# 45 36 10
```
##### mapply
mapply函数可以将处理函数运用到多个列表的每一个元素中，　如
``` R
firstList <- list(A = matrix(1:16, 4), B = matrix(1:9, 3))
secondList <- list(A = matrix(1:9, 3), B = matrix(1:16, 8) )

simpleFunc <- function(x, y) {
  NROW(x) + NROW(y) # NROW函数返回对象的行数
}
mapply(simpleFunc, firstList, secondList)
＃　result: 
# A  B 
# 7 11 
```
