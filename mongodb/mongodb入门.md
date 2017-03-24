mongodb入门

## mongodb简介
MongoDB是开源文档型数据库。

### 文档 (Documents)
在MongoDB中，一条记录是一个文档，文档结构由数量不等的键值对组成，类似于JSON对象。键的值可能包含其它文档、数组、或文档数组。
```
{
   "_id" : ObjectId("54c955492b7c8eb21818bd09"),
   "address" : {
      "street" : "2 Avenue",
      "zipcode" : "10075",
      "building" : "1480",
      "coord" : [ -73.9557413, 40.7720266 ]
   },
   "borough" : "Manhattan",
   "cuisine" : "Italian",
   "grades" : [
      {
         "date" : ISODate("2014-10-01T00:00:00Z"),
         "grade" : "A",
         "score" : 11
      },
      {
         "date" : ISODate("2014-01-16T00:00:00Z"),
         "grade" : "B",
         "score" : 17
      }
   ],
   "name" : "Vella",
   "restaurant_id" : "41704620"
}
```

### 集合 (Collections)
MongoDB把文档存储在集合中。集合类似于关系型数据库中的表。与表不同的是，集合不要求它的集合符合相同的格式。

## windows中安装MongoDB
- 下载并安装MongoDB。下载时注意选择合适版本的msi，安装时安装到默认路径。
- 创建存放数据库和log的目录，目录位置任意指定，如在D盘创建"D:\mongodb\db"和"D:\mongodb\db"两个文件
- 创建配置文件。配置文件位置任意，如"D:\mongodb\mongod.cfg"。配置文件必须设置systemLog.path。
```
systemLog:
    destination: file
    path: d:\mongodb\log\mongod.log
storage:
    dbPath: d:\mongodb\db
```
- 创建MongoDB服务。如果创建服务时，提示服务已存在，先使用sc命令删除服务。
```
sc.exe create MongoDB binPath= "\"C:\Program Files\MongoDB\Server\3.2\bin\mongod.exe\" --service --config=\"D:\mongodb\mongod.cfg\"" DisplayName= "MongoDB" start= "auto"
```
- 启动MongoDB服务。启动的服务默认运行在localhost:27017   
`net start MongoDB`
- 停止或删除MongoDB服务   
停止服务   
`net stop MongoDB`   
删除服务，先停止服务，再使用下面命令删除服务   
`sc.exe delete MongoDB`   
- 连接MongoDB服务   
`mongo`

## 导入数据到数据库
导入文件中数据到指定服务器端口指定数据库指定集合中。   
`mongoimport --db test --collection restaurants --drop --file ~/downloads/primer-dataset.json`   
--db 指定导出数据库   
--collection 指定导出集合   
--drop 如果test数据库已存在restaurants集合，操作会先删除集合，再添加   
--file 指定源数据文件，一般是json文件   
--host 指定服务所在机器ip地址   
--port 指定服务所在机器端口   

## 插入数据
在MongoDB中，可以使用`insert()`方法向集合中添加文档。如果向一个不存在的集合中添加文档，MongoDB会先自动创建集合。

### 前提
- 1. 连接到MongoDB服务
- 2. 切换到指定数据库   
`use test`

### 插入文档
向名为restaurants的集合插入一条文档
```
db.restaurants.insert(
   {
      "address" : {
         "street" : "2 Avenue",
         "zipcode" : "10075",
         "building" : "1480",
         "coord" : [ -73.9557413, 40.7720266 ]
      },
      "borough" : "Manhattan",
      "cuisine" : "Italian",
      "grades" : [
         {
            "date" : ISODate("2014-10-01T00:00:00Z"),
            "grade" : "A",
            "score" : 11
         },
         {
            "date" : ISODate("2014-01-16T00:00:00Z"),
            "grade" : "B",
            "score" : 17
         }
      ],
      "name" : "Vella",
      "restaurant_id" : "41704620"
   }
)
```
如果插入的文档中没有包含_id字段，mongo会自动向文档中添加该自段，并将值设置为一个生成的 ObjectId。

## 查询数据
在MongoDB中，使用`find()`方法从集合中查询数据。查询会返回集合中的所有文档或符合指定过滤规则的文档。查询规则存放在一个文档中，并作为参数传递给`find()`方法。`find()`方法返回一个cursor，cursor是一个可遍历对象。

### 查询集合中所有文档
调用`find()`方法不传入条件文档参数，会返回集合中的所有文档。
```
db.restaurants.find()
```

### 相等条件查询
使用相等条件查询，使用以下格式：
```
{ <field1>: <value1>, <field2>: <value2>, ... }
```
如果field是顶层field，且不是嵌入的文档或数组，field名字可以加双引号，也可以省略不加。   
如果field是嵌入的文档或数组，使用dot notaion访问field，且必须为这种field名称加上双引号。

#### 使用顶级field查询
下面的操作查询borough字段等于Manhattan的文档。   
```
db.restaurants.find( { "borough": "Manhattan" } )
``

#### 使用嵌入文档中的字段查询
使用嵌入文档中的字段查询，需要使用dot notaion。使用dot notaion时，需要在整个字段名称上加上双引号。
```
db.restaurants.find( { "address.zipcode": "10075" } )
```

#### 使用数组中字段查询
grades数组包含的元素都是文档，使用这种嵌入文档中的字段查询，需要使用dot notation，并加上双引号。
```
db.restaurants.find( { "grades.grade": "B" } )
```

### 运作符条件查询
MongoDB支持使用运算符设置查询条件。这种查询条件格式如下，一般是在value位置设置：
```
{ <field1>: { <operator1>: <value1> } }
```

#### 大于 ($gt)
```
db.restaurants.find( { "grades.score": { $gt: 30 } } )
```

#### 小于 ($lt)
```
db.restaurants.find( { "grades.score": { $lt: 10 } } )
```

### 组合查询
可以把多个查询条件使用AND或OR的方式组合起来查询。

#### AND
与查询时，把多个查询条件放在同一个文档中，多个查询条件之间以逗号分隔，最后把文档传给find()方法作为查询条件。
```
db.restaurants.find( { "cuisine": "Italian", "address.zipcode": "10075" } )
```

#### OR
或查询时。多个查询条件分别是一个文档，然后把这些文档放在一个数组中。最外层作为查询参数的文档，使用$or作为field，值使用多个查询条件组成的数组。最后把文档传给find()方法。
```
db.restaurants.find(
   { $or: [ { "cuisine": "Italian" }, { "address.zipcode": "10075" } ] }
)
```

### 为查询结果排序
为了给查询结果排序，在find()方法后追加sort()方法。传递给sort()方法一个文档，文档中的字段表示要排序的字段，字段值为排序情况，1表示正序，-1表示倒序。   
下例中，查询集合中所有文档，然后先用正序为borough字段排序，然后对每个borough，再用正序为"address.zipcode" 字段排序。
```
db.restaurants.find().sort( { "borough": 1, "address.zipcode": 1 } )
```
