---
title : 'Spring Boot'
weight: 3
description: "Concise Spring Boot interview cheatsheet for quick preparation"  # Short description
keywords: 
  - Java Spring Boot
  - Interview
  - Cheatsheet
  - Programming
tags: 
  - Java
---

# Spring Boot Comprehensive Guide

## 1. Core Fundamentals

### Spring Boot Overview
Spring Boot is a powerful framework for building production-ready applications in Java. It provides a rapid application development approach by offering:
- Opinionated configuration
- Embedded server support
- Production-ready features
- Simplified dependency management

### Dependency Injection
Dependency Injection (DI) is a design pattern that allows for loose coupling between classes by managing object creation and lifecycle.

#### Dependency Injection Annotations

1. **`@Autowired`**
   - Automatically injects dependencies
   - Can be used on constructors, methods, and fields
   - Default mechanism for dependency injection in Spring
   ```java
   @Autowired
   private UserService userService;
   ```

2. **`@Qualifier`**
   - Used when multiple beans of the same type exist
   - Helps Spring identify the specific bean to inject
   ```java
   @Autowired
   @Qualifier("specificUserService")
   private UserService userService;
   ```

3. **`@Primary`**
   - Indicates the primary implementation when multiple beans of the same type exist
   ```java
   @Component
   @Primary
   public class PrimaryUserService implements UserService {}
   ```

4. **`@Resource`**
   - Java standard annotation for dependency injection
   - Similar to `@Autowired`, but with some subtle differences
   ```java
   @Resource(name = "specificUserService")
   private UserService userService;
   ```

5. **`@Inject`**
   - Part of Java CDI (Contexts and Dependency Injection)
   - Standard Java alternative to `@Autowired`
   ```java
   @Inject
   private UserService userService;
   ```

6. **`@Bean`**
   - Used in configuration classes to manually create and configure beans
   ```java
   @Configuration
   public class AppConfig {
       @Bean
       public UserService userService() {
           return new UserServiceImpl();
       }
   }
   ```

7. **`@Lazy`**
   - Delays bean initialization until first use
   ```java
   @Component
   @Lazy
   public class HeavyService {}
   ```

8. **`@Scope`**
   - Defines the lifecycle and visibility of beans
   ```java
   @Component
   @Scope("prototype")
   public class PrototypeBean {}
   ```

### Inversion of Control (IoC)
IoC is a design principle where the control of object creation and lifecycle is managed by the framework, not the application code. Spring's IoC container manages bean creation, configuration, and lifecycle.

### Spring Boot Architecture
Spring Boot follows a layered architecture:
- Presentation Layer
- Service Layer
- Repository/Data Access Layer
- Configuration Layer

### Core Annotations

1. **`@SpringBootApplication`**
   - Combination of `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`
   - Entry point of Spring Boot application
   ```java
   @SpringBootApplication
   public class MyApplication {
       public static void main(String[] args) {
           SpringApplication.run(MyApplication.class, args);
       }
   }
   ```

2. **`@Configuration`**
   - Indicates a class as a source of bean definitions
   ```java
   @Configuration
   public class AppConfig {
       // Bean definitions
   }
   ```

3. **`@Component`**
   - Generic stereotype for any Spring-managed component
   ```java
   @Component
   public class GenericComponent {}
   ```

4. **`@Service`**
   - Specialization of `@Component` for service layer
   ```java
   @Service
   public class UserService {}
   ```

5. **`@Repository`**
   - Specialization for data access components
   ```java
   @Repository
   public class UserRepository {}
   ```

6. **`@Controller`**
   - Marks a class as a Spring MVC Controller
   ```java
   @Controller
   public class UserController {}
   ```

7. **`@RestController`**
   - Combines `@Controller` and `@ResponseBody`
   - Used for RESTful web services
   ```java
   @RestController
   public class UserRestController {}
   ```

## 2. Configuration

### Configuration Types
1. **`application.properties`**
   - Traditional key-value configuration
   ```properties
   server.port=8080
   spring.datasource.url=jdbc:mysql://localhost:3306/mydb
   ```

2. **`application.yml`**
   - YAML-based configuration with hierarchical structure
   ```yaml
   server:
     port: 8080
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/mydb
   ```

