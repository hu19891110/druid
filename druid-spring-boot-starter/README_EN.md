# Druid Spring Boot Starter
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.alibaba/druid-spring-boot-starter/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.alibaba/druid-spring-boot-starter/)

## English | [中文](https://github.com/alibaba/druid/blob/master/druid-spring-boot-starter/README.md)
Spring Boot with Druid support, help you simplify Druid config in Spring Boot.

## Usage
Add the ```druid-spring-boot-starter``` dependency in Spring Boot project.

Maven
```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid-spring-boot-starter</artifactId>
   <version>VERSION_CODE</version>
</dependency>
```
Gradle
```xml
compile 'com.alibaba:druid-spring-boot-starter:VERSION_CODE'

```
Note: Please check the latest release version name in [Here][1] , or select a release version name in [Here][2] , **replace ```VERSION_CODE```**,  such as ```1.1.1``` .

[1]: https://maven-badges.herokuapp.com/maven-central/com.alibaba/druid-spring-boot-starter/
[2]: http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.alibaba%22%20AND%20a%3A%22druid-spring-boot-starter%22
## Configuration Properties
Druid Spring Boot Starter properties name in full compliance with Druid configuration, you can configure the Druid database connection pool and monitor the configuration properties through the following configuration properties, using default values if not configured.
```xml
# JDBC
spring.datasource.druid.url= # or spring.datasource.url= 
spring.datasource.druid.username= # or spring.datasource.username=
spring.datasource.druid.password= # or spring.datasource.password=
spring.datasource.druid.driver-class-name= # or spring.datasource.driver-class-name=

# Connection pool properties, detail see Druid Wiki
spring.datasource.druid.initial-size=
spring.datasource.druid.max-active=
spring.datasource.druid.min-idle=
spring.datasource.druid.max-wait=
spring.datasource.druid.pool-prepared-statements=
spring.datasource.druid.max-pool-prepared-statement-per-connection-size= 
spring.datasource.druid.max-open-prepared-statements= #Equivalent to the above 'max-pool-prepared-statement-per-connection-size'
spring.datasource.druid.validation-query=
spring.datasource.druid.validation-query-timeout=
spring.datasource.druid.test-on-borrow=
spring.datasource.druid.test-on-return=
spring.datasource.druid.test-while-idle=
spring.datasource.druid.time-between-eviction-runs-millis=
spring.datasource.druid.min-evictable-idle-time-millis=
spring.datasource.druid.max-evictable-idle-time-millis=
spring.datasource.druid.filters= #Druid filters, default value stat, multiple separated by comma.

# WebStatFilter properties, detail see Druid Wiki
spring.datasource.druid.web-stat-filter.enabled= #Enable StatFilter, default value true.
spring.datasource.druid.web-stat-filter.url-pattern=
spring.datasource.druid.web-stat-filter.exclusions=
spring.datasource.druid.web-stat-filter.session-stat-enable=
spring.datasource.druid.web-stat-filter.session-stat-max-count=
spring.datasource.druid.web-stat-filter.principal-session-name=
spring.datasource.druid.web-stat-filter.principal-cookie-name=
spring.datasource.druid.web-stat-filter.profile-enable=

# StatViewServlet properties, detail see Druid Wiki
spring.datasource.druid.stat-view-servlet.enabled= #Enable StatViewServlet, default value true.
spring.datasource.druid.stat-view-servlet.url-pattern=
spring.datasource.druid.stat-view-servlet.reset-enable=
spring.datasource.druid.stat-view-servlet.login-username=
spring.datasource.druid.stat-view-servlet.login-password=
spring.datasource.druid.stat-view-servlet.allow=
spring.datasource.druid.stat-view-servlet.deny=

# With Spring monitoring properties, detail see Druid Wiki
spring.datasource.druid.aop-patterns= # Spring monitoring AOP point, such as x.y.z.service.*, multiple separated by comma.
# If 'spring.datasource.druid.aop-patterns' to be the agent class does not define interface need set 'spring.aop.proxy-target-class = true' .
```
Note: The IDE prompts the above configuration properties, the format of the configuration file you can choose ```.properties``` or``` .yml```, the effect is the same.
## How to Extended Configuration
If the configuration properties provided by the auto-configuration do not meet your needs, you can use ```DruidDataSourceBuilder``` to create ``` DruidDataSource```, and then do some custom configuration, as follows.

```java
@Bean
public DataSource dataSource(Environment env){
    DruidDataSource dataSource = DruidDataSourceBuilder
            .create()
            .build(env,"spring.datasource.druid.");//Use a custom prefix
    
    //dataSource.setProxyFilters(filters);//Add a custom Filter
    //...Other
    return dataSource;
}
```
Note: [FAQ #25](https://github.com/alibaba/druid/wiki/FAQ#25-how-to-add-custom-wallconfig-filter-in-the-spring-boot-) has a custom ```WallConfig, Filter``` example for reference.

## How to Configuration Multiple DataSource
1. Add DataSource properties
```xml
spring.datasource.druid.one.url=
spring.datasource.druid.one.username=
spring.datasource.druid.one.password=
spring.datasource.druid.one.driver-class-name=
spring.datasource.druid.one.max-active=
...

spring.datasource.druid.two.url=
spring.datasource.druid.two.username=
spring.datasource.druid.two.password=
spring.datasource.druid.two.driver-class-name=
spring.datasource.druid.two.max-active=
...
```
2. Use```DruidDataSourceBuilder```create DataSource
```java
@Bean
@Primary
public DataSource dataSourceOne(Environment env){
   return DruidDataSourceBuilder
           .create()
           .build(env, "spring.datasource.druid.one.");
}
@Bean
public DataSource dataSourceTwo(Environment env){
   return DruidDataSourceBuilder
           .create()
           .build(env, "spring.datasource.druid.two.");
}
```

## Samples
Clone the project, run ```DemoApplication``` within the ```test``` package.

## Reference
- [Druid Wiki](https://github.com/alibaba/druid/wiki)

- [Spring Boot Reference](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)