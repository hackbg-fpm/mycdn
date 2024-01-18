---
title: Java学习笔记
author: 肥胖喵
abbrlink: '2675'
tags:
  - Java
categories:
  - 笔记
summary: Java笔记
date: 2022-06-08 17:53:00
---

## List

### 1.List去重

```java
// 利用list中的元素创建HashSet集合，此时set中进行了去重操作
HashSet set = new HashSet(list); 
// 清空list集合
list.clear(); 
// 将去重后的元素重新添加到list中
list.addAll(set);
```

----


### 2.判断某个值是否存在List中

**List的contains(obj)方法**

实际上，List调用contains(Object obj)方法时，会遍历List中的每一个元素，然后再调用每个元素的equals()方法去跟contains()方法中的参数进行比较，如果有一个元素的equals()方法返回true则contains()方法返回true，否则所有equals()方法都不返回true，则ontains()方法则返回false。因此，重写了Course类的equals()方法，否则，testListContains()方法的第二条输出为false。
　　
**Set的Contains(obj)方法**

当调用HashSet的contains(Object obj)方法时，其实是先调用每个元素的hashCode()方法来返回哈希码，如果哈希码的值相等的情况下再调用equals(obj)方法去判断是否相等，只有在这两个方法所返回的值都相等的情况下，才判定这个HashSet包含某个元素。因此，需重写Course类的hashCode()方法和equals()方法。
　　
 **Map中是否包含指定的Key和Value**

在Map中，用containsKey()方法，判断是否包含某个Key值；用containsValue()方法，判断是否包含某个Value值。

**注**：跟List中的Contains()方法一样，Map中的ContainsValue()方法也需要调用某个Value值的equals()方法，去和参数对象进行比较，如果匹配成功，返回结果为true，说明在Map中的Value值确实包含参数对象。因此，需要重写Student类的equals()方法。

----

## Java日期时间

### 搜DateUtil工具类

### 搜DateFormatUtils工具类

### 常用date对象方法


- boolean after(Date date)
  若当调用此方法的Date对象在指定日期之后返回true,否则返回false。
- boolean before(Date date)
  若当调用此方法的Date对象在指定日期之前返回true,否则返回false。
- int compareTo(Date date)
  比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。
- long getTime( )
  返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。

----

### 日期格式化

```java
Date dNow = new Date( ); 
SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd hh:mm:ss"); 
System.out.println("当前时间为: " + ft.format(dNow));
```

### Date与String的相互转换

String转换成Date类型

```Java
SimpleDateFormat format= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date date = format.parse("2019-09-19")
```

Date转换成String类型

```Java
SimpleDateFormat format= new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String time = format.format(new Date());
```

---

### Calendar类

```java
//创建一个代表2009年6月12日的Calendar对象
Calendar c1 =Calendar.getInstance();
c1.set(2009, 6 - 1, 12);
//日期加10天
c1.add(Calendar.DATE, 10);
//获取月份
int month=c1.get(Calendar.MONTH);
```


#### Calendar与Date的相互转换

 Calendar转化为Date 

```Java
Calendar cal=Calendar.getInstance();  
Date date=cal.getTime();  
```


Date转化为Calendar  

```java
Date date=new Date();  
Calendar cal=Calendar.getInstance();  
cal.setTime(date);  
```

   

-----

### 打印间隔日期

```java
        //每隔3天打印日期
        String dateStart = "2021-02-07";
        String dateEnd = "2021-03-19";
        SimpleDateFormat date = new SimpleDateFormat("yyyy-MM-dd");
        long startTime = date.parse(dateStart).getTime();
        long endTime = date.parse(dateEnd).getTime();
        long day = 1000 * 60 * 60 * 24*3;
        for (long i = startTime; i <= endTime; i += day) {
            System.out.println(date.format(new Date(i)));
        }
```

## 对象拷贝

- 相当于get/set代码。
- BeanUtils.copyProperties()：浅拷贝，会拷贝**属性名称**和**属性类型**相同的属性。`BeanUtils.copyProperties(A对象, B对象);    //A对象拷贝到B对象`