### Profiles
- Environment-specific configurations
- Activated using `spring.profiles.active`
```yaml
# application-dev.yml
spring:
  datasource:
    url: jdbc:h2:mem:devdb

# application-prod.yml
spring:
  datasource:
    url: jdbc:mysql://prodserver/proddb
```

### External Configuration
- Command-line arguments
- Java System properties
- OS environment variables
- Application property files

### Configuration Properties
**`@ConfigurationProperties`**
- Binds external configuration to a bean
```java
@Configuration
@ConfigurationProperties(prefix = "app")
public class AppConfig {
    private String name;
    private int version;
    // Getters and setters
}
```

## 3. Bean Lifecycle

### Bean Scopes
1. **Singleton** (default)
   - Single instance per Spring container
2. **Prototype**
   - New instance created for each request
3. **Request**
   - One instance per HTTP request
4. **Session**
   - One instance per HTTP session
5. **Application**
   - One instance per web application

### Lifecycle Callbacks

1. **`init-method`**
   ```java
   @Bean(initMethod = "init")
   public MyService myService() {
       return new MyService();
   }
   ```

2. **`destroy-method`**
   ```java
   @Bean(destroyMethod = "cleanup")
   public MyService myService() {
       return new MyService();
   }
   ```

3. **`@PostConstruct`**
   ```java
   @PostConstruct
   public void init() {
       // Initialization logic
   }
   ```

4. **`@PreDestroy`**
   ```java
   @PreDestroy
   public void cleanup() {
       // Cleanup logic
   }
   ```

## 4. Database Interaction

### Spring Data JPA Annotations

1. **`@Entity`**
   ```java
   @Entity
   public class User {
       // Class definition
   }
   ```

2. **`@Table`**
   ```java
   @Entity
   @Table(name = "users")
   public class User {}
   ```

3. **`@Column`**
   ```java
   @Column(name = "username", unique = true)
   private String username;
   ```

4. **`@Id`**
   ```java
   @Id
   private Long id;
   ```

5. **`@GeneratedValue`**
   ```java
   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   private Long id;
   ```

6. **`@OneToMany`**
   ```java
   @OneToMany(mappedBy = "user")
   private List<Order> orders;
   ```

7. **`@ManyToOne`**
   ```java
   @ManyToOne
   @JoinColumn(name = "user_id")
   private User user;
   ```

8. **`@ManyToMany`**
   ```java
   @ManyToMany
   @JoinTable(name = "user_role")
   private Set<Role> roles;
   ```

9. **`@Transient`**
   ```java
   @Transient
   private String calculatedField;
   ```

## 5. REST API Development

### Request Mapping Annotations

1. **`@RequestMapping`**
   ```java
   @RequestMapping("/users")
   public class UserController {}
   ```

2. **`@GetMapping`**
   ```java
   @GetMapping("/{id}")
   public User getUser(@PathVariable Long id) {}
   ```

3. **`@PostMapping`**
   ```java
   @PostMapping
   public User createUser(@RequestBody User user) {}
   ```

4. **`@PutMapping`**
   ```java
   @PutMapping("/{id}")
   public User updateUser(@PathVariable Long id, @RequestBody User user) {}
   ```

5. **`@DeleteMapping`**
   ```java
   @DeleteMapping("/{id}")
   public void deleteUser(@PathVariable Long id) {}
   ```

6. **`@PatchMapping`**
   ```java
   @PatchMapping("/{id}")
   public User partialUpdateUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {}
   ```

7. **`@RequestParam`**
   ```java
   @GetMapping
   public List<User> getUsers(@RequestParam(required = false) String status) {}
   ```

8. **`@PathVariable`**
   ```java
   @GetMapping("/{id}")
   public User getUserById(@PathVariable Long id) {}
   ```

9. **`@RequestBody`**
   ```java
   @PostMapping
   public User createUser(@RequestBody User user) {}
   ```

10. **`@RequestHeader`**
    ```java
    @GetMapping
    public void processRequest(@RequestHeader("X-Custom-Header") String customHeader) {}
    ```

---

# Spring Boot Advanced Guide

## 6. Dependency Management

### Maven vs Gradle
**Maven**
- XML-based configuration
- Conventional project structure
- Dependency management through `pom.xml`
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

