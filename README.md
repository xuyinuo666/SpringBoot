
# 自动配置



```java

```



# Web开发

## 注解

@Configuration:说明这是一个配置类

@Bean、@Component、@Controller、@Service、@Repository 

@ComponentScan、@Import

@Conditional :根据条件判断

@ImportResource: 引入原生文件 XXX.xml

==@ConfigurationProperties和@EnableConfigurationProperties的作用和区别==

```java
@ConfigurationProperties :此类和配置文件中绑定 例如 @ConfigurationProperties(prefix="xxx")
@EnableConfigurationProperties：作用：启用配置属性类,开启使用和配置文件绑定的类

用@EnableConfigurationProperties注解使@ConfigurationProperties注解生效。因此在该启动类中就可以获取刚才application.properties配置文件转化的bean了。
```



## 配置文件等

> properties文件绑定

```java
public class test {
    public static void main(String[] args) {
        String x=Thread.currentThread().getContextClassLoader().getResource("aa.properties").getPath();
        Properties p = new Properties();
        try {
            p.load(new FileInputStream(x));
        } catch (IOException e) {
            e.printStackTrace();
        }
        String one = p.getProperty("one");
        String two = p.getProperty("two");
        System.out.println(one+""+two);
    }
}
```



### 插件

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>


 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## 静态资源

```
mvc:
    static-path-pattern: /res/**
```

==默认访问静态资源文件没有前缀的,这时前缀添加一个res才能访问到,可以用来拦截过滤请求是访问静态资源文件还是Controller请求==

```
resources:
    static-locations: classpath:/haha/ //也可以是数组[]
```

==默认访问的静态资源文件都是在/static` (or `/public` or `/resources` or `/META-INF/resources文件夹下的,这时就指定了静态资源文件的位置为haha==

## 请求映射原理

SpringMVC功能分析都从 org.springframework.web.servlet.DispatcherServlet-》doDispatch（）

```
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;

		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

		try {
			ModelAndView mv = null;
			Exception dispatchException = null;

			try {
				processedRequest = checkMultipart(request);
				multipartRequestParsed = (processedRequest != request);

				// 找到当前请求使用哪个Handler（Controller的方法）处理
				mappedHandler = getHandler(processedRequest);
                
                //HandlerMapping：处理器映射。/xxx->>xxxx
```

handlerMapping 遍历可以处理请求的规则

```
• 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息。
• 如果有就找到这个请求对应的handler
• 如果没有就是下一个 HandlerMapping
• 我们需要一些自定义的映射处理，我们也可以自己给容器中放HandlerMapping。自定义 HandlerMapping
```

## 常用注解

```
@PathVariable、获取请求路径中的变量    	@GetMapping("/car/{id}") 获取到id
@RequestHeader、获取请求头中的信息
@ModelAttribute、
@RequestParam、提交的表单信息
@MatrixVariable、
@CookieValue、获取cookie的信息 @CookieValue cookie cookie 接收
@RequestBody 处理post请求的请求体表单信息
@RequestAttribute  获取请求域中的信息 例如 在conntroller中setAttribute后forward另一个请求,在这个请求中可以获取到setAttribute的值
```

## 数据响应

返回json数据:  依赖+@ResponseBody

```
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.3.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

