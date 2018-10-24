## 1.集合(set)

redis 中set分为有序集合(sorted set)和set(无序集合), 每个set可保存多个字符串元素, set类型中的元素值默认只保存唯一值, 重复元素仅保留一个

集合的操作有基本的增删改查, 还支持多个集合进行交,并,差集, 针对这些特性可用于不同的场景

## 2.基本操作命令

### 2.1添加元素(sadd)

>sadd key member [member ...]

``` shell
127.0.0.1:6379> sadd fruit 'apple' 'banana' 'orange'
(integer) 3
```

添加已存在的元素将会被忽略

### 2.2获取元素(smembers)

>smember key

``` shell
127.0.0.1:6379> smembers fruit
1) "banana"
2) "watermelon"
3) "orange"
4) "apple"
```

### 2.3删除元素(srem)

>srem key member [member ...]

``` shell
127.0.0.1:6379> srem fruit banana
(integer) 1
```

若删除的元素不在set中返回空

### 2.4统计元素个数(scard)

>scard key

``` shell
127.0.0.1:6379> scard fruit
(integer) 3
```

### 2.5判断元素是否存在(sismember)

>sismember key member

``` shell
127.0.0.1:6379> sismember fruit banana
(integer) 0
127.0.0.1:6379> sismember fruit apple
(integer) 1
```

接收单个元素

### 2.6随机获取元素(srandmember)

>srandmember key count

``` shell
127.0.0.1:6379> srandmember fruit 2
1) "watermelon"
2) "apple"
```

### 2.7随机弹出并删除元素

>spop key [count]

``` shell
127.0.0.1:6379> spop fruit 1
1) "apple"
127.0.0.1:6379> smembers fruit
1) "watermelon"
2) "orange"
```

与srandmember不同, spop操作将元素从集合中移除

### 2.8查看交集

> sinter key [key ...]

``` shell
127.0.0.1:6379> sinter fruit animal
1) "banana"
127.0.0.1:6379> smembers fruit
1) "banana"
2) "watermelon"
3) "orange"
127.0.0.1:6379> smembers animal
1) "banana"
2) "pig"
3) "fish"
```

### 2.9查看并集

>sunion key  [key ...]

``` shell
127.0.0.1:6379> sunion smembers animal
1) "banana"
2) "fish"
3) "pig"
127.0.0.1:6379> sunion fruit animal
1) "banana"
2) "watermelon"
3) "orange"
4) "pig"
5) "fish"
```

### 2.10查看差集

>sdiff key [key ...]

``` shell
127.0.0.1:6379> smembers fruit
1) "banana"
2) "watermelon"
3) "orange"
127.0.0.1:6379> smembers animal
1) "banana"
2) "pig"
3) "fish"
127.0.0.1:6379> sdiff fruit animal
1) "watermelon"
2) "orange"
```

与不存在的key取作差集, 结果为该key元素值

---
Author: leiyu
Date: 2018-10-24 