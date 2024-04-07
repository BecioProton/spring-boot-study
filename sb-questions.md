🔹 Basic Spring Boot Questions

1.  What is Spring Boot, and how is it different from the Spring Framework?

          Spring Boot is an extension of the Spring Framework designed to simplify the development of Spring-based applications. It eliminates the need for extensive XML configuration and enables rapid application development by providing auto-configuration, embedded servers, and dependency management.

    🔹 Key Differences:

    - Spring Framework requires manual configuration, whereas Spring Boot provides auto-configuration.
    - Spring Boot includes an embedded server (Tomcat, Jetty, etc.), while Spring Framework needs an external setup.
    - Spring Boot provides opinionated defaults, whereas Spring Framework is flexible but requires more setup.

2.  What are the advantages of using Spring Boot?

    - Auto-configuration reduces manual configuration.
    - Embedded servers (Tomcat, Jetty) eliminate the need for external application servers.
    - Simplified dependency management through Spring Boot Starters.
    - roduction-ready features like monitoring, logging, and metrics via Actuator.
    - Microservices support with built-in tools for cloud deployment.
    - Reduced boilerplate code, making development faster and more efficient.

3.  What is the purpose of the `@SpringBootApplication` annotation?

    @SpringBootApplication is a convenience annotation that combines three important annotations:

        @SpringBootConfiguration  // Marks the class as a configuration class
        @EnableAutoConfiguration  // Enables Spring Boot’s auto-configuration feature
        @ComponentScan            // Scans for Spring components in the package

    This annotation marks the main entry point of a Spring Boot application.

    Example:

    ```
    @SpringBootApplication
    public class MyApp {
        public static void main(String[] args) {
            SpringApplication.run(MyApp.class, args);
        }
    }
    ```

4.  What is auto-configuration in Spring Boot, and how does it work?

        Auto-configuration is a feature that automatically configures Spring Beans based on the dependencies present in the classpath.

    How it works:

        Spring Boot scans the classpath for dependencies (e.g., if spring-boot-starter-data-jpa is present, Hibernate and JPA will be auto-configured).
        It applies sensible default configurations.
        Developers can override configurations using properties in application.properties or by defining custom beans.

    Example: If you add Spring Data JPA, Spring Boot auto-configures a DataSource and EntityManager without manual setup. 
    
