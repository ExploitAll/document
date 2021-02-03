# elasticseach学习笔记

## 一，Mapping映射

### 1，参数

#### 1.1 	index_options

用于控制倒排索引的生成。目前有四种类型的控制。该参数只用于text字段类型。

| 字段名   | 说明                                             |
| -------- | ------------------------------------------------ |
| docs     | 只记录doc id                                     |
| freqs    | doc id还有词频 term frequences都会被记录         |
| postions | 默认选项，doc id,词频。出现位置会被记录          |
| offsets  | doc id, 词频，出现位置，首字符与终止字符的偏移量 |



#### 1.2 	null_value

用于表示字段的值为null,只能应用于keyword字段类型，通过该字段值可以实现对null字段值的搜索，例如设置name字段属性的null_value属性为nil，那么通过name:nil的就可以搜索出name字段值为null的文档数据了。

#### 1.3 	copy_to

通过该属性可以将当前字段的内容复制到目标字段中，从而可以实现某一个目前字段内容的汇总，可以实现_all【汇总文档所有字段内容的特点】



## 二，索引

### 1，索引的重命名

​	可以通过发送一个post请求到_aliases, 来为当前的索引进行重命名，其中还可以指定过滤条件，例如可以为带日期的索引制定一个带current名字的索引，从而每天只需要访问一个索引名字就可以访问到当天的所有数据。请求实例如下：

![image-20210127204903324](C:\Users\pblov\AppData\Roaming\Typora\typora-user-images\image-20210127204903324.png)

## 三，搜索

### 1，分词器

分词器主要由三个模块构成。

- character filters 用于对原始的字符流进行操作，可以增加，修改，删除其中的字符，例如可以将其中的中文数字修改为阿拉伯数字，或者是去除HTML中的一些标签值。
- tokenizers 将character filters处理过的字符流切分成独立的标记符号。
- token filters 接收tokenizers 中切分的标记流，可以对其中的标记进行增加，删除，修改的操作。

### 2，查询类型

#### 2.1 	constant_score

只返回查询的结果而不进行计算相关性得分，能够提升一定的性能。

#### 2.2 	上下文

查询主要分为两种上下文，分别是Query context与Filter context。两者的区别在于是否计算相关性得分，对于Query context会计算文档的相关性得分，但是对于Filter context只会筛选文档而不会计算文档的相关性得分。

### 3，相关性

ES 5之前默认的相关性算法为TF-IDF算法，现在采用的算法为BM 25

#### 3.1 	TF-IDF算法简介

TF: 表示词频，计算检索词在全部文档中出现的频率。

IDF: inverse document frequency 通过检索词所在文档数与总文档数计算而来，计算公式如下：=log(全部文档数 / 检索词出现过的文档数)

TF-IDF算法将单纯的TF累加变成了加权累加的计算方式。计算示例如下：

TF(检索词-大海) * IDF(检索词-大海) + TF(检索词-小偷)* IDF(检索词-小偷) + TF(检索词-黑客)* *IDF(检索词-黑客)

####  3.2	BM 25算法简介



## 三，集群

## 四，调优