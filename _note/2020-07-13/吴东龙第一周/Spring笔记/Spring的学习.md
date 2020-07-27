## Spring的学习

- **Srping的体系结构**

  ![1595162075135](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1595162075135.png)

- **Spring的mvn依赖**

   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220200813890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMTc5Njkx,size_16,color_FFFFFF,t_70) 

  ___

  

- **搭建基于xml的Spring开发环境**

  - **1、创建maven工程 (不使用骨架创建)**

  - **2、在pom中导入Spring框架的依赖**

    ``` xml
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>5.0.5.RELEASE</version>
            </dependency>
    ```

  - **3、创建Bean标签，让Spring获得对象的创建和管理权限**

    ```xml
     <bean id="userDao" class="com.wudonglong.dao.impl.UserDaoImpl" ></bean>
    ```

  - **4、从Spring的IOC容器中根据id获取对象**

    ```java
    ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
    UserDao userDao =  (UserDao) app.getBean("userDao");
    userDao.save();
    ```

    

  ***

  

  ## ApplicationContext的三个常用实现类

  -  **ClassPathXmlApplicationContext** 

     可以加载类路径下的配置文件，配置文件必须放在类路径下 

  -  **AnnotationConfigApplicationContext** 

     可以加载任意磁盘下的配置文件（必须有访问权限） 

    ```java
     ApplicationContext ac = new FileSystemXmlApplicationContext("D:\Spring\spring_ioc\src\main\resources\applicationContext.xml"); 
    ```

  -  **FileSystemXmlApplicationContext** 

     用于读取注解创建容器 

  ___

  ## Bean对象的创建时机

  - **ApplicationContext**：（单例对象适用）创建容器的时候，创建对象采取的策略是立即加载。即只要一读取完配置文件就立即创建配置文件中配置的对象。

    

  - **BeanFactory**：（多例对象适用）创建容器的时候，创建对象采取的策略是延迟加载。即什么时候根据id获取对象，什么时候才真正的创建对象。


  ___

  ## 创建Bean的三种方法

  - **无参构造方法构造（默认构造函数）**

    ```xml
    <bean id="userdao" class="com.itheima.service.impl.AccountServiceImpl"></bean>
    ```

  - **工厂静态方法构造**

    ```xml
    <bean id="userDao" class="com.wudonglong.factory.StaticFactory" factory-method="getUserDao" ></bean>
    ```

    ```java
     public static UserDao getUserDao(){
            return new UserDaoImpl();
        }
    ```

    

    **注意**：方法必须为静态方法

  - **工厂实例方法构造**

    ```xml
     <bean id="factory" class="com.wudonglong.factory.DynamicFactory"  ></bean>    
     <bean id="userDao" factory-bean="factory" factory-metho d="getUserDao"></bean>
    ```

    ```java
        public  UserDao getUserDao(){
            return new UserDaoImpl();
        }
    ```

  ___

  ## Bean对象的作用范围

  - Bean标签的scope属性
                    作用：用于指定bean的作用范围
  - 取值:
    - **singleton**：单例的（默认值）
    -  **prototype**：多例的
    -  **request**：作用于web应用的请求范围
    -  **session**：作用于web应用的会话范围
    -  **global-session**：作用于集群环境的会话  范围（全局会话范文），当不是集群环境时它就是session
  - 当我们创建两个bean对象时，他们是一样的，即**默认情况**下spring的bean对象是**单例的**

  ___

  

  

  ## Bean对象的生命周期

  - **单例对象**
    - **出生：** 容器创建时对象出生 
    - **存活****：** 只要容器还在，对象就存活 
    - **死亡**： 容器销毁时，对象也被销毁 
    - **总结**： **单例对象生命周期和容器相同** 

  - **多例对象**
    - **出生：** 当我们使用对象时spring框架为我们创建 
    - **存活： 对象再使用过程中一直活着 **
    - **死亡：** 当对象长时间不用且没有别的对象引用时由java垃圾回收机制回收 

  ___

  ## 依赖注入

  - **对象的注入**

  - **使用构造函数注入**

    ```java
    package com.itheima.service.impl;
    
    import com.itheima.service.IAccountService;
    
    import java.util.Date;
    
    /**
     * 账户业务层实现类
     */
    public class AccountServiceImpl implements IAccountService {
    
        // 如果是经常变化的数据，并不适用于注入的方式
        private String name;
        private Integer age;
        private Date birthday;
    
        public AccountServiceImpl(String name, Integer age, Date birthday) {
            this.name = name;
            this.age = age;
            this.birthday = birthday;
        }
    
        public void saveUser() {
            System.out.println("AccountServiceImpl中的saveUser()方法执行了\n"+name+","+age+","+birthday);
        }
    
    
    }
    
    
    ```

    **xml配置**

    ```xml
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
            <constructor-arg name="name" value="小明"></constructor-arg>
            <constructor-arg name="age" value="18"></constructor-arg>
            <constructor-arg name="birthday" ref="now"></constructor-arg>
        </bean>
    
        <!-- 配置一个日期对象 -->
        <bean name="now" class="java.util.Date"></bean>
    
    ```

  - **set方法注入**

    ```java
    public class UserServiceImpl implements UserService {
    
        private UserDao userDao;
           public void setUserDao(UserDao userDao) {
            this.userDao = userDao;
        }
    
    }
    
    ```

    ```xml
            <bean id="userDao" class="com.wudonglong.dao.impl.UserDaoImpl" ></bean>
            <bean id="userService" class="com.wudonglong.service.impl.UserServiceImpl">    
                    <property name="userDao  (set后面的东西,首字母要小写)" ref="userDao"></property> 		 </bean>
    ```

  - **有参构造注入**

    ```java
        public UserServiceImpl(UserDao userDao) {
            this.userDao = userDao;
        }
    ```

    

    ```xml
            <bean id="userService" class="com.wudonglong.service.impl.UserServiceImpl">
                    <constructor-arg name="userDao" ref="userDao"></constructor-arg>
                    <!--               构造对象的参数名字    -->
            </bean>
    ```

    - **P命名空间**

      ```xml
              <bean id="userDao" class="com.wudonglong.dao.impl.UserDaoImpl" ></bean>
              <bean id="userService" class="com.wudonglong.service.impl.UserServiceImpl" p:userDao-ref="userDao"></bean>
      ```

      - **首先要配置**

      ```xml
      
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:p="http://www.springframework.org/schema/p"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      ```

      

    ___

    

  - **普通属性注入**

    ```xml
            <bean id="userDao" class="com.wudonglong.dao.impl.UserDaoImpl" >
                    <property name="username" value="zhangsan"/>
                    <property name="age" value="24"/>
            </bean>
    ```

    ```java
        public void setAge(int age) {
            this.age = age;
        }
    	 public void setUsername(int username) {
            this.username = username;
        }
    ```

  - **集合的注入**

    ```xml
          <bean id="userDao" class="com.wudonglong.dao.impl.UserDaoImpl" >
                    <property name="strList">
                          <list>
                                  <value>aaa</value>
                                  <value>bbb</value>
                                  <value>ccc</value>
                          </list>
                    </property>
    
                    <property name="userMap">
                            <map>
                                    <entry key="u1" value-ref="user1"></entry>
                                    <entry key="u2" value-ref="user2"></entry>
                            </map>
                    </property>
    
                    <property name="properties">
                            <props>
                                    <prop key="p1">pp1</prop>
                                    <prop key="p2">pp2</prop>
                                    <prop key="p3">pp3</prop>
                            </props>
                    </property>
            </bean>
    
    
    
    
            <bean id="user1" class="com.wudonglong.domain.User">
                    <property name="name" value="tom"/>
                    <property name="addr" value="guangdong"/>
            </bean>
            <bean id="user2" class="com.wudonglong.domain.User">
                    <property name="name" value="tom2"/>
                    <property name="addr" value="guangdong2"/>
            </bean>
    ```

    ```java
      private List<String> strList;
        private Map<String, User> userMap;
        private Properties properties;
    
        public void setStrList(List<String> strList) {
            this.strList = strList;
        }
    
        public void setUserMap(Map<String, User> userMap) {
            this.userMap = userMap;
        }
    
        public void setProperties(Properties properties) {
            this.properties = properties;
        }
    ```

    ___

