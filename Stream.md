# Stream

```java
 		List<String> list = Lists.newArrayList("haha","dsas","sadasdasdsada","saad");
    List<Integer> list2 = Lists.newArrayList(10,3,4,6,3,2,7,8,101);
    List<Product> products = Lists.newArrayList(new Product().setId(10).setName("haha"),new Product().setId(10).setName("wawa"),new Product().setId(10).setName("lala"));
   //吧原stream元素全部加一百生成新的Stream
    list2.stream().mapToInt(item -> item+100).forEach(System.out::println);
    //吧原stream元素全部加String成新的Stream
    list.stream().map(item ->item+"sssss").forEach(System.out::println);
    list2.stream().mapToDouble(item ->item+2).forEach(System.out::println);
    //map可以操作所有对象（可以转变类型）
    products.stream().map(item -> item.setName("无敌")).forEach(System.out::println);
    //peek对所有对象进行消费（做一些操作）
    products.stream().peek(item -> item.setId(18)).forEach(System.out::println);
    //filter里面传入一个遍历出来的值，返回值为false则抛弃值,用collect转到另一个list
    List<String> longs = list.stream().filter(s -> s.length()<5).collect(Collectors.toList());
    //总数
    long count = list.stream().filter(s -> s.length()<5).count();
    //去重
    list2.stream().distinct().forEach(System.out::println);
    //不会遍历seed
    //Stream.iterate(list2,item -> item).limit(10).forEach(System.out::println);
    //按照seed生产元素
    Stream.iterate("aa", item -> item + 1).limit(10).forEach(System.out::println);
   //生产元素
    Stream.generate(Math::random);
    //skip跳过前几个，limit只要前几个
    //返回值为optional这个函数有两个参数，第一个参数是上次函数执行的返回值（也称为中间结果），第二个参数是stream中的元素，这个函数把这两个值
    // 相加，得到的和会被赋值给下次执行这个函数的第一个参数。要注意的是：**第一次执行的时候第一个参数的值是Stream的第一个元素，第二个参数是Stream的第二个元素**。
    Integer a = list2.stream().reduce((sum,item) ->sum+item).get();
    //为空时返回预设值
    list2.stream().reduce(0, (sum, item) -> sum + item);
    //allMatch：是不是Stream中的所有元素都满足给定的匹配条件
    System.out.println( list2.stream().allMatch(item ->item>0));
    //– anyMatch：Stream中是否存在任何一个元素满足匹配条件
    System.out.println(list2.stream().anyMatch(item ->item>1000));
    //– findFirst: 返回Stream中的第一个元素，如果Stream为空，返回空Optional
    //sorted()排序
    System.out.println(list2.stream().sorted().findFirst().get());
    //– noneMatch：是不是Stream中的所有元素都不满足给定的匹配条件
    System.out.println(list2.stream().noneMatch(item ->item>1000));
    //– max和min：使用给定的比较器（Operator），返回Stream中的最大|最小值
    list2.stream().max((o1, o2) -> o1.compareTo(o2)).ifPresent(System.out::println);
    list2.stream().sorted((o1, o2) -> o1.compareTo(o2)).forEach(System.out::println);
}
```