**Gradle**
- Groovy/Kotlin DSL-based configuration
- More flexible build scripts
- Dependency management through `build.gradle`
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

### Spring Initializer
- Web-based tool for creating Spring Boot projects
- Generates project structure with selected dependencies
- Available at [start.spring.io](https://start.spring.io/)
- Supports Maven and Gradle
- Allows selection of:
  - Project metadata
  - Spring Boot version
  - Project dependencies
  - Project structure

### Dependency Resolution
- Automatic dependency management
- Transitive dependency handling
- Version compatibility checking
- Bill of Materials (BOM) support

### Bill of Materials (BOM)
- Centralized dependency version management
- Ensures compatibility between different library versions
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring.boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 7. Security

### Authentication Mechanisms
1. **Form-based Authentication**
2. **Basic Authentication**
3. **JWT (JSON Web Token)**
4. **OAuth2**
5. **LDAP Authentication**

### Authorization
- Role-based access control
- Permission-based access
- Method-level security

### JWT Token Authentication
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeRequests()
            .antMatchers("/public/**").permitAll()
            .antMatchers("/admin/**").hasRole("ADMIN")
            .anyRequest().authenticated()
            .and()
            .addFilterBefore(jwtTokenFilter, UsernamePasswordAuthenticationFilter.class);
    }
}
```

### OAuth2 Configuration
```java
@Configuration
@EnableResourceServer
public class OAuth2ResourceServer extends ResourceServerConfigurerAdapter {
    @Override
    public void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .antMatchers("/api/**").authenticated()
            .anyRequest().permitAll();
    }
}
```

### Security Annotations

1. **`@EnableWebSecurity`**
   - Enables Spring Security configuration
   ```java
   @Configuration
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter {}
   ```

2. **`@PreAuthorize`**
   - Method-level authorization before method execution
   ```java
   @PreAuthorize("hasRole('ADMIN')")
   public void adminMethod() {}
   ```

3. **`@PostAuthorize`**
   - Method-level authorization after method execution
   ```java
   @PostAuthorize("returnObject.owner == authentication.name")
   public User getUser(Long id) {}
   ```

4. **`@Secured`**
   - Role-based method security
   ```java
   @Secured("ROLE_ADMIN")
   public void deleteUser() {}
   ```

## 8. Microservices

### Service Discovery
- **Eureka**
  - Netflix OSS service discovery server
  - Client-side load balancing
```java
@EnableEurekaServer
public class ServiceDiscoveryServer {}

@EnableEurekaClient
public class MyMicroservice {}
```

### Load Balancing
- **Ribbon**
  - Client-side load balancing
  - Works with Eureka
```java
@RibbonClient(name = "user-service")
public interface UserServiceClient {
    @GetMapping("/users")
    List<User> getUsers();
}
```

### Circuit Breaker
- **Resilience4j**
  - Fault tolerance mechanism
  - Prevents cascade failures
```java
@CircuitBreaker(name = "userService", fallbackMethod = "fallbackMethod")
public User getUserById(Long id) {
    // Normal implementation
}