- 搭建基于注解的Spring开发环境

  -  **想使用注解配置，必须先在spring配置文件中添加一下约束**
    还有告诉spring创建容器时要扫描的包，配置的标签不是在beans的约束，而是一个名称为
    **context**名称空间中和约束中 

    - **设置context命名空间**

      ```xml
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                  http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd">
      ```

      **@Component**

      - 创建对象的注解

       - 他们的作用就和在xml配置文件中编写一个<bean>标签实现的功能一样

         ```java
         @Component("userService")
         //等同于在xml中创建userService的bean
         public class UserServiceImpl implements UserService {
         }
         ```

      **@Controller：一般用在表现层**     

      **@Service：一般用在业务层** 

      **@Repository一般用在持久层** 

      ！：跟**@Component**注解功能一样

      ___

      ## 注解注入对象

       **@Autowired、@Qualifier与@Resource**

      ```java
      public class UserServiceImpl implements UserService {
          //等同于<property name="userDao" ref="userDao"></property>
      
      //    @Autowired 按照数据类型从Spring容器中进行匹配的
      //    @Qualifier("userDao")按照ID值从容器中进行匹配的 但是主要此处 @Qualifier 结合 @Autowired						  一起使用
      //    @Resourc 按照名称从Spring容器中进行匹配的
          @Resource(name="userDao") 
          private UserDao userDao;
      
      }
      ```

      ___

      ## 改变作用范围的注解

      **@Scope**:改变单例以及多例范围

      ___

      ## 和生命周期相关的注解

      **@PostConstruct：用于指定初始化方法  @PreDestroy：用于指定销毁方法** 

      ```java
          @PostConstruct
          public void init(){
              System.out.println("Service对象的初始化方法");
          }
          @PreDestroy
          public void destroy(){
              System.out.println("Service对象的销毁方法");
          }
      ```

      ___

      ## spring配置类（为了完全消除xml文件）

      

       **@Configuration**：表面该类是一个配置类

       细节：当配置类作为**AnnotationConfigApplicationContext**对象创建对象时，该注解可以不写 

      **@ComponentScan**： 用于通过注解指定spring在创建容器时要扫描的包 

      ```java
      //标志该类是Spring的核心配置类
      @Configuration
      //配置组件扫描
      @ComponentScan("com.wudonglong")
      //总配置
      @Import({DataSourceConfiguration.class})
          public class SpringCofiguration {
      
          }
      ```

      **@Bean**: 用于把方法的返回值作为bean对象存入spring的IOC容器中 

      ```java
          @Bean(name = "runner")
          public QueryRunner createQueryRunner(DataSource dataSource){
              return new QueryRunner(dataSource);
          }
      ```

      

      ___

      ## 数据源抽取以及调用相关的注解

      ```java
      //配置类
      @Configuration
      //数据源相关配置
      @PropertySource("classpath:jdbc.properties")
      public class DataSourceConfiguration {
      
          @Value("${jdbc.Driver}")
          private String Driver;
      
          @Value("${jdbc.url}")
          private String url;
      
          @Value("${jdbc.username}")
          private String username;
      
          @Value("${jdbc.password}")
          private String password;
      
          @Bean("dataSource")
          public DataSource getDataSource() throws PropertyVetoException {
              ComboPooledDataSource dataSource = new ComboPooledDataSource();
              dataSource.setDriverClass(Driver);
              dataSource.setJdbcUrl(url);
              dataSource.setUser(username);
              dataSource.setPassword(password);
              return dataSource;
          }
      }
      ```

      **@PropertySource**:加载外部properties文件

      等同于：

      ```xml
      <context:property-placeholder location="classpath:jdbc.properties"/>
      ```

      ```properties
      jdbc.Driver=com.mysql.jdbc.Driver
      jdbc.url=jdbc:mysql://localhost:3306/shop
      jdbc.username=root
      jdbc.password=root
      ```

      **@Impor**t:导入相同的配置类（利于模块化开发）

      ![1595168013197](C:\Users\25469\AppData\Roaming\Typora\typora-user-images\1595168013197.png)

      等同于：

      ```xml
       <import resource="applicationContext-user.xml"/>
      ```

      ___

      ## Spring整合junit

      步骤：

       - 1、导入整合junit的jar包

         ```xml
                 <dependency>
                     <groupId>org.springframework</groupId>
                     <artifactId>spring-test</artifactId>
                     <version>5.0.5.RELEASE</version>
                 </dependency>
         ```

         

       - 2、使用junit提供的注解把原有的main方法替换，替换成spring提供的main方法@RunWith

       - 3、告知spring运行器，spring的IOC创建时基于xml配置文件还是基于注解，并说明位置@ContextConfiguration

         ```java
         @RunWith(SpringJUnit4ClassRunner.class)
         //加载配置文件
         //@ContextConfiguration("classpath:applicationContext.xml")
         //加载配置类
         @ContextConfiguration(classes = {SpringCofiguration.class})
         public class SpringJunitTest {
         
             @Autowired
             private UserService userService;
         
             @Autowired
             private DataSource dataSource;
         
             @Test
             public void test1() throws SQLException {
                 System.out.println(dataSource.getConnection());
                 userService.sava();
             }
         
         }
         ```

         

       - locations: 指定xml文件位置和classpath关键字，表示在类路径下

       - classes: 指定注解类所在的位置

         ### 直接用Autowired注入对象来进行测试

      ___

      ## AOP（面向切面编程）

      - **配置XML实现**

      - Aop环境配置与案例

        ```xml
                <dependency>
                    <groupId>org.aspectj</groupId>
                    <artifactId>aspectjweaver</artifactId>
                    <version>1.8.4</version>
                </dependency>
                <dependency>
                    <groupId>org.springframework</groupId>
                    <artifactId>spring-test</artifactId>
                    <version>5.0.5.RELEASE</version>
                </dependency>
        ```

      - 创建接口以及对应的实现类

        ```java
        public interface TargetInterface {
            public void save();
        }
        
        
        //实现类
        @Component("target")
        public class Target implements TargetInterface {
            public void save() {
        //        int i = 1/0;
                System.out.println("sava running");
        
            }
        }
        
        ```

      - 对方法进行增强

        ```java
        public class MyAspect {
            public void before(){
                System.out.println("前置增强");
            }
        
            public void afterReturning(){
                System.out.println("后置增强");
            }
        
            //ProceedingJoinPoint: 正在执行的连接点
            public Object around(ProceedingJoinPoint pjp) throws Throwable {
                System.out.println("环绕前增强");
                Object proceed = pjp.proceed();//切点方法
                System.out.println("环绕后增强");
                return proceed;
            }
        
        
            public void afterThrowing(){
                System.out.println("异常抛出增强");
            }
        
            public void after(){
                System.out.println("最终增强");
            }
        }
        ```

      - XML中需要配置（首先需要配置aop命名空间）

        ```xml
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:aop="http://www.springframework.org/schema/aop"
               xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                   http://www.springframework.org/schema/aop  http://www.springframework.org/schema/aop/spring-aop.xsd">
        <!--    目标对象-->
            <bean id="target" class="com.wudonglong.aop.Target"></bean>
        
        <!--    切面对象-->
            <bean id="myAspect" class="com.wudonglong.aop.MyAspect"></bean>
        
        <!--    配置织入-->
            <aop:config>
                <aop:aspect ref="myAspect">
        <!--            抽取切点表达式-->
                    <aop:pointcut id="myPoincut" expression="execution(public void com.wudonglong.aop.Target.save())"/>
        
        <!--            <aop:before method="before" pointcut="execution(public void com.wudonglong.aop.Target.save())"/>-->
        <!--            <aop:after-returning method="afterReturning" pointcut="execution(public void com.wudonglong.aop.Target.save())"/>-->
        <!--            <aop:around method="around" pointcut="execution(public void com.wudonglong.aop.Target.save())"/>-->
        <!--            <aop:after-throwing method="afterThrowing" pointcut="execution(public void com.wudonglong.aop.Target.save())"/>-->
        <!--            <aop:after method="after" pointcut="execution(public void com.wudonglong.aop.Target.save())"/>-->
                        <aop:around method="around" pointcut-ref="myPoincut"></aop:around>
                </aop:aspect>
            </aop:config>
        <!--
                spring中基于xml的AOP配置步骤
                    1、把通知bean交给spring来管理
                    2、使用aop:config标签表面开始aop的配置
                    3、使用aop:aspect标签表明配置切面
                        id属性：给切面提供唯一标识
                        ref属性：指定通知类bean的id
                    4、在aop:aspect的内部使用对应的标签来配置通知的类型
                        我们现在的示例是让printLog方法在切入点方法执行之前执行
                        aop:before：配置前置通知
                            method：用于指定类中哪个方法是前置通知
                            pointcut：用于指定切入点表达式，该表达式含义指的是对业务层中哪些方法增强
        
                    切入点表达式写法：
                        关键字：execution(表达式)
                        表达式写法：
                            访问修饰符   返回值   包.包....包.类名.方法名（参数列表）
                        标准的表达式写法：
                            public void com.itheima.service.impl.AccountService.saveAccount()
                        访问修饰符可以省略
                            void com.itheima.service.impl.AccountService.saveAccount()
                        返回值可以使用通配符，表示任意返回值
                            * com.itheima.service.impl.AccountService.saveAccount()
                        包名可以使用通配符，表示任意包。但是有几级包就要写几个*.
                            * *.*.*.*.AccountService.saveAccount()
                        包名可以使用 .. 当前包及其子包
                            * *..AccountService.saveAccount()
                        类名和方法名都可以使用*来实现通配
                            * *..*.saveAccount()
                        参数列表
                            可以直接写数据类型
                                基本类型直接写名称 int    * *..*.*(int)
                                引用类型写包名.类名方式  java.lang.String     * *..*.*(java.lang.String)
                            可以使用通配符表示任意类型，但是必须有参数
                                * *..*.*(*)
                            可以适用 .. 表示有无参数即可
                        全通配写法：
                            * *..*.*(..)
        
                        通常写法：
                            切到业务层实现类下的所有方法
                                * com.itheima.service.impl.*.*(..)
             -->
        
        </beans>
        ```

        ___

      - 使用注解实现AOP

        - 首先配置xml

          ```xml
          <beans xmlns="http://www.springframework.org/schema/beans"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xmlns:aop="http://www.springframework.org/schema/aop"
                 xmlns:context="http://www.springframework.org/schema/context"
                 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                     http://www.springframework.org/schema/aop  http://www.springframework.org/schema/aop/spring-aop.xsd
                                     http://www.springframework.org/schema/context   http://www.springframework.org/schema/context/spring-context.xsd">
          
          <!--    开启组件扫描-->
              <context:component-scan base-package="com.wudonglong.anno"></context:component-scan>
          <!--    aop自动代理-->
              <aop:aspectj-autoproxy/>
          </beans>
          ```

        - 切面类

          ```java
          @Aspect  //标注当前类是个切面类
          public class MyAspect {
              //注解的前置增强
              @Before("execution(public void com.wudonglong.anno.Target.save())")
              public void before(){
                  System.out.println("前置增强");
              }
          	
              @AfterReturning("pointcut()")
              public void afterReturning(){
                  System.out.println("后置增强");
              }
          
              //ProceedingJoinPoint: 正在执行的连接点
               * 环绕通知
               * 问题：
               *      配置了环绕通知之后，切入点方法没有执行，而通知方法执行了
               * 分析：
               *
               *  动态代理中的环绕通知有明确的切入点方法调用，而我们的方法中没有
               * 解决：
               *      Spring为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法					proceed(),此方法相当于明确调用切入点方法
               * 该接口可以作为环绕通知方法参数，在程序执行时，spring框架会为我们提供该接口的实现类				供我们使用
              * 它是spring框架为我们提供的一种可以在代码中手动控制增强代码何时执行的方式
              public Object around(ProceedingJoinPoint pjp) throws Throwable {
                  System.out.println("环绕前增强");
                  Object proceed = pjp.proceed();//切点方法
                  System.out.println("环绕后增强");
                  return proceed;
              }
          
              @AfterThrowing(..)
              public void afterThrowing(){
                  System.out.println("异常抛出增强");
              }
          	
              @After(..)
              public void after(){
                  System.out.println("最终增强");
              }
          
              @Pointcut("execution(public void com.wudonglong.anno.Target.save())")
              //定义切点表达式
              public void pointcut(){}
          
          }
          ```

          ___

          ## JdbcTemplate的使用

          - 导包

            ```xml
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>spring-jdbc</artifactId>
                        <version>5.0.5.RELEASE</version>
                    </dependency>
            ```

          - 配置数据源

            ```xml
            <beans xmlns="http://www.springframework.org/schema/beans"
                   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xmlns:context="http://www.springframework.org/schema/context"
                   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                       http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd">
            
                <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
            
            <!--    数据源对象-->
                <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
                    <property name="driverClass" value="${jdbc.driver}"/>
                    <property name="jdbcUrl" value="${jdbc.url}"/>
                    <property name="user" value="${jdbc.username}"/>
                    <property name="password" value="${jdbc.password}"/>
                </bean>
            
                <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
                    <property name="dataSource" ref="dataSource"></property>
                </bean>
            </beans>
            ```

          - 测试类

            ```java
            public class JdbcTemplateTest {
            
            
                @Test
                //测试JdbcTemplate开发步骤
                public void test1() throws PropertyVetoException {
                    //创建数据源对象
                    ComboPooledDataSource dataSource = new ComboPooledDataSource();
                    dataSource.setDriverClass("com.mysql.jdbc.Driver");
                    dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/account?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC");
                    dataSource.setUser("root");
                    dataSource.setPassword("root");
                    JdbcTemplate jdbcTemplate = new JdbcTemplate();
            
                    //设置数据源对象   了解数据库在哪
                    jdbcTemplate.setDataSource(dataSource);
            
                    //执行操作
                    int row =  jdbcTemplate.update("insert into account values(?,?,?)" ,0,"tom",5000);
                    System.out.println(row);
                }
            
            
                @Test
                //测试Spring产生JdbcTemplate对象
                public void test2() throws PropertyVetoException {
                    ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
                    JdbcTemplate jdbcTemplate = (JdbcTemplate) app.getBean("jdbcTemplate");
            
                    //执行操作
                    int row =  jdbcTemplate.update("insert into account values(?,?,?)" ,0,"cat2",2333);
                    System.out.println(row);
                }
            }
            ```

            ___

            ## 使用Junit集成测试

            ```java
            @RunWith(SpringJUnit4ClassRunner.class)
            @ContextConfiguration("classpath:applicationContext.xml")
            public class JdbcTemplateCRUDTest {
            
                @Autowired
                private JdbcTemplate jdbcTemplate;
            
                @Test
                public void testUpdate(){
                    jdbcTemplate.update("update account set money = ? where name =?",10000,"tom");
                }
            
                @Test
                //自动封装实体类
                public void testQueryAll(){
                    List<Account> accountList = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
                    System.out.println(accountList);
                }
            
                @Test
                //查询一个
                public void testQueryOne(){
                    Account account = jdbcTemplate.queryForObject("select * from account where name=?", new BeanPropertyRowMapper<Account>(Account.class),"cat");
                    System.out.println(account);
                }
                @Test
                //查询总数
                public void testQueryTotalCount(){
                    Long count = jdbcTemplate.queryForObject("select count(*) from account", long.class);
                    System.out.println(count);
                }
            
            }
            ```

            ___

            ## 事务控制

            - 基于xml的事务控制

            - 导包

              ```xml
                      <dependency>
                          <groupId>org.springframework</groupId>
                          <artifactId>spring-tx</artifactId>
                          <version>5.0.5.RELEASE</version>
                      </dependency>
              ```

            - ```xml
              <?xml version="1.0" encoding="UTF-8"?>
              <beans xmlns="http://www.springframework.org/schema/beans"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xmlns:context="http://www.springframework.org/schema/context"
                     xmlns:tx="http://www.springframework.org/schema/tx"
                     xmlns:aop="http://www.springframework.org/schema/aop"
                     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                         http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
                                         http://www.springframework.org/schema/tx  http://www.springframework.org/schema/tx/spring-tx.xsd
                                         http://www.springframework.org/schema/aop  http://www.springframework.org/schema/aop/spring-aop.xsd ">
              
                      <!--
                      spring中基于xml的声明式事务控制配置步骤
                          1. 配置事务管理器
                          2. 配置事务通知
                              此时需要导入事务的约束 tx名称空间与约束，同时也需要aop
                              使用 tx:advice 标签配置事务通知
                                  属性：
                                      id：给事务通知起唯一标志
                                      transaction-manager：给事务通知提供事务管理器
                          3. 配置aop中的通用切入点表达式
                          4. 建立事务通知和切入点表达式的对应关系
                              使用 aop:advisor
                                  属性：
                                      advice-ref：通知的引用
                          5. 配置事务的属性
                              在事务的通知tx:advice标签内部配置
                   -->
                 
                  <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
              
                  <bean id="dataSourse" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
                      <property name="driverClassName" value="${jdbc.driver}"></property>
                      <property name="url" value="${jdbc.url}"></property>
                      <property name="username" value="${jdbc.username}"></property>
                      <property name="password" value="${jdbc.password}"></property>
                  </bean>
              
                  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
                      <property name="dataSource" ref="dataSourse"></property>
                  </bean>
              
                  <bean id="accountDao" class="com.wudonglong.dao.impl.AccountDaoImpl">
                      <property name="jdbcTemplate" ref="jdbcTemplate"></property>
                  </bean>
              
              <!--    目标对象  内部的方法就是切点-->
                  <bean id="accountService" class="com.wudonglong.service.impl.AccountServiceImpl">
                      <property name="accountDao" ref="accountDao"></property>
                  </bean>
              
              
                  <!--        配置平台事务管理器-->
                  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
                      <property name="dataSource" ref="dataSourse"></property>
                  </bean>
              
              
                  <!--    通知   事务的增强-->
                  <tx:advice id="txAdvice" transaction-manager="transactionManager">
                      <!--
                          配置事务的属性
                              name:事务层中执行的方法名
                                  * ：所有方法
                                  find* : "find"开头的所有方法，优先级高于 *
                              isolation：  用于指定事务的隔离级别。默认值是DEFAULT，表示使用数据库的默认隔离级别
                              propagation：用于指定事务的传播行为。默认值是REQUIRED，表示一定会有事务，增删改的选择。查询方法可以使用SUPPORTS
                              read-only：用于指定事务是否只读。只有查询方法才能指定只读。默认值是false，表示读写
                              timeout：用于指定事务的超时事件，默认值-1，表示是永不超时。如果指定了数值，以秒为单位
                              rollback-for：用于指定一个异常，当产生该异常时事务回滚，产生其他异常事务不回滚。没有默认值。表示任何异常都回滚
                              no-rollback-for：用于指定一个异常，当产生该异常时事务不回滚，产生其他异常事务回滚。没有默认值。表示任何异常都回滚
                      -->
                      <tx:attributes>
                          <tx:method name="*"/> <!--业务层中所有的方法 
              									*find：业务层中所有的find方法-->
                      </tx:attributes>
                  </tx:advice>
              
              
                  <!--    配置事务的aop织入-->
                  <aop:config>
                      <!-- 一个通知的的切面 -->
                      <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.wudonglong.service.impl.*.*(..))"></aop:advisor>
                  </aop:config>
              
              
              
              
              
              
              
              
              
                  <!-- 配置业务层-->
                  <bean id="accountService" class="com.AccountServiceImpl">
                      <property name="accountDao" ref="accountDao"></property>
                  </bean>
              
              
              
                  <!-- 配置数据源-->
                  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
                      <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
                      <property name="url" value="jdbc:mysql://localhost:3306/eesy"></property>
                      <property name="username" value="root"></property>
                      <property name="password" value="1234"></property>
                  </bean>
                  <!-- 配置账户的持久层-->
                  <bean id="accountDao" class="com.AccountDaoImpl">
                      <property name="dataSource" ref="dataSource"></property>
                  </bean>
              
              </beans>
              ```

              ___

            - 基于注解的事务控制（项目与xml配置开发一样）

              - **在xml中需要配置才能正常使用注解开发**

              ```xml
              <!--    事务的注解驱动-->
                  <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
              ```

              - Service层代码

              ```java
              //@Transactional
              public class AccountServiceImpl implements AccountService {
              
                  @Autowired
                  private AccountDao accountDao;
              
                  //事务属性
                  @Transactional(...)
                  public void transfer(String outMan, String inMan, double money){
                      accountDao.out(outMan,money);
              //        int i = 1/0;  抛出异常时停止该事务，不能转账
                      accountDao.in(inMan,money);
                  }
              }
              ```

            - 仿写的web层

              ```java
              public class AccountController {
                  public static void main(String[] args) {
              
                      ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
                      AccountService accountService = (AccountService) app.getBean("accountService");
                      accountService.transfer("cat2","cat1",634);
                  }
              }
              ```

              

      ## 

  

