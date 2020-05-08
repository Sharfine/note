# JPA

```java
//建表类

@Data
@Entity
@DynamicUpdate
@ToString(callSuper = true)
@Table(indexes = {
        @Index(columnList = "age")
})
public class Person {
    @Id
    @GeneratedValue
    private Integer id;
    @Column(nullable = false)
    private String name;
    @Column(nullable = false)
    private String age;

}
```

```properties
#配置文件
spring.data.mongodb.uri= mongodb://localhost:27017/sharfine
spring.datasource.url=jdbc:mysql://192.168.1.19:3306/sharfine
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=tr
```

```java
//主启动类
@SpringBootApplication
@EntityScan(basePackages = "com.jpa.demo.mysql.database.model")
@EnableJpaRepositories(basePackages = "com.jpa.demo.mysql.database.dao")
```

```java
//dao
public interface PersonDao extends JpaSpecificationExecutor<Person>, JpaRepository<Person, Integer> {
    Person findFirstByAge(String age);
}
```