public User fallbackMethod(Long id, Exception e) {
    // Fallback logic
}
```

### Feign Client
- Declarative REST client
```java
@FeignClient(name = "user-service")
public interface UserClient {
    @GetMapping("/users/{id}")
    User getUserById(@PathVariable Long id);
}
```

## 9. Caching

### Caching Annotations

1. **`@Cacheable`**
   - Caches method return value
   ```java
   @Cacheable("users")
   public User getUserById(Long id) {
       return userRepository.findById(id);
   }
   ```

2. **`@CachePut`**
   - Always updates the cache
   ```java
   @CachePut(value = "users", key = "#user.id")
   public User updateUser(User user) {
       return userRepository.save(user);
   }
   ```

3. **`@CacheEvict`**
   - Removes entries from cache
   ```java
   @CacheEvict(value = "users", key = "#id")
   public void deleteUser(Long id) {
       userRepository.deleteById(id);
   }
   ```

4. **`@Caching`**
   - Complex caching operations
   ```java
   @Caching(
       evict = {
           @CacheEvict("users"),
           @CacheEvict(value = "userCount", allEntries = true)
       }
   )
   public void refreshUsers() {}
   ```

### Cache Providers
- **EhCache**
- **Redis**
- **Hazelcast**
- **Caffeine**

## 10. Logging

### Logging Configurations
```properties
# application.properties
logging.level.root=INFO
logging.level.org.springframework=WARN
logging.level.com.myapp=DEBUG
```

### Log Levels
- **TRACE**: Most detailed
- **DEBUG**: Debugging information
- **INFO**: General information
- **WARN**: Potential issues
- **ERROR**: Error conditions
- **FATAL**: Critical errors

### Logging Frameworks
1. **Logback**
   - Default Spring Boot logging framework
2. **Log4j2**
   - High-performance logging
3. **SLF4J**
   - Logging abstraction

## 11. Testing

### Testing Annotations

1. **`@SpringBootTest`**
   - Loads full application context
   ```java
   @SpringBootTest
   class UserServiceTest {
       @Autowired
       private UserService userService;
   }
   ```

2. **`@Test`**
   - Marks a method as test case
   ```java
   @Test
   void testUserCreation() {
       User user = userService.createUser(new User());
       assertNotNull(user);
   }
   ```

3. **`@MockBean`**
   - Creates mock of a Spring bean
   ```java
   @MockBean
   private UserRepository userRepository;
   ```

4. **`@SpyBean`**
   - Partial mocking of a Spring bean
   ```java
   @SpyBean
   private UserService userService;
   ```

5. **`@TestConfiguration`**
   - Additional configuration for tests
   ```java
   @TestConfiguration
   static class TestConfig {
       @Bean
       public TestDataService testDataService() {
           return new TestDataService();
       }
   }
   ```

### Testing Techniques
- Unit Testing
- Integration Testing
- Mockito for mocking
- JUnit 5
- Test coverage analysis

## 12. Performance

### Spring Boot Actuator
- Provides production-ready features
- Exposes operational information
```properties
management.endpoints.web.exposure.include=*
```

### Metrics
- Health checks
- Performance monitoring
- Resource utilization

### Monitoring Endpoints
- `/actuator/health`
- `/actuator/metrics`
- `/actuator/prometheus`

## 13. Advanced Topics

### AOP (Aspect-Oriented Programming)

#### Aspect Annotations

1. **`@Aspect`**
   ```java
   @Aspect
   @Component
   public class LoggingAspect {}
   ```

2. **`@Before`**
   ```java
   @Before("execution(* com.example.service.*.*(..))")
   public void logBefore(JoinPoint joinPoint) {
       // Pre-method logging
   }
   ```

3. **`@After`**
   ```java
   @After("execution(* com.example.service.*.*(..))")
   public void logAfter(JoinPoint joinPoint) {
       // Post-method logging
   }
   ```

4. **`@Around`**
   ```java
   @Around("execution(* com.example.service.*.*(..))")
   public Object aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
       // Before and after method execution
       return joinPoint.proceed();
   }
   ```

5. **`@AfterReturning`**
   ```java
   @AfterReturning(
       pointcut = "execution(* com.example.service.*.*(..))",
       returning = "result"
   )
   public void logAfterReturning(Object result) {
       // Log method return value
   }
   ```

6. **`@AfterThrowing`**
   ```java
   @AfterThrowing(
       pointcut = "execution(* com.example.service.*.*(..))",
       throwing = "error"
   )
   public void logAfterThrowing(Exception error) {
       // Log exceptions
   }
   ```

### Exception Handling

1. **`@ControllerAdvice`**
   ```java
   @ControllerAdvice
   public class GlobalExceptionHandler {
       // Centralized exception handling
   }
   ```

2. **`@ExceptionHandler`**
   ```java
   @ExceptionHandler(ResourceNotFoundException.class)
   public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
       // Custom error response
       return new ResponseEntity<>(errorResponse, HttpStatus.NOT_FOUND);
   }
   ```

3. **`@ResponseStatus`**
   ```java
   @ResponseStatus(HttpStatus.BAD_REQUEST)
   public class InvalidRequestException extends RuntimeException {
       // Custom exception with HTTP status
   }
   ```

### Additional Advanced Concepts
- Custom Validations
- Interceptors
- Filters
- Event Handling
- Listeners

---

# Spring Boot Advanced Topics

## 14. Messaging

### Spring Message Converters
- Responsible for converting between Java objects and message formats
- Supports various message formats (JSON, XML, etc.)

```java
@Configuration
public class MessageConverterConfig {
    @Bean
    public MappingJackson2HttpMessageConverter jsonConverter() {
        return new MappingJackson2HttpMessageConverter();
    }
}
```

### JMS (Java Message Service)
- Standard API for messaging between applications
- Supports point-to-point and publish-subscribe messaging

```java
@Configuration
public class JmsConfig {
    @Bean
    public JmsTemplate jmsTemplate(ConnectionFactory connectionFactory) {
        return new JmsTemplate(connectionFactory);
    }