5. How does Spring Boot simplify dependency management?

    Spring Boot uses Spring Boot Starters to manage dependencies efficiently.

    🔹 Key Features:

    It provides predefined dependency sets (spring-boot-starter-web, spring-boot-starter-data-jpa, etc.).
    It manages compatible dependency versions automatically.
    It uses Bill of Materials (BOM) to maintain version consistency.

    Example (pom.xml):
    ```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    Here, Spring Boot automatically resolves all necessary dependencies like Spring MVC and an embedded Tomcat server. 
6. What is a Spring Boot starter, and can you name some commonly used starters?

        A Spring Boot Starter is a dependency that includes all the required libraries to simplify configuration.

    Common Spring Boot Starters:
    * Starter Description
    * spring-boot-starter-web For building web applications (Spring MVC + Tomcat)
    * spring-boot-starter-data-jpa For working with JPA and Hibernate
    * spring-boot-starter-security For implementing security and authentication
    * spring-boot-starter-test For unit and integration testing
    * spring-boot-starter-actuator For monitoring and application health checks
    * spring-boot-starter-thymeleaf For integrating Thymeleaf templates
    * spring-boot-starter-mail For sending emails with JavaMailSender

    Spring Boot starters reduce configuration complexity by bundling commonly used dependencies. 

7. How does Spring Boot handle dependency injection?

    Spring Boot uses Spring's Dependency Injection (DI) mechanism to manage bean dependencies.

    📌 Key Features:

        Automatically injects dependencies using @Autowired.
        Uses IoC (Inversion of Control) to manage object lifecycles.
        Supports constructor injection, setter injection, and field injection.
        Allows explicit bean definitions via @Bean methods.

    Example (Constructor Injection):

    @Service
    public class UserService {
    private final UserRepository userRepository;

        @Autowired
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }

    }

    Here, UserService automatically receives an instance of UserRepository. 
8. What is the difference between @Component, @Service, and @Repository annotations?

    All three annotations are used for defining Spring Beans, but they have different purposes:
    Annotation Purpose
    @Component Generic annotation for Spring-managed beans
    @Service Specialized for service layer components (business logic)
    @Repository Specialized for DAO (Data Access Objects) and database interactions

    Example:

    @Component
    public class GeneralComponent { } // Generic bean

    @Service
    public class MyService { } // Business logic

    @Repository
    public class MyRepository { } // Database interaction

    Although they behave similarly, @Service and @Repository provide additional metadata for service logic and exception handling. 

9. What is the purpose of the @Autowired annotation, and how does it work?

    @Autowired is used for automatic dependency injection in Spring Boot.

    How it works:

        Spring scans for a matching bean and injects it automatically.
        Works with constructor injection (recommended), field injection, and setter injection.
        If multiple beans match, Spring throws an exception unless a qualifier (@Qualifier) or @Primary is specified.

    Example (Field Injection):

    @Service
    public class UserService {
    @Autowired
    private UserRepository userRepository;
    }

    Example (Constructor Injection - Preferred):

    @Service
    public class UserService {
    private final UserRepository userRepository;

        @Autowired
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }

    }

    Using constructor injection is preferred because it ensures immutability and testability. 
10. What are the different ways to define beans in Spring Boot?

    Spring Boot provides multiple ways to define beans:

    1️⃣ Using @Component, @Service, or @Repository (Auto-Scanning)
    ```
        @Component
        public class MyComponent { }
    ```

    2️⃣ Using @Bean in a Configuration Class
    ```
        @Configuration
        public class AppConfig {
            @Bean
            public MyComponent myComponent() {
                return new MyComponent();
            }
        }
    ```
    3️⃣ Using XML Configuration (Rarely Used in Spring Boot)

    <bean id="myComponent" class="com.example.MyComponent"/>

    Spring Boot prefers annotation-based bean definition for simplicity and better maintainability.

🔹 Intermediate Spring Boot Questions

1.  How does Spring Boot handle application properties and configuration?

Spring Boot handles application properties and configuration by using a set of default configuration settings that can be easily overridden in the application.properties or application.yml files. These files can contain properties to configure beans, server settings, logging, etc. Spring Boot automatically loads these files from the classpath (usually in src/main/resources).

📌 How it works:

    Properties from application.properties or application.yml are loaded at application startup.
    These values can be accessed and injected into beans using @Value, @ConfigurationProperties, or Environment.
    Profiles (e.g., application-dev.properties) allow for environment-specific configurations.

2. What is the difference between application.properties and application.yml?

Both application.properties and application.yml are used to configure Spring Boot applications, but the main difference lies in the format:

    application.properties is a key-value pair format (simple and flat).

server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/mydb

application.yml (YAML) is a hierarchical structure (supports nesting).

    server:
      port: 8080
    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/mydb

YAML is generally more readable and better suited for complex configurations, while properties files are simpler and more familiar to many developers. 3. How do you create custom properties in Spring Boot and access them?

To create custom properties in Spring Boot:

    Define custom properties in application.properties or application.yml.

myapp.custom.property1=value1
myapp.custom.property2=value2

Access them in a Spring bean using @Value or @ConfigurationProperties.

    Using @Value:

@Value("${myapp.custom.property1}")
private String property1;

Using @ConfigurationProperties (more robust for grouping related properties):

        @ConfigurationProperties(prefix = "myapp.custom")
        @Component
        public class CustomProperties {
            private String property1;
            private String property2;

            // getters and setters
        }

For @ConfigurationProperties, make sure to enable it in the main class by using @EnableConfigurationProperties if not automatically enabled. 4. What is the difference between @Value and @ConfigurationProperties?

    @Value: Used for injecting individual property values into fields.

@Value("${myapp.custom.property1}")
private String property1;

Use case: Injecting single values, typically for simple configuration properties.

@ConfigurationProperties: Used for binding a group of related properties to a Java object.

    @ConfigurationProperties(prefix = "myapp.custom")
    public class CustomProperties {
        private String property1;
        private String property2;
        // getters and setters
    }

    Use case: Grouping related properties and providing them as a strongly-typed object.

@ConfigurationProperties is more scalable and useful when you have a complex or large set of properties, while @Value is for simpler cases. 5. What is the purpose of the @Bean annotation in Spring Boot?

The @Bean annotation is used to define bean methods within a @Configuration class. It tells Spring that the method should be executed and its return value should be registered as a Spring Bean in the application context.

Example:

@Configuration
public class AppConfig {
@Bean
public MyService myService() {
return new MyServiceImpl();
}
}

Purpose: To define and configure beans manually (typically when the bean creation requires some custom logic). 6. What is the use of @Qualifier, and when should you use it?

@Qualifier is used to resolve ambiguity when there are multiple beans of the same type and Spring needs to know which one to inject. It is typically used alongside @Autowired.

Example:

@Autowired
@Qualifier("mySpecificService")
private Service myService;

When to use: When there are multiple candidates for injection, @Qualifier helps identify which one should be injected. 7. What are the different types of dependency injection supported by Spring Boot?

Spring Boot supports three types of dependency injection:

    Constructor Injection (Recommended):
        Dependencies are injected through the constructor.
        Ensures immutability and testability.

@Autowired
public MyService(MyRepository repository) { ... }

Setter Injection:

    Dependencies are injected via setter methods.
    Useful when the dependency is optional or requires some post-construction initialization.

@Autowired
public void setRepository(MyRepository repository) { ... }

Field Injection:

    Dependencies are injected directly into fields.
    Least preferred because it’s difficult to test and can lead to non-immutable objects.

    @Autowired
    private MyRepository repository;

8. What is Spring Boot Actuator, and how does it help in monitoring applications?

Spring Boot Actuator provides production-ready features to help monitor and manage your application. It exposes a set of REST endpoints that provide health checks, metrics, environment information, application status, and more.

Some useful endpoints provided by Spring Boot Actuator include:

    /actuator/health - Health status of the application
    /actuator/metrics - Application metrics like memory usage, active threads, etc.
    /actuator/env - Properties and environment details

How it helps: It simplifies the process of monitoring Spring Boot applications in production by providing easily accessible information about system health and performance. 9. What is the role of CommandLineRunner and ApplicationRunner in Spring Boot?

Both CommandLineRunner and ApplicationRunner are functional interfaces that allow you to execute code after the Spring Boot application has started.

    CommandLineRunner:
        It provides access to command-line arguments (String[] args).
        Useful for executing initialization logic after the application has started.

@Component
public class MyCommandLineRunner implements CommandLineRunner {
@Override
public void run(String... args) throws Exception {
// Your code here
}
}

ApplicationRunner:

    Similar to CommandLineRunner, but provides access to the ApplicationArguments interface, which gives more information about the application's arguments.

    @Component
    public class MyApplicationRunner implements ApplicationRunner {
        @Override
        public void run(ApplicationArguments args) throws Exception {
            // Your code here
        }
    }

Both are typically used for running tasks at startup. 10. How do you enable and use profiles in Spring Boot?

In Spring Boot, profiles allow you to define environment-specific configurations (e.g., development, production).

    Enable profiles in application.properties:

spring.profiles.active=dev

Define profile-specific properties in different files:

    application-dev.properties for the development environment
    application-prod.properties for the production environment

Use @Profile annotation to specify which beans should be loaded for specific profiles:

    @Profile("dev")
    @Bean
    public MyDevService myDevService() {
        return new MyDevService();
    }

Spring Boot will automatically pick the correct properties based on the active profile and load the corresponding beans.

🔹 Advanced Spring Boot Questions

    1. What is the difference between @EnableAutoConfiguration and @SpringBootApplication?

    @EnableAutoConfiguration: This annotation is used to automatically configure Spring Beans based on the classpath settings, other beans, and various property settings. It tells Spring Boot to start applying configuration based on the libraries in the classpath.

    @SpringBootApplication: This is a meta-annotation that combines three key annotations:
        @EnableAutoConfiguration
        @ComponentScan
        @Configuration

    It provides a convenient and concise way to enable auto-configuration, component scanning, and the configuration of beans in a Spring Boot application.

In short, @SpringBootApplication includes @EnableAutoConfiguration but adds more functionality, such as component scanning and configuration. 2. How does Spring Boot determine which beans to autoconfigure?

Spring Boot uses Auto Configuration to automatically configure beans based on the dependencies found in the classpath. Here's how it works:

    Spring Boot inspects the dependencies in the classpath (e.g., if spring-boot-starter-web is present, Spring Boot will automatically configure a web server).
    It uses the @EnableAutoConfiguration annotation to apply a set of pre-defined configurations that match the available libraries.
    Spring Boot also checks for conditions based on classpath or other configuration properties (via @Conditional annotations like @ConditionalOnClass, @ConditionalOnProperty, etc.).

3. How can you disable a specific auto-configuration in Spring Boot?

You can disable a specific auto-configuration in Spring Boot by using the @EnableAutoConfiguration annotation's exclude attribute or by using spring.autoconfigure.exclude in application.properties or application.yml.

Example using @EnableAutoConfiguration:

@EnableAutoConfiguration(exclude = DataSourceAutoConfiguration.class)
public class MyApp {
public static void main(String[] args) {
SpringApplication.run(MyApp.class, args);
}
}

Example using application.properties:

spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration

This will exclude the DataSourceAutoConfiguration from being applied, which can be useful if you don’t need a database in your application. 4. What is the purpose of the spring.factories file?

The spring.factories file is part of the Spring Boot ServiceLoader mechanism and is used for auto-configuration and loading custom configurations in a Spring Boot application. It defines a list of classes or configuration beans that Spring Boot can use.

    Location: Typically found under src/main/resources/META-INF/spring.factories.
    Purpose: Allows Spring Boot to discover and register auto-configuration classes without explicitly mentioning them in the code.

Example:

org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyCustomAutoConfiguration

Spring Boot will automatically load the MyCustomAutoConfiguration class as part of its auto-configuration mechanism. 5. What is the ApplicationContext, and how does it work in Spring Boot?

The ApplicationContext is the central interface to the Spring Framework's Inversion of Control (IoC) container. It manages beans and their lifecycle in a Spring application. Spring Boot creates an ApplicationContext during application startup and provides it to manage the beans.

    Role: It holds the beans defined in the application, manages their dependencies, and ensures that they are initialized, wired, and ready for use.
    How it works: When the application starts, Spring Boot initializes the ApplicationContext, scans for @Component, @Service, @Repository, and other bean definitions, and wires them together as needed.

6. How can you override default Spring Boot configurations?

You can override default Spring Boot configurations in several ways:

    Using application.properties or application.yml: Override built-in properties by defining your own values.

server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/mydb

Using @Configuration and @Bean: Define your own beans or override Spring Boot's default beans in a custom configuration class.

    @Configuration
    public class MyConfig {
        @Bean
        public MyService myService() {
            return new MyServiceImpl();
        }
    }

    Using Profiles: You can create separate configuration files for different profiles (application-dev.properties, application-prod.properties), and Spring Boot will automatically load the appropriate configuration based on the active profile.

7. What is a @ConditionalOnProperty, and when would you use it?

@ConditionalOnProperty is a conditional annotation used to register a bean or a configuration only if a specific property is defined and has a certain value. It's useful for configuring beans conditionally based on external configuration or property settings.

Example:

@Bean
@ConditionalOnProperty(name = "myapp.featureEnabled", havingValue = "true")
public MyFeatureService myFeatureService() {
return new MyFeatureServiceImpl();
}

When to use it: Use this annotation when you want to enable/disable beans or configurations based on the presence and value of a property. For example, enabling certain features only when a specific configuration property is set. 8. How do you implement and register a custom Spring Boot Starter?

A custom Spring Boot Starter is a reusable, self-contained module that can encapsulate common configuration and dependencies for a specific use case.

To implement and register a custom starter:

    Create a new module (typically a Maven or Gradle project) to house your starter.
    Define auto-configuration classes using @Configuration or @ConfigurationProperties to configure beans or services.
    Create a META-INF/spring.factories file to specify auto-configuration classes.
    Package your starter and make it available via a Maven/Gradle repository.

Example of spring.factories:

org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.MyCustomAutoConfiguration

9. How do you manage transactions in Spring Boot applications?

Spring Boot provides built-in support for managing transactions with Spring's @Transactional annotation.

    Declarative Transactions: Use @Transactional to specify the scope of a transaction.

    @Transactional
    public void myTransactionalMethod() {
        // Transactional code here
    }

    Configuration: Spring Boot automatically configures a transaction manager (like DataSourceTransactionManager or JpaTransactionManager) if Spring Data JPA or JDBC is present.

    Propagation and Isolation: You can specify transaction propagation and isolation levels with the @Transactional annotation.

10. What is Spring Boot DevTools, and how does it help during development?

Spring Boot DevTools provides a set of tools to improve the development experience by enabling features like automatic restarts, live reload, and enhanced logging.

    Automatic Restart: DevTools watches for changes in the classpath and automatically restarts the application when changes are detected, making it easier to test and debug.
    LiveReload: Works with front-end tools (like the browser) to reload content when changes are made.
    Enhanced Logging: Provides better logging for debugging purposes during development.

To enable Spring Boot DevTools, simply add the dependency to your pom.xml or build.gradle:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
</dependency>

How it helps: It significantly improves the development speed by providing fast feedback loops during application changes.

🔹 Spring Boot Web & REST Questions

1. What is the difference between @Controller and @RestController?

   @Controller: This is a generic annotation used to define a controller in a Spring MVC application. It is typically used for handling web pages and rendering views (using technologies like Thymeleaf or JSP). Methods in a class annotated with @Controller return views (HTML, JSP, etc.) by default, unless @ResponseBody is used to return data directly.

   @RestController: This is a convenience annotation that combines @Controller and @ResponseBody. It is used to define RESTful web services where the methods return JSON or XML data directly (without views). Every method in a class annotated with @RestController automatically assumes the @ResponseBody behavior, meaning that the return values are serialized directly into HTTP responses.

2. How do you handle exceptions in a Spring Boot REST API?

In Spring Boot, you can handle exceptions globally or locally:

    Global Exception Handling: Use @ControllerAdvice to define a global exception handler for all controllers.

@ControllerAdvice
public class GlobalExceptionHandler {
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
ErrorResponse errorResponse = new ErrorResponse("Resource not found", ex.getMessage());
return new ResponseEntity<>(errorResponse, HttpStatus.NOT_FOUND);
}
}

Local Exception Handling: Handle exceptions in individual controllers using @ExceptionHandler.

    @RestController
    public class MyController {
        @GetMapping("/resource/{id}")
        public Resource getResource(@PathVariable Long id) {
            if (id == null) {
                throw new ResourceNotFoundException("Resource not found with ID: " + id);
            }
            return resourceService.findById(id);
        }

        @ExceptionHandler(ResourceNotFoundException.class)
        public ResponseEntity<String> handleResourceNotFound(ResourceNotFoundException ex) {
            return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
        }
    }

3. What is @RequestMapping, and how is it different from @GetMapping and @PostMapping?

   @RequestMapping: It is a general-purpose annotation used to map HTTP requests to handler methods of MVC and REST controllers. It supports all HTTP methods (GET, POST, PUT, DELETE, etc.).

@RequestMapping("/resource")
public String getResource() {
return "Resource";
}

@GetMapping: A specialized version of @RequestMapping for HTTP GET requests. It is used to handle GET requests specifically.

@GetMapping("/resource")
public String getResource() {
return "Resource";
}

@PostMapping: A specialized version of @RequestMapping for HTTP POST requests. It is used to handle POST requests specifically.

    @PostMapping("/resource")
    public String createResource(@RequestBody Resource resource) {
        return "Resource created";
    }

4. How do you pass request parameters using @RequestParam vs. @PathVariable?

   @RequestParam: This is used to retrieve query parameters from the URL. For example, you can use @RequestParam to extract parameters from the query string.

@GetMapping("/greet")
public String greet(@RequestParam String name) {
return "Hello, " + name;
}
// URL: /greet?name=John

@PathVariable: This is used to retrieve parameters from the path of the URL (i.e., URI templates). It's used when you need to extract variables directly from the URL path.

    @GetMapping("/greet/{name}")
    public String greet(@PathVariable String name) {
        return "Hello, " + name;
    }
    // URL: /greet/John

5. How can you enable Cross-Origin Resource Sharing (CORS) in Spring Boot?

CORS can be enabled globally or locally:

    Globally: Use @Configuration and WebMvcConfigurer to configure CORS settings for all controllers.

@Configuration
public class WebConfig implements WebMvcConfigurer {
@Override
public void addCorsMappings(CorsRegistry registry) {
registry.addMapping("/\*_")
.allowedOrigins("http://localhost:3000") // Allow CORS from specific origin
.allowedMethods("GET", "POST", "PUT", "DELETE")
.allowedHeaders("_");
}
}

Locally: You can also enable CORS for specific controller methods using @CrossOrigin:

    @RestController
    @RequestMapping("/api")
    public class MyController {
        @CrossOrigin(origins = "http://localhost:3000")
        @GetMapping("/resource")
        public String getResource() {
            return "Resource data";
        }
    }

6. How do you configure Spring Boot to use a specific port?

You can configure the port in application.properties or application.yml:

    In application.properties:

server.port=8081

In application.yml:

    server:
      port: 8081

Alternatively, you can also set the port as a command-line argument:

java -jar myapp.jar --server.port=8081

7. What is the use of @ResponseBody in a Spring Boot application?

@ResponseBody is used to indicate that the return value of a method should be written directly to the HTTP response body. It is typically used in RESTful APIs to return JSON, XML, or other data formats instead of rendering views.

    Example:

    @RestController
    public class MyController {
        @GetMapping("/resource")
        @ResponseBody
        public Resource getResource() {
            return new Resource();
        }
    }

In the case of @RestController, @ResponseBody is automatically applied to all methods. 8. How do you handle file uploads in Spring Boot?

To handle file uploads in Spring Boot:

    Make sure your application can accept multi-part file uploads by adding the following in application.properties:

spring.servlet.multipart.enabled=true

Use @RequestParam to handle the uploaded file in your controller:

    @PostMapping("/upload")
    public String handleFileUpload(@RequestParam("file") MultipartFile file) {
        // Process the file (e.g., save to disk)
        return "File uploaded successfully";
    }

9. What are interceptors in Spring Boot, and how do you use them?

Interceptors allow you to pre-process and post-process HTTP requests in a Spring application. They can be used for logging, authentication, and modifying requests or responses.

    To use an interceptor, you implement HandlerInterceptor and register it in a configuration class:

    public class MyInterceptor implements HandlerInterceptor {
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
            // Pre-process request
            return true;  // Continue with the request
        }

        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
            // Post-process response
        }
    }

    @Configuration
    public class WebConfig implements WebMvcConfigurer {
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
            registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**");
        }
    }

10. What is the difference between @RestControllerAdvice and @ExceptionHandler?

    @RestControllerAdvice: This is a specialized version of @ControllerAdvice for RESTful applications. It is used to globally handle exceptions and customize the response for API calls. It can also be used to apply advice on model attributes or binders.

@RestControllerAdvice
public class GlobalExceptionHandler {
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
}
}

@ExceptionHandler: This is used within controllers or @ControllerAdvice to handle specific exceptions at the controller level. It is applied to individual methods to handle exceptions that occur within that controller.

    @RestController
    public class MyController {
        @ExceptionHandler(ResourceNotFoundException.class)
        public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
            return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
        }
    }

@RestControllerAdvice is used for global exception handling, while @ExceptionHandler is for handling exceptions locally within controllers.

🔹 Spring Boot Data & JPA Questions

    1. How does Spring Boot integrate with JPA and Hibernate?

Spring Boot integrates with JPA and Hibernate by automatically configuring the necessary beans and dependencies for you. The primary components are:

    Spring Data JPA: Spring Boot provides easy configuration for JPA repositories. It uses @Entity to map Java objects to database tables, and repositories such as CrudRepository, JpaRepository, etc., for easy CRUD operations.
    Hibernate: Hibernate is the default JPA implementation in Spring Boot. Spring Boot automatically configures Hibernate as the JPA provider, including setting up the EntityManagerFactory and TransactionManager.
    DataSource Configuration: Spring Boot uses properties in application.properties or application.yml to configure the database connection, such as the database URL, username, password, and dialect.

Example in application.properties:

spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=update

2. What is the purpose of the @Entity annotation in Spring Boot?

The @Entity annotation is used to mark a class as a JPA entity, which means the class is a persistent object that will be mapped to a database table. It tells Hibernate (or another JPA provider) to treat the class as a table row in the database and map its fields to columns.

@Entity
public class Product {
@Id
private Long id;
private String name;
private double price;
}

3. What is the difference between @OneToOne, @OneToMany, and @ManyToMany in JPA?

   @OneToOne: Establishes a one-to-one relationship between two entities. Each instance of one entity is associated with exactly one instance of the other.

@OneToOne
private Address address;

@OneToMany: Defines a one-to-many relationship, where one entity instance can be associated with multiple instances of the other entity.

@OneToMany(mappedBy = "customer")
private List<Order> orders;

@ManyToMany: Defines a many-to-many relationship, where many instances of one entity are associated with many instances of the other entity.

    @ManyToMany
    @JoinTable(name = "user_roles", joinColumns = @JoinColumn(name = "user_id"), inverseJoinColumns = @JoinColumn(name = "role_id"))
    private Set<Role> roles;

4. How does Spring Boot handle database migrations with Flyway and Liquibase?

   Flyway: Flyway is a database migration tool that allows you to version control your database schema. In Spring Boot, you can configure Flyway by adding the Flyway dependency to your pom.xml and placing migration scripts in the src/main/resources/db/migration folder. Flyway will automatically run these scripts at application startup.

   Example Maven dependency:

<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>

Liquibase: Liquibase is another database migration tool. Spring Boot can automatically configure Liquibase by adding the necessary dependencies. Liquibase uses XML, YAML, or JSON files to define migration changes.

Example Maven dependency:

    <dependency>
        <groupId>org.liquibase</groupId>
        <artifactId>liquibase-core</artifactId>
    </dependency>

5. What is the difference between CrudRepository, JpaRepository, and PagingAndSortingRepository?

   CrudRepository: This is the simplest repository that provides CRUD (Create, Read, Update, Delete) operations for an entity. It provides methods like save(), findById(), delete(), etc.

   JpaRepository: Extends CrudRepository and adds additional JPA-specific operations, like batch operations (flush()), pagination (findAll(Pageable pageable)), and sorting (findAll(Sort sort)).

   PagingAndSortingRepository: Extends CrudRepository and provides additional functionality for pagination and sorting, but it does not include JPA-specific methods.

In summary:

    Use CrudRepository for basic CRUD operations.
    Use JpaRepository for more advanced JPA-related operations.
    Use PagingAndSortingRepository when you need pagination and sorting.

6. How does caching work in Spring Boot, and what annotations are used for it?

Spring Boot supports caching via the @Cacheable, @CachePut, and @CacheEvict annotations:

    @Cacheable: This annotation is used to cache the result of a method. The method will return the cached result if it has been called previously with the same parameters.

@Cacheable("products")
public Product getProduct(Long id) {
return productRepository.findById(id).orElse(null);
}

@CachePut: Used to update the cache with a new value every time the method is called, regardless of whether the method has been invoked before.

@CacheEvict: Used to evict (remove) entries from the cache.

    @CacheEvict("products")
    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }

To enable caching in Spring Boot, you can add @EnableCaching to your configuration class. 7. What is lazy loading vs. eager loading in Hibernate?

    Lazy Loading: In lazy loading, related entities are not loaded until they are explicitly accessed. This is the default behavior for most associations in JPA (@OneToMany, @ManyToMany).

@ManyToOne(fetch = FetchType.LAZY)
private Customer customer;

Eager Loading: In eager loading, related entities are loaded immediately along with the parent entity. This is useful when you always need the associated data and don’t want to incur multiple database calls.

    @ManyToOne(fetch = FetchType.EAGER)
    private Customer customer;

8. How do you handle transactions in Spring Boot with @Transactional?

The @Transactional annotation is used to manage transactions declaratively. It ensures that methods run within a transaction, providing automatic commit/rollback behavior based on the method outcome (success or failure).

    Example:

    @Transactional
    public void transferMoney(Account from, Account to, BigDecimal amount) {
        from.withdraw(amount);
        to.deposit(amount);
    }

If any exception occurs in the method, the transaction will be rolled back by default. You can customize this behavior using attributes like rollbackFor, noRollbackFor, etc. 9. How does Spring Boot support NoSQL databases like MongoDB and Redis?

    MongoDB: Spring Boot integrates with MongoDB using the spring-boot-starter-data-mongodb dependency. It automatically configures the MongoTemplate and MongoRepository for MongoDB operations.

    Example Maven dependency for MongoDB:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>

Redis: Spring Boot supports Redis using the spring-boot-starter-data-redis dependency. It integrates seamlessly for caching and other Redis-based use cases like message queues.

Example Maven dependency for Redis:

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

10. What is optimistic vs. pessimistic locking in Hibernate?

    Optimistic Locking: Optimistic locking is based on the assumption that no conflict will happen when multiple transactions try to update the same entity. The version number or timestamp is checked before committing changes. If there is a conflict (the version is outdated), an exception is thrown.

@Version
private int version;

Pessimistic Locking: Pessimistic locking assumes that conflicts will happen, and it locks the resource (typically a database row) for the entire transaction. Other transactions are blocked until the lock is released.

@Lock(LockModeType.PESSIMISTIC_WRITE)
public void lockResource(Long id) {
// perform locked operation
}

🔹 Spring Boot Security Questions

1. How does Spring Boot integrate with Spring Security?

Spring Boot integrates with Spring Security by automatically configuring a default security setup for web applications, including basic authentication, HTTP basic authentication, or form-based login. Spring Boot automatically adds the spring-boot-starter-security dependency and configures a default security configuration if no custom configuration is provided. You can customize Spring Security by creating a custom security configuration class annotated with @EnableWebSecurity and overriding the configure method to specify authentication, authorization, and other security settings.

Example of a basic Spring Boot security configuration:

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
@Override
protected void configure(HttpSecurity http) throws Exception {
http
.authorizeRequests()
.antMatchers("/admin/\*\*").hasRole("ADMIN")
.anyRequest().authenticated()
.and()
.formLogin()
.permitAll();
}

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .withUser("user").password("{noop}password").roles("USER")
            .and()
            .withUser("admin").password("{noop}admin").roles("ADMIN");
    }

}

2. What is the difference between authentication and authorization?

   Authentication: The process of verifying the identity of a user or system. Authentication ensures that the entity is who it claims to be, usually through credentials such as a username and password, tokens, or biometric data.

   Authorization: The process of granting access to resources or actions based on the authenticated entity's permissions. After authentication, authorization determines whether the authenticated entity is allowed to access a particular resource or perform a specific action.

3. What is the purpose of @PreAuthorize and @PostAuthorize?

   @PreAuthorize: This annotation allows method-level security by enforcing authorization checks before a method is executed. It is used to specify authorization rules based on a user's roles, permissions, or other attributes.

@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long userId) {
// method logic
}

@PostAuthorize: This annotation allows method-level security by enforcing authorization checks after a method has executed. It's typically used when the return value of the method needs to be checked for authorization.

    @PostAuthorize("returnObject.owner == authentication.name")
    public User getUser(Long userId) {
        // method logic
        return userRepository.findById(userId).orElse(null);
    }

4. How can you implement JWT-based authentication in Spring Boot?

To implement JWT (JSON Web Token) authentication in Spring Boot:

    Generate JWT Token: Implement a service to generate a JWT token when the user logs in, signing the token with a secret key.
    Validate JWT Token: Create a filter that intercepts incoming requests, checks for the presence of a JWT token in the request header, and validates it.
    Security Configuration: Configure Spring Security to use this filter and restrict access to specific endpoints based on the token.

Example steps:

    JWT Filter:

@Component
public class JwtFilter extends OncePerRequestFilter {
@Autowired
private JwtUtil jwtUtil;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        String token = extractToken(request);
        if (token != null && jwtUtil.isValidToken(token)) {
            Authentication authentication = jwtUtil.getAuthentication(token);
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }
        filterChain.doFilter(request, response);
    }

    private String extractToken(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7);
        }
        return null;
    }

}

JWT Utility (Generate, Validate):

    public class JwtUtil {
        private String secretKey = "secret";

        public String generateToken(String username) {
            return Jwts.builder()
                    .setSubject(username)
                    .setIssuedAt(new Date())
                    .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60))  // 1 hour expiry
                    .signWith(SignatureAlgorithm.HS256, secretKey)
                    .compact();
        }

        public boolean isValidToken(String token) {
            try {
                Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token);
                return true;
            } catch (Exception e) {
                return false;
            }
        }

        public Authentication getAuthentication(String token) {
            String username = extractUsername(token);
            return new UsernamePasswordAuthenticationToken(username, null, Collections.emptyList());
        }

        private String extractUsername(String token) {
            return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody().getSubject();
        }
    }

5. How do you configure OAuth2 authentication in a Spring Boot application?

Spring Boot integrates with OAuth2 via spring-boot-starter-oauth2-client. To configure OAuth2 authentication:

    Add the necessary OAuth2 client dependencies (spring-boot-starter-oauth2-client) in pom.xml.
    Configure the OAuth2 client settings in application.properties or application.yml.

Example configuration:

spring.security.oauth2.client.registration.google.client-id=your-client-id
spring.security.oauth2.client.registration.google.client-secret=your-client-secret
spring.security.oauth2.client.registration.google.scope=profile,email
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}

You can then use @EnableOAuth2Sso to automatically configure single sign-on or use @AuthenticationPrincipal to access the authenticated user. 6. What is CSRF protection, and how does Spring Security handle it?

    CSRF (Cross-Site Request Forgery) is an attack where a malicious user tricked into performing an unintended action in a web application where they are authenticated.
    Spring Security provides built-in CSRF protection, which prevents these attacks by requiring that requests (especially state-modifying requests like POST, PUT, DELETE) include a special CSRF token, typically as a hidden field or a header.

To disable CSRF protection (not recommended for public APIs):

http.csrf().disable();

7. How can you enable method-level security in Spring Boot?

Method-level security is enabled by annotating your Spring Boot application with @EnableGlobalMethodSecurity and specifying the level of security checks (e.g., pre/post annotations).

@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
}

After enabling it, you can use annotations like @PreAuthorize, @PostAuthorize, @Secured, and @RolesAllowed. 8. What is the difference between UserDetailsService and AuthenticationProvider?

    UserDetailsService: This is an interface that is responsible for retrieving user-related data. It has a method loadUserByUsername() that loads a user by their username and returns a UserDetails object containing user information like username, password, roles, etc.

    AuthenticationProvider: This interface is used for custom authentication logic. It handles the authentication of a user by validating credentials and returning an Authentication object if the user is successfully authenticated.

9. How can you implement multi-factor authentication (MFA) in Spring Boot?

To implement MFA:

    Use a service like Google Authenticator for the second factor.
    First, authenticate the user with their username and password.
    After successful authentication, generate a QR code or OTP (One-Time Password) for the second factor.
    Validate the OTP entered by the user.

You can integrate the second factor by using Spring Security's custom filters or a service like Authy or Twilio. 10. How can you secure REST APIs using API keys or tokens?

To secure REST APIs with API keys or tokens:

    API Key: Add the API key in the request header (e.g., Authorization: Api-Key your-api-key) and validate it in a custom filter.
    JWT: Secure the APIs using JWT tokens by adding a custom filter to authenticate each incoming request using the JWT token in the Authorization header.

Example API Key filter:

public class ApiKeyFilter extends OncePerRequestFilter {
private static final String API_KEY = "your-api-key";

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String apiKey = request.getHeader("Authorization");
        if (API_KEY.equals(apiKey)) {
            filterChain.doFilter(request, response);
        } else {
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Invalid API Key");
        }
    }

}

🔹 Spring Boot Performance & Best Practices

1. How can you improve Spring Boot application performance?

To improve the performance of a Spring Boot application:

    Use caching: Implement caching to reduce unnecessary database calls and speed up frequently accessed data. Use @Cacheable to cache methods and configure a caching provider (e.g., Ehcache, Redis).
    Database optimization: Optimize database queries, use pagination for large datasets, and minimize the number of queries executed per request.
    Asynchronous processing: Use @Async for long-running tasks to avoid blocking the main application thread.
    Proper connection pooling: Use a connection pool like HikariCP (Spring Boot's default) to manage database connections efficiently.
    Use a profiler: Profile your application with tools like Spring Boot Actuator, JProfiler, or VisualVM to identify bottlenecks.

2. What is Spring Boot's default logging framework, and how can you customize it?

Spring Boot uses Logback as the default logging framework. You can customize the logging behavior by configuring the application.properties or application.yml file, or by providing a custom logback-spring.xml configuration file.

Example:

logging.level.org.springframework.web=DEBUG
logging.level.com.yourcompany=TRACE
logging.file.name=logs/myapp.log

To customize further, you can define a logback-spring.xml file to configure log patterns, appenders, and log levels. 3. How do you enable asynchronous processing in Spring Boot?

To enable asynchronous processing in Spring Boot:

    Enable asynchronous processing by adding the @EnableAsync annotation to your configuration class.
    Use the @Async annotation on methods that should run asynchronously.

Example:

@Configuration
@EnableAsync
public class AsyncConfig {
}

@Service
public class MyService {
@Async
public void asyncMethod() {
// This method runs in a separate thread
}
}

Make sure that you have a thread pool configured (Spring Boot uses a default one, but you can customize it if needed). 4. What are the best practices for writing scalable and maintainable Spring Boot applications?

Best practices include:

    Separation of concerns: Keep controllers, services, and repositories separate. Use appropriate design patterns like the Singleton, Factory, and Strategy patterns.
    Use DTOs and mappers: Avoid exposing entity objects directly. Use DTOs (Data Transfer Objects) to transfer data and map them with libraries like MapStruct or ModelMapper.
    Exception handling: Implement global exception handling using @ControllerAdvice to manage exceptions centrally.
    Testing: Write unit tests for services and integration tests for API endpoints. Use @SpringBootTest for testing the application context.
    Configuration management: Store environment-specific configurations outside of the codebase (e.g., in application.properties, application.yml, or environment variables).
    Use profiles: Separate configurations using Spring profiles (e.g., dev, prod, test).
    Use logging: Enable logging for debugging and troubleshooting, and ensure that logs provide sufficient detail without being overly verbose.

5. How can you use distributed tracing and observability in Spring Boot?

To implement distributed tracing and observability:

    Spring Cloud Sleuth: Add the spring-cloud-starter-sleuth dependency to your project. It integrates with Spring Boot and adds tracing capabilities, including tracing of HTTP requests and other asynchronous operations.
    Micrometer: Use spring-boot-starter-actuator and configure Micrometer to export metrics to monitoring systems (e.g., Prometheus, Datadog, etc.).
    Zipkin: Configure Spring Cloud Sleuth with Zipkin to visualize and analyze traces.

Example:

spring.sleuth.sampler.probability=1.0
spring.zipkin.baseUrl=http://localhost:9411

6. What is the circuit breaker pattern, and how can you implement it using Spring Boot?

The circuit breaker pattern is used to prevent a system from repeatedly trying to execute a failing operation. When a failure threshold is exceeded, the circuit breaker "opens," preventing further attempts and allowing fallback mechanisms.

You can implement it using Spring Cloud Circuit Breaker and Resilience4J:

    Add the necessary dependency:

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>

    Use @CircuitBreaker annotation:

@CircuitBreaker(name = "myService", fallbackMethod = "fallbackMethod")
public String callExternalService() {
// Call external service
return "response";
}

public String fallbackMethod(Throwable throwable) {
return "fallback response";
}

7. How does Spring Boot handle rate limiting?

Spring Boot does not include built-in rate-limiting, but you can implement rate limiting with Spring Boot using libraries like Bucket4j or Resilience4J. These libraries help manage request rates and apply limits to incoming requests.

Example with Resilience4J:

resilience4j.ratelimiter:
instances:
apiRateLimiter:
limitForPeriod: 100
limitRefreshPeriod: 1s
timeoutDuration: 500ms

8. How can you configure a connection pool in Spring Boot?

Spring Boot uses HikariCP by default for database connection pooling. You can configure it in the application.properties or application.yml file.

Example configuration:

spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.minimum-idle=10
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.max-lifetime=600000

For a different connection pool, such as Apache DBCP2 or C3P0, you would add the relevant dependency and configure it similarly. 9. What are some common memory management issues in Spring Boot applications?

Common memory management issues in Spring Boot include:

    Memory leaks: Improper handling of resources like database connections, file streams, or external API connections can lead to memory leaks.
    Excessive heap usage: Applications with high heap usage might not have sufficient garbage collection or resource cleanup, leading to out-of-memory errors.
    Unclosed connections: Failing to close database or network connections can lead to memory exhaustion.

To avoid these issues:

    Use proper resource management practices like closing database connections and external resources.
    Enable GC logging and monitor memory usage using Spring Boot Actuator or JVisualVM.
    Tune the JVM heap size using -Xms and -Xmx flags based on the application's needs.

10. How can you debug slow queries in a Spring Boot application?

To debug slow queries:

    Enable Hibernate SQL logging: Log all SQL queries to identify performance bottlenecks.

spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.generate_statistics=true

Analyze slow queries: Use database tools like MySQL Slow Query Log or pg_stat_statements (for PostgreSQL) to identify slow or unoptimized queries.

Optimize queries: Use indexes, proper joins, and pagination to reduce query execution times.

Enable query caching: Use caching mechanisms to reduce the load on the database.