- 除BeanUtils外还有一个名为**PropertyUtils**的工具类，它也提供copyProperties()方法，作用与BeanUtils的同名方法十分相似，主要的区别在于后者提供类型转换功能，即发现两个JavaBean的同名属性为不同类型时，在支持的数据类型范围内进行转换，而前者不支持这个功能，但是速度会更快一些

**注意**：属性类型不同的问题、空格问题、误传问题

## 常用注解

| 名称                  | 用法                                                         |
| --------------------- | ------------------------------------------------------------ |
| @Data                 | 用于实体类上方，自动生成get、set、toString等方法。需要有lombok插件和依赖。 |
| @NoArgsConstructor    | 用于实体类上方，自动生成无参构造函数。需要有lombok插件和依赖。 |
| @AllArgsConstructor   | 用于实体类上方，自动生成全参构造函数。需要有lombok插件和依赖。 |
| @Table                | 用于实体类上方，让实体类映射数据库中的表。用法：`@Table(name="tab_user",uniqueConstraints{@UniqueConstraint(columnNames={"uid","email"})}) `用于实体类上方，关联“tab_user”表，"uid","email"对应数据库表中的字段是唯一的，等同于多个`@Column(unique=true)`。 |
| @Id和@GeneratedValue  | 用于标注主键属性。如：`@Id@Column(name = "parts_info_id")@GeneratedValue(strategy = GenerationType.IDENTITY, generator = "JDBC")` |
| @Column               | 用于属性上方，将实体属性与数据库字段映射，用法：`@Column(name = "parts_info_num")` ,当属性名为partsInfoNum时可省略。 |
| @Api                  | 用于Controller类上方，Swagger的注解。表示标识这个类是swagger的资源。如：`@Api(tags = "记录类")` |
| @ApiOperation         | 用于Controller类的方法上方，Swagger的注解。表示一个http请求的操作。如：`@ApiOperation(value = "方法描述", notes = "提示内容")` |
| @ApiMode              | 用在返回对象类上，Swagger的注解。表示对类进行说明。如：`@ApiModel("备件领取请求类")`。value--表示对象名description--描述 |
| @ApiModelProperty     | 用于返回对象属性上，Swagger的注解。如：`@ApiModelProperty(value="备件信息主键")`。value--字段说明，name--重写属性名字，dataType--重写属性类型，required--是否必填，example--举例说明，hidden--隐藏 |
| @NotNull              | 用于实体类或model类属性上方，用于数据校验。需要有注解@valid开启校验。不能为null，但可以为empty。null: 表示对象为空的校验。empty: 表示对象为空或长度为0的String。blank: 表示对象为空或长度为0的String、空格字符串。 |
| @NotEmpty             | 同上为校验注解。不能为null，而且长度必须大于0。              |
| @NotBlank             | 同上为校验注解。**只能作用在String上**，不能为null，而且调用trim()后，长度必须大于0。 |
| @Size                 | @Size(min = 0, max = 30, message = "登录账号长度不能超过30个字符")，校验字符串长度 |
| @Slf4j                | 用于类上方，用于日志输出。需要有lombok插件和依赖。           |
| @Service              | 用于service类上方，标注业务层组件，默认名称为类名。          |
| @Transactiona         | 是声明式事务管理，在接口实现类或接口实现方法上添加，只有public的方法才起作用，只读的接口不需要使用事务接口。@Transactional(rollbackFor = Exception.class) |
| @Resource和@Autowired | 都是用来实现依赖注入的。只是@AutoWried按by type自动注入，而@Resource默认按byName自动注入。 |
| @RequestParam         | 用于Controller类的请求参数前，将请求参数绑定到你控制器的方法参数上（是springmvc中接收普通参数的注解）用法：`@RequestParam(value=参数名”,required=”true（请求路径必须包含该参数）/false”,defaultValue=“默认值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值”)`                                                                                                  如：public ApiResponse method11(@RequestParam("id") int id, @RequestBody List<Dog> dogs) |
| @RequestPart          | 用于Controller类的请求参数前，这个注解用在multipart/form-data表单提交请求的方法上，主要用来搭配springboot接收MultipartFile类型的文件 如：`@RequestPart("file") MultipartFile file`。 |
| @RequestBody          | 用于Controller类的请求参数前，通过@requestBody可以将请求体中的JSON字符串绑定到相应的bean上，当然，也可以将其分别绑定到对应的字符串上。如`(@requestBody String userName,@requestBody String pwd) 或 （@requestBody User user）` |
| @PathVariable         | 映射 URL 绑定的占位符<br/>@RequestMapping("/getUserById/{name}")<br/>    public User getUser(@PathVariable("name") String name){<br/>        return userService.selectUser(name);<br/>    } |
| @RestController       | 用于Controller类上方，使返回的数据是Json格式                 |
| @Slf4j                | 放在需要日志输出的类上方，相当`logprivate final Logger logger = LoggerFactory.getLogger(当前类名.class);` ,就可以用log.info("")了 |
| @IgnoreUserToken      | 注解关闭token验证                                            |
| @PostConstruct        | 是Java自带的注解，在方法上加该注解会在项目启动的时候执行该方法，也可以理解为在spring容器初始化的时候执行该方法。 |