    @JmsListener(destination = "myQueue")
    public void receiveMessage(String message) {
        // Process message
    }
}
```

### RabbitMQ Integration
- Advanced message queuing protocol (AMQP) support
- Lightweight and scalable messaging

```java
@Configuration
public class RabbitMQConfig {
    @Bean
    public Queue myQueue() {
        return new Queue("myQueue", true);
    }

    @RabbitListener(queues = "myQueue")
    public void processMessage(String message) {
        // Message processing logic
    }
}
```

### Apache Kafka Integration
- Distributed streaming platform
- High-throughput message processing

```java
@Configuration
public class KafkaConfig {
    @Bean
    public NewTopic myTopic() {
        return new NewTopic("myTopic", 1, (short) 1);
    }

    @KafkaListener(topics = "myTopic")
    public void listenToTopic(String message) {
        // Message processing
    }
}
```

### Messaging Patterns
1. **Point-to-Point Messaging**
   - One sender, one receiver
2. **Publish-Subscribe**
   - One sender, multiple receivers
3. **Request-Reply**
   - Synchronous messaging pattern

## 15. Reactive Programming

### Spring WebFlux
- Non-blocking web framework
- Built on Project Reactor
- Supports reactive programming paradigm

```java
@RestController
public class UserController {
    @GetMapping("/users")
    public Flux<User> getAllUsers() {
        return userService.findAll();
    }

    @GetMapping("/user/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);
    }
}
```

### Reactive Streams
- Standard for asynchronous stream processing
- Key interfaces:
  - Publisher
  - Subscriber
  - Subscription
  - Processor

```java
public class ReactiveExample {
    public Mono<String> processData(Mono<String> input) {
        return input
            .map(String::toUpperCase)
            .flatMap(this::processAsyncOperation);
    }
}
```

### Non-blocking I/O
- Eliminates thread blocking
- Improves application scalability
- Supports high concurrency

```java
@Service
public class ReactiveUserService {
    public Flux<User> findActiveUsers() {
        return Flux.fromIterable(userRepository.findAll())
            .filter(User::isActive)
            .transform(this::enhanceUserData);
    }
}
```

### Reactive Operators
- `map()`: Transform elements
- `flatMap()`: Transform and flatten
- `filter()`: Select elements
- `reduce()`: Aggregate elements
- `zip()`: Combine multiple streams

## 16. Externalization

### Internationalization (i18n)
- Support for multiple languages
- Locale-specific content

```java
@Configuration
public class LocaleConfig {
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver slr = new SessionLocaleResolver();
        slr.setDefaultLocale(Locale.US);
        return slr;
    }

    @Bean
    public LocaleChangeInterceptor localeChangeInterceptor() {
        LocaleChangeInterceptor lci = new LocaleChangeInterceptor();
        lci.setParamName("lang");
        return lci;
    }
}
```

### Localization (l10n)
- Adapting application to specific regions
- Formatting dates, numbers, currencies

```properties
# messages_en.properties
welcome.message=Welcome
greeting=Hello, {0}

# messages_fr.properties
welcome.message=Bienvenue
greeting=Bonjour, {0}
```

### Resource Bundles
- Manage language-specific resources
- Centralized translation management

```java
@Component
public class MessageService {
    @Autowired
    private MessageSource messageSource;

