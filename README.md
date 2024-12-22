# SpringDataJPA
A web-application built in Spring boot using JPA and Hibernate concepts, in order to perform CRUD operations.

**Configuration file code:
	#Spring boot mysql configuration
	spring.datasource.url=jdbc:mysql://localhost:3306/shakti
	spring.datasource.username=root
	spring.datasource.password=root
	spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

	#JPA/Hibernate configuration: 
	spring.jpa.hibernate.ddl-auto=update
	
	#Hibernate Dialect: to mention DB to whom our application will interact. Below code is optional because SB Data JPA will automatically detect the DB based on connecter jar file of DB
	spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQLDialect    //MySQL8Dialect is deprecated

	# For learning purpose only (Optional) : To see the operations internally performed by JPA
	spring.jpa.show-sql=true
	
	#Prod: Connection is alive or not  (optional)
	# spring.datasource.hikari.connection-test-query=SELECT 1
	# spring.datasource.hikari.validation-timeout=5000 
	
**Dependencies (POM.xml)
	spring-boot-starter-data-jpa
	mysql-connector-j

***CODE***
PART-I :-
//@Entity annotation will manage schema of table
//@Table annotation will inform to JPA, what will be our table name if not present. If it is already there then make sure the table name should be same
	@Entity
	@Table(name="JOB")

//	@Id will denote id column as PRIMARY KEY. 
//	@Column will give the name to the column. 
//	@GeneratedValue will make the column as auto-increment
	@Id
	@Column(name="TASK_ID", length = 50)
	

//creating repository to perform operations with DB
Create UserRepository interface extends JpaRepository<{Entity-class-Name},{Wrapper-class-of-primary-key-field}> interface.
Create UserService class ->@Service and inject UserRepository Object using @Autowired. We will call save method using it.
	create a method with Entity class as RT and parameter object means "User" class.

Calling of service method :
	- from main class: extends CommandLineRunner and override run() method. Inject UserService class. or
	- from HomeController:  @PostMapping("/todojpa/save") public User method(@RequestBody User userObject){} 
Implementation of Service class method
	 by using "userRepository" object, we can call save method by passing Entity class object as a parameter

PART-II :-
Getting data:
	Optional<UserEntity> optionalUser = userRepository.findById(id);
		We can check later by exception call whether data is present for request id or not
	List<UserEntity> allUsers = userRepository.findAll();
		Getting all data

PART-III :-
Updating id specific data:
	UserEntity existingUserEntity=userRepository.findById(id).orElseThrow(()->new RuntimeException("Id specific record not  found"));
	
Part-IV :-
Deleting data:
	UserEntity user=userRepository.findById(id).orElseThrow(()->new RuntimeException("No record found to delete for this id"));
	OR 
	userRepository.deleteById(id);
 
	