自定义注解

```java
package com.ruoyi.common.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * 对外接口校验token信息是否正确
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(value={ElementType.METHOD,ElementType.TYPE})
public @interface TokenVerify {

    boolean value() default true;
}

```



----

## Map遍历




- 方法一：普通的foreach循环，使用keySet()方法，遍历key

```java
for(Integer key:map.keySet()){
       System.out.println("key:"+key+" "+"Value:"+map.get(key));
    }
```

简化

```java
map.forEach((key,value) -> {
    System.out.println(key);
    System.out.println(value);
});
```

- 方法二：把所有的键值对装入迭代器中，然后遍历迭代器

```java
Iterator<Map.Entry<Integer,String>> it=map.entrySet().iterator();
while(it.hasNext()){
    Map.Entry<Integer,String> entry=it.next();
    System.out.println("key:"+entry.getKey()+" "+"Value:"+entry.getValue());
}
```

- 方法三：分别得到key和value

```java
for(Integer obj : map.keySet()){
      System.out.println("key:"+obj);
}
for(String obj : map.values()){
      System.out.println("value:"+obj);
}
```

- 方法四，entrySet()方法

```java
Set<Map.Entry<Integer,String>> entries=map.entrySet();
for (Map.Entry entry:entries){
     System.out.println("key:"+entry.getKey()+" "+"value:"+entry.getValue());
}
```

## Map求和

```java
public Integer mapCount(HashMap<String, Integer> map) {
        List<Integer> collect = map.entrySet().stream().map(x -> x.getValue()).collect(Collectors.toList());
        return collect.stream().mapToInt(x -> x).sum();
    }
```

## StringJoiner字符串处理

```java
setEmptyValue()：设置空值
toString()：转换成String
add()：添加字符串
merge():从另一个StringJoiner合并
length()：长度（包括前后缀）
```

示例

```java
public class Test {
    public static void main(String[] args) {
        StringJoiner stj = new StringJoiner(",", "--", "++");
        stj.add("hello");
        stj.add("world");
        stj.add("欢迎使用StringJoiner");
        System.out.println(stj);
    }
}
 
--hello,world,欢迎使用StringJoiner++
```



## 其他

### SQL按条件求和

```sql
一般求和
select sum(money) from user group by id;

按条件求和
select sum(if(type=1,money,0)) from user group by id;
```

### Windows10关闭端口

```cmd
netstat -ano|findstr 8080   #查看对应端口的pid
taskkill /pid 165376 /F     #终止对应pid的进程
```

#### 打包tar文件

```java
docker save -o psds-xyqz-dataapi.tar ps-docker-registry.cn-beijing.cr.aliyuncs.com/psdsframework/psds-xyqz-dataapi:0.0.47
```

