### 启动

在/bin目录创建data/db文件夹

```bash
 ./mongod --dbpath '/usr/local/mongodb/bin/data/db'//启动服务

 ./mongo //启动数据库
```

## 数据库

### 创建数据库

创建并切换到指定数据库

```
use DATABASE_NAME
```

### 查看所有数据库

```sql
show dbs
```

### 删除数据库

删除当前数据库

```
db.dropDatabase()
```

## 集合

### 集合删除

```
db.collection.drop()
```

### 集合创建

```
db.createCollection(name, options)
```

- name: 要创建的集合名称
- options: 可选参数, 指定有关内存大小及索引的选项

### 删除集合

```
db.collection.drop()
```

## 文档

### 插入文档

MongoDB 使用 insert() 或 save() 方法向集合中插入文档

```
db.COLLECTION_NAME.insert(document)

>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})

> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});
```

### update() 方法

update() 方法用于更新已存在的文档。语法格式如下：

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)

>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})

```

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别。

### save() 方法

save() 方法通过传入的文档来替换已有文档。语法格式如下：

objectId一样

```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

### remove

删除文档

```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明：**

- **query** :（可选）删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- **writeConcern** :（可选）抛出异常的级别。

### 查询数据

```
db.collection.find(query, projection)
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

##### or

```
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

##### and

```
>db.col.find({key1:value1, key2:value2}).pretty()
```

##### 大于操作符 - $gt

如果你想获取 "col" 集合中 "likes" 大于 100 的数据，你可以使用以下命令：

```
db.col.find({likes : {$gt : 100}})
```

##### 大于等于操作符 - $gte

如果你想获取"col"集合中 "likes" 大于等于 100 的数据，你可以使用以下命令：

```
db.col.find({likes : {$gte : 100}})
```

##### 小于操作符 - $lt

如果你想获取"col"集合中 "likes" 小于 150 的数据，你可以使用以下命令：

```
db.col.find({likes : {$lt : 150}})
```

##### 小于等于操作符 - $lte

如果你想获取"col"集合中 "likes" 小于等于 150 的数据，你可以使用以下命令：

```
db.col.find({likes : {$lte : 150}})
```

##### 使用 (<) 和 (>) 查询 - $lt 和 $gt

如果你想获取"col"集合中 "likes" 大于100，小于 200 的数据，你可以使用以下命令：

```
db.col.find({likes : {$lt :200, $gt : 100}})
```

##### 操作符 - $type 实例

如果想获取 "col" 集合中 **title 为 String** 的数据：

```
db.col.find({"title" : {$type : 2}})
或
db.col.find({"title" : {$type : 'string'}})
```

##### MongoDB Skip() 方法

我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。

```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "Java 教程" }
>
```

##### MongoDB sort() 方法

在 MongoDB 中使用 sort() 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而 -1 是用于降序排列。

```
>db.COLLECTION_NAME.find().sort({KEY:1})
```

## MongoDB 聚合

MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值,求和等)，并返回计算后的数据结果。有点类似sql语句中的 count(*)。

#### aggregate() 方法

MongoDB中聚合的方法使用aggregate()。



```sql
通过name分组并计算个数
> db.sharfine.aggregate([{$group : {_id : "$name",num : {$sum : 1}}}])
{ "_id" : "赵六", "num" : 1 }
{ "_id" : "王五", "num" : 1 }
{ "_id" : "李四", "num" : 2 }
{ "_id" : "张三", "num" : 2 }
通过age分组并计算个数
> db.sharfine.aggregate([{$group : {_id : "$age",num : {$sum : 1}}}])
{ "_id" : "26", "num" : 1 }
{ "_id" : "25", "num" : 1 }
{ "_id" : "24", "num" : 1 }
{ "_id" : "21", "num" : 2 }
{ "_id" : "23", "num" : 1 }
通过name分组,并选出age最大值
> db.sharfine.aggregate([{$group : {_id : "$name",num : {$max : "$age"}}}])
{ "_id" : "赵六", "num" : "26" }
{ "_id" : "王五", "num" : "25" }
{ "_id" : "李四", "num" : "24" }
{ "_id" : "张三", "num" : "23" }
通过name分组,并选出age最小值
> db.sharfine.aggregate([{$group : {_id : "$name",num : {$min : "$age"}}}])
{ "_id" : "赵六", "num" : "26" }
{ "_id" : "王五", "num" : "25" }
{ "_id" : "李四", "num" : "21" }
{ "_id" : "张三", "num" : "21" }
> 