    public String getMessage(String code, Locale locale) {
        return messageSource.getMessage(code, null, locale);
    }
}
```

## 17. Scheduling & Async Processing

### Scheduling Tasks

1. **`@Scheduled` Annotation**
   - Simple task scheduling
   ```java
   @Component
   @EnableScheduling
   public class ScheduledTasks {
       @Scheduled(fixedRate = 5000)  // Every 5 seconds
       public void performTask() {
           // Task logic
       }

       @Scheduled(cron = "0 0 * * * *")  // Hourly
       public void hourlyTask() {
           // Hourly task logic
       }
   }
   ```

2. **`@EnableScheduling`**
   - Enables Spring's scheduling capabilities
   - Must be used on a configuration class

### Asynchronous Processing

1. **`@EnableAsync`**
   - Enables Spring's asynchronous method execution
   ```java
   @Configuration
   @EnableAsync
   public class AsyncConfig {
       @Bean
       public Executor taskExecutor() {
           ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
           executor.setCorePoolSize(2);
           executor.setMaxPoolSize(10);
           executor.setQueueCapacity(500);
           executor.initialize();
           return executor;
       }
   }
   ```

2. **`@Async` Annotation**
   - Marks method for asynchronous execution
   ```java
   @Service
   public class UserService {
       @Async
       public CompletableFuture<User> processUser(Long userId) {
           // Long-running task
           return CompletableFuture.completedFuture(processedUser);
       }
   }
   ```

### Async Processing Considerations
- Return types:
  - `void`
  - `Future`
  - `CompletableFuture`
- Exception handling
- Thread pool management

---

## 18. Validation

### Bean Validation

#### Validation Annotations

1. **`@Valid`**
   - Triggers validation of nested objects
   - Ensures method argument or return value meets validation constraints
   ```java
   @RestController
   public class UserController {
       @PostMapping("/users")
       public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
           userService.createUser(user);
           return ResponseEntity.ok(user);
       }
   }
   ```

2. **Common Bean Validation Annotations**

   a. **`@NotNull`**
      ```java
      @NotNull(message = "Username cannot be null")
      private String username;
      ```

   b. **`@Size`**
      ```java
      @Size(min = 3, max = 50, message = "Username must be between 3 and 50 characters")
      private String username;
      ```

   c. **`@Email`**
      ```java
      @Email(message = "Invalid email format")
      private String email;
      ```

   d. **`@Pattern`**
      ```java
      @Pattern(regexp = "^\\d{10}$", message = "Phone number must be 10 digits")
      private String phoneNumber;
      ```

   e. **`@Min` and `@Max`**
      ```java
      @Min(value = 18, message = "Age must be at least 18")
      @Max(value = 100, message = "Age must be no more than 100")
      private int age;
      ```

### Custom Validation

#### Creating Custom Validators

1. **Custom Constraint Annotation**
   ```java
   @Target({ElementType.FIELD})
   @Retention(RetentionPolicy.RUNTIME)
   @Constraint(validatedBy = UniqueUsernameValidator.class)
   public @interface UniqueUsername {
       String message() default "Username must be unique";
       Class<?>[] groups() default {};
       Class<? extends Payload>[] payload() default {};
   }
   ```

2. **Validator Implementation**
   ```java
   public class UniqueUsernameValidator implements ConstraintValidator<UniqueUsername, String> {
       @Autowired
       private UserRepository userRepository;

       @Override
       public boolean isValid(String username, ConstraintValidatorContext context) {
           return userRepository.findByUsername(username) == null;
       }
   }
   ```

### Validation Groups

- Support for different validation scenarios
- Allows partial object validation

```java
public interface CreateValidation {}
public interface UpdateValidation {}

public class User {
    @NotNull(groups = UpdateValidation.class)
    private Long id;

    @NotNull(groups = {CreateValidation.class, UpdateValidation.class})
    private String username;
}
```

### Global Exception Handling for Validation

```java
@ControllerAdvice
public class GlobalValidationExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Object> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error -> 
            errors.put(error.getField(), error.getDefaultMessage()));
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }
}
```

### Validation Best Practices

1. Use validation at the application's entry points
2. Validate input before processing
3. Provide clear, user-friendly validation messages
4. Use custom validators for complex validation logic
5. Implement validation at both client and server sides

### Performance Considerations

- Validation can add overhead to method execution
- Use selective validation
- Consider caching validation results for repeated objects
- Profile and optimize validation logic