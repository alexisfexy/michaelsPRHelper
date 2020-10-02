# Phase 1: Pre Code Review Guidelines

### Overview: 

Below are some easy, standardized steps you should follow while coding.  Please follow + complete all of the following, <u>**prior**</u> to submitting your code for review.

*• For pull review questions, contact: darren24@michaels.com or kevin294@michaels.com.* 

*• For questions or errors pertaining to this document, contact: alexi474@michaels.com*.

----
### 1. SOLID Principles

All of the backend should follow *SOLID principles*.

Below is a link to a quick explanation of what this entails. The examples are in Ruby, but the priniciples are the same.

[https://www.monterail.com/hubfs/PDF%20content/SOLID_cheatsheet.pdf](https://www.monterail.com/hubfs/PDF content/SOLID_cheatsheet.pdf)



### 2. Logger

If you have a method that has logic in it, please surround it using Logger. This will make it much easier to debug in the future.

Below is an example: 

```java
@Service
public class UserService {
  /// DECLARE LOGGER
  private static final Logger logger = LoggerFactory.getLogger(UserController.class);

  private final UserDao userDao;

  @Autowired
  public UserService(final UserDao userDao) {
    this.userDao = userDao;
  }

  public User createUser(final User user) {
    logger.debug("UserService.createUser start"); // SURROUND YOUR CODE WITH LOGGER
    final User createdUser = userDao.createUser(user);
    logger.debug("UserService.createUser end"); // SURROUND YOUR CODE WITH LOGGER
    return createdUser;
  }
}

```



### 3. Lombok

Please do <u>not</u> write your own "getters" and "setters". Simply use Lombok annotate your classes with `@Data`. `@Data` is a convenient shortcut annotation that bundles the features of [`@ToString`](https://projectlombok.org/features/ToString), [`@EqualsAndHashCode`](https://projectlombok.org/features/EqualsAndHashCode), [`@Getter` / `@Setter`](https://projectlombok.org/features/GetterSetter) and [`@RequiredArgsConstructor`](https://projectlombok.org/features/constructor) together.

If Lombok is not working for you, confirm the following: 

1. You have the Lombok dependency in your pom.xml file. If you are pulling from the master branch, this should already be included.

   ```xml
   <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <scope>provided</scope>
           </dependency>
   ```

2. You have the Lombok plugin installed in Intellj & are enabling annotation processing:

   1. If you do not, follow the steps: https://www.baeldung.com/lombok-ide



### 4. Using Final

For those of us not familiar with Java, `final` does <u>not</u> mean the same things as `const` in other programming languages. Rather, `final` is a way to ensure the reference is not changed. 

To ensure our variables are not being overriden, we want to use `final` as much as possible! When in doubt, try to make a variable `final`. 

If you recieve an error, then you know you are unable to do so. Using final really matters whenever passing to methods - so <u>use it on method params!</u> 





### 5. JUnit

Ideally, write your JUnit tests <u>prior</u> to writing any code. In the case of Phase 1, you may write them after your code is done. It would be preferred to have these written <u>**prior**</u> to submitting your code for a code review. If you have already submitted your code for review for phase one & you have not already wrote your unit tests -  these will be required by Labor Day. 

- Each unit test should be testing for <u>one</u> thing. It is better to write 60 unit tests checking specifics rather than one unit test, checking everything. For example, you should be writing unit tests for all of your: 
  - If statements
  - Exceptions

There are plenty of examples of JUnit tests online. We are using JUnit 5. One good example is: https://www.youtube.com/watch?v=GoyAeom2f2k&feature=emb_title/. See the <u>Users repo, develop branch</u> in BitBucket for more info.



### 6. Controllers & Response Entities 

Please do **<u>not</u>** return response entities in your controllers. For example, do <u>not</u> have a return type of `ResponseEntity<?>` . It is very ambiguous!

Instead, your return type should be whatever you expect your controller to return on success. When you encouter any failures, throw exceptions, and add these to the exception handler. 

You will see a *shared* package in the users repo (develop branch) that will give you great example on how we want to handle exceptions. For example:

```java
/// InactiveException.java
@ResponseStatus(HttpStatus.FORBIDDEN)
public class InactiveException extends RuntimeException {
  public InactiveException(String message) {
    super(message);
  }
}
```

```java
//GlobalExceptionHandler.java
	@ExceptionHandler(InactiveException.class)
  @ResponseStatus(HttpStatus.FORBIDDEN)
  public ResponseEntity<Object> handleInactiveUserException(final InactiveException ex) {
    ErrorResponse errorResponse = new ErrorResponse(HttpStatus.FORBIDDEN, ex);
    return new ResponseEntity<>(errorResponse, new HttpHeaders(), errorResponse.getStatus());
  }
```



### 7. Swagger

Please register <u>ALL</u> your controllers to Swagger so they have their own "drop downs".

For more information, please look at Swagger.java in the the Users repo (develop branch). It is in the the foundationalservices/configuration package. 

Follow the existing Swagger documentation and make sure every endpoint and controller is annotated with Swagger annotations. For example, `@Tag` on the controller & other annotations on the endpoint - examples can be found in the Users repo.



### *Need More Information?* 

If you are still stuck on one of the prior concepts, look at the **Users repo, develop branch** on BitBucket for examples.
