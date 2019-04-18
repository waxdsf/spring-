### bootstrap篇

```java
@SpringBootApplication
public class MyApplication {
	public static void main(String[] args) {
		SpringApplication springApplication = new SpringApplication(MyApplication.class);
		springApplication.run(args);
	}
}
```

这是一个典型的springboot项目初始引导文件，可以看到其中实例化了org.springframework.boot.SpringApplication类，并调用了run方法；首先来看下这个类：

```java
public SpringApplication(Class<?>... primarySources) {
    this(null, primarySources);
}
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
    this.resourceLoader = resourceLoader;
    Assert.notNull(primarySources, "PrimarySources must not be null");
    this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = deduceMainApplicationClass();
}
```

可以看到首先调用了构造函数SpringApplication(Class<?>... primarySources)，先来看下文件注释的介绍：[github项目文件](<https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot/src/main/java/org/springframework/boot/SpringApplication.java>)

>此类可用于通过一个java main方法引导并启动一个spring应用，默认将通过下面几步来引导启动应用：
>
>- 根据传入的类路径构建合适的应用上下文；
>- 注册一个CommandLinePropertySource来将命令行参入的参数当成Spring properties(?配置)处理；
>- 更新应用上下文，加载所有的单例bean(bean是一个什么概念呢？我们后面再细细道来)；
>- 触发任何CommandLineRunner的bean；
>
>在大多数情况下，可以直接调用静态的run(Class, String[])方法来启动应用:
>
>```java
>public class MyApplication  {
>    public static void main(String[] args) {
>        SpringApplication.run(MyApplication.class, args);
>    }
>}
>```
>
>若要在run之前定制操作，则可以如下使用：
>
>```java
>public static void main(String[] args) {
>    SpringApplication application = new SpringApplication(MyApplication.class);
>    //... customize application settings here
>    application.run(args)
>}
>```
>
>spring应用可以通过多种方式读取bean，一般来讲建议使用一个单一的配置类引导应用启动，当然，你也可以通过下面几种方式配置资源：
>
>- 通过AnnotatedBeanDefinitionReader加载配置类
>- 通过XmlBeanDefinitionReader加载xml配置
>- 通过GroovyBeanDefinitionReader加载适当的脚本文件
>- 通过ClassPathBeanDefinitionScanner来扫描一个包下全部配置类
>
>配置属性绑定在SpringApplication类，所以可以实现动态配置



>初学者可能会好奇Class<?>… 这种写法代表的意义，其中Class<?>代表一种范型，即参数可以传入任意类型的类，…代表可变长参数，可以传入任意数量的参数，后面可以看到这些参数会被组织成数组处理

> 范型中?与T的差异：
>
> T代表了一种固定的类型，在调用时就要确定具体的类型；而?则代表不确定的类型，在调用时更灵活

