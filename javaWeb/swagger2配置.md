# 使用Swagger2

## 1.引入依赖

```pom
<!--swagger-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.7.0</version>
</dependency>
```

## 2.配置

```java

@Configuration
@EnableSwagger2

public class Swagger2Config {

    @Bean
    public Docket webApiConfig(){

        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("webApi") // 分组
                .apiInfo(webApiInfo()) // 
                .select()
                .paths(Predicates.not(PathSelectors.regex("/admin/.*"))) // 不显示 not
                .paths(Predicates.not(PathSelectors.regex("/error.*")))
                .build();
    }

    @Bean
    public Docket adminApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("adminApi")
                .apiInfo(adminApiInfo())
                .select()
                .paths(Predicates.and(PathSelectors.regex("/admin/.*"))) // 显示 and
                .build();
    }
    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
                .title("网站-课程中心API文档")
                .description("本文档描述了课程中心微服务接口定义")
                .version("1.0")
                .contact(new Contact("作者", "网址", "1871648088@qq.com"))
                .build();
    }
    private ApiInfo adminApiInfo(){
        return new ApiInfoBuilder()
                .title("后台管理系统-课程中心API文档")
                .description("本文档描述了后台管理系统课程中心微服务接口定义")
                .version("1.0")
                .contact(new Contact("作者", "网址", "1871648088@qq.com"))
                .build();
    }
}
```

## 3. 注解

```java
# MP的代码生成器已经在entity的实体类中生成了模型的定义
@ApiModel(value="Teacher对象", description="讲师")
@ApiModelProperty(value = "...")
    
@ApiModelProperty(value = "创建时间", example = "2019-01-01 8:00:00")
@TableField(fill = FieldFill.INSERT)
private Date gmtCreate;

@ApiModelProperty(value = "更新时间", example = "2019-01-01 8:00:00")
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date gmtModified;
```

##  4. 定义接口说明和参数说明

定义在类上：@Api

定义在方法上：@ApiOperation

定义在参数上：@ApiParam

```java
@Api(description="讲师管理")
@RestController
@RequestMapping("/admin/edu/teacher")
public class TeacherAdminController {
    @Autowired
    private TeacherService teacherService;
    @ApiOperation(value = "所有讲师列表")
    @GetMapping
    public List<Teacher> list(){
        return teacherService.list(null);
    }
    @ApiOperation(value = "根据ID删除讲师")
    @DeleteMapping("{id}")
    public boolean removeById(
            @ApiParam(name = "id", value = "讲师ID", required = true)
            @PathVariable String id){
        return teacherService.removeById(id);
    }
}
```



