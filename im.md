im

- [mongdb](./3-24 mongo.md)

  - ```java
    //查找最早的记录并删除
    ImCus imCus = mongoTemplate.findAndRemove(new Query().with(Sort.by(Sort.Order.asc("timestamp"))), ImCus.class);
    //保存
    mongoTemplate.save(new ImCus().setCustomerService(imCus.getCustomerService()).setUser(ums.getUsername()).setTimestamp(System.currentTimeMillis()), "im_cus");
    ```

  - ​	

```
MongoRepository<Product,String>
//按某字段最大值取一个
findFirstByOrderByIdDesc
```

- 前台方式

  缺省情况下，当为一个集合创建索引时，这个操作将阻塞其他的所有操作。即该集合上的无法正常读写，直到索引创建完毕

  任意基于所有数据库申请读或写锁都将等待直到前台完成索引创建操作

  后台方式

  将索引创建置于到后台，适用于那些需要长时间创建索引的情形

  这样子在创建索引期间，MongoDB依旧可以正常的为提供读写操作服务

  等同于关系型数据库在创建索引的时候指定online，而MongoDB则是指定background

  其目的都是相同的，即在索引创建期间，尽可能的以一种占用较少的资源占用方式来实现，同时又可以提供读写服务

  后台创建方式的代价：索引创建时间变长
  

- rabbitmq
  
  - [基础概念](./rabbitmq)
  
  - ##### [Spring Boot整合RabbitMQ详细教程](https://blog.csdn.net/qq_38455201/article/details/80308771)
  
  - 
  
  - 
  
- Redis
  - ##### [RedisTemplate中hash类型的使用](https://www.jianshu.com/p/569d19fa356d)
    
    ```java
     //以hash类型存,如果不存在就保存 A_B 
    redisTemplate.opsForHash().putIfAbsent("IM_CHAT", fromAccount + "_" + to, "0");
    ```
    
  - ```java
     //定时任务，每30秒把key取出来，分割成form，to，把from对to的消息已读，
    @Scheduled(fixedDelay = 30 * ONE_SECOND_MILLIS)
        @Transactional(rollbackFor = Exception.class)
        public void setUnReadMSG() {
            Set<String> keys = redisTemplate.opsForHash().keys("IM_CHAT");
            updateMsgs(keys);
        }
    
        public void updateMsgs(Set<String> keys) {
            List<Pair<Query, Update>> updateList = new ArrayList<>();
    
            keys.forEach(key -> {
                String[] strList = key.split("_");
                String fromAccount = strList[0];
                String to = strList[1];
                redisTemplate.opsForHash().delete("IM_CHAT", key);
                Query query = new Query();
                query.addCriteria(Criteria.where("msg.fromAccount").is(to)
                        .and("msg.to").is(fromAccount)
                        .and("read").is(false));
                Update update = new Update();
                update.set("read", true);
                Pair<Query, Update> updatePair = Pair.of(query, update);
                updateList.add(updatePair);
            });
          //批量update
            DbUtil.batchUpdatesContinueOnError(mongoTemplate, ImMsg.class, updateList);
        }
    ```
  
  - 

