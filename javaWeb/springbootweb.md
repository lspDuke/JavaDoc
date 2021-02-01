# springBootWeb整理

## 静态资源

```
"classpath:/META-INF/resources/",
"classpath:/resources/", 
"classpath:/static/", 
"classpath:/public/"
"/**"
```

优先级：resources>static>public

##  首页

```java
@Configuration
public class WebmvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");

    }
}

```

## mybatis

1. 引入配置

2. 编写实体类

3. 编写xxxMapper

4. 编写xxxMapper.xml

   ```xml
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.li.mapper.UserMapper">
   
   </mapper>
   ```

   

5. 编写yml配置

   ```yaml
   mybatis:
     type-aliases-package: com.li.pojo
     mapper-locations: classpath:mapper/*.xml
   ```

   

6. 测试

```
<!-- https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-starter -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.23</version>
</dependency>

```

## springSecurity 安全



## Shiro 安全