通过name分组并计算个数
select by_user, count(*) from mycol group by by_user
```

| 表达式    | 描述                                           | 实例                                                         |
| :-------- | :--------------------------------------------- | :----------------------------------------------------------- |
| $sum      | 计算总和。                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg      | 计算平均值                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min      | 获取集合中所有文档对应值得最小值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max      | 获取集合中所有文档对应值得最大值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push     | 在结果文档中插入值到一个数组中。               | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}]) |
| $addToSet | 在结果文档中插入值到一个数组中，但不创建副本。 | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}]) |
| $first    | 根据资源文档的排序获取第一个文档数据。         | db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}]) |
| $last     | 根据资源文档的排序获取最后一个文档数据         | db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}]) |

- $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。

- $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。

- $limit：用来限制MongoDB聚合管道返回的文档数。

- $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。

- $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。

- $group：将集合中的文档分组，可用于统计结果。

- $sort：将输入文档排序后输出。

- $geoNear：输出接近某一地理位置的有序文档。

  

### mongodb集群

##### 复制

### 分片



## 整合springboot

### mongodb的document实体类	数据记录行/文档

```java
package com.abmatrix.im.main.database.model;

import com.abmatrix.im.common.database.model.base.BaseMongoModel;
import com.abmatrix.im.main.bean.req.YunXinMsgCopyReq;
import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.ToString;
import lombok.experimental.Accessors;
import org.springframework.data.mongodb.core.index.CompoundIndex;
import org.springframework.data.mongodb.core.index.CompoundIndexes;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.mapping.Document;

/**
 * @author bool
 */
@Data
@Accessors(chain = true)
@ToString(callSuper = true)
@EqualsAndHashCode(callSuper = true)
@Document(collection = "im_msg")
@CompoundIndexes({
        @CompoundIndex(
                name = "from_to_time",
                def = "{'msg.fromAccount': 1, 'msg.to': 1, 'msg.msgTimestamp': 1}",
                background = true
        )
})
public class ImMsg extends BaseMongoModel {

    @Indexed(unique = true, background = true)
    private String md5;

    private Boolean read;

    private YunXinMsgCopyReq msg;

}
```

```java
package com.abmatrix.im.common.database.model.base;

import lombok.Data;
import org.springframework.data.annotation.Id;

import java.io.Serializable;

/**
 * @author abm
 */
@Data
//提供id
public abstract class BaseMongoModel implements Serializable {

    private static final long serialVersionUID = 1L;

    @Id
    private String mongoId;
}
```

### test

```java
ImMsg msg = new ImMsg();
msg.setMd5(md5);
msg.setMsg(msgCopyReq);

else if("30".equals(eventType)){
    //收到回执抄送则把对应的消息状态改为true
    updateMsg(fromAccount,to);
}
mongoTemplate.save(msg);

return "ok";
}
//更新储存的未读消息为已读
public void updateMsg(String fromAccount,String to ){
        Query query = new Query();
        query.addCriteria(Criteria.where("msg.fromAccount").is(fromAccount));
        query.addCriteria(Criteria.where("msg.to").is(to));
        query.addCriteria(Criteria.where("msg.eventType").is("1"));
        query.addCriteria(Criteria.where("read").is(false));

        Update update = new Update();
        update.set("read", true);

        mongoTemplate.upsert(query,update,ImMsg.class);
    }
//根据传入用户数组获取对应未读条数数组
public List<String> getUnRead(String fromAccount, List<String>  otherUsersName){
        //存放未读消息数的数组
        List<String> counts = new ArrayList<>();
        //取出fromAccount和to去数据库查询两人聊天未读条数
        //按list查询
        otherUsersName.forEach(otherUserName -> {
            Query query = new Query();
            query.addCriteria(Criteria.where("msg.fromAccount").is(fromAccount));
            query.addCriteria(Criteria.where("msg.to").is(otherUserName));
            query.addCriteria(Criteria.where("read").is(false));
            query.addCriteria(Criteria.where("msg.eventType").is(1));
            Long count = mongoTemplate.count(query, ImMsg.class);
            counts.add(count.toString());
        });
```

## 实例

按timestamp升序排列，取出第一条

```java
List<ImCus> timestamp = mongoTemplate.find(new Query().with(Sort.by(Sort.Order.asc("timestamp"))).limit(1), ImCus.class);
//查询并删除排序后第一个
ImCus imCus = mongoTemplate.findAndRemove(new Query().with(Sort.by(Sort.Order.asc("timestamp"))), ImCus.class);
```

查询总数

```java
Query query = new Query();
query.addCriteria(Criteria.where("customerService").is(imUser.getUsername()));
long count = mongoTemplate.count(query, ImCus.class,"im_cus");
```













