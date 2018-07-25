http://javasampleapproach.com/spring-framework/spring-data/spring-data-redis-example-spring-boot-redis-example

I. Technology
II. Spring Data Redis
1. Maven Dependency
2. Redis Configuration
3. Redis CRUD Operations with RedisTemplate
III. Practice
1. Project Structure
2. Step by Step
1. Create Spring Boot project & add Dependencies
2. Configure Spring Data Redis
3. Create DataModel Class
4. Create Repository
5. Create Web Controller
6. Run Spring Boot Application & Check Result
IV. Source Code


I. Technology
– Java 1.8
– Maven 3.3.9
– Spring Tool Suite – Version 3.9.0.RELEASE
– Spring Boot: 1.5.9.RELEASE

II. Spring Data Redis
1. Maven Dependency
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

2. Redis Configuration

We use Jedis as a Redis client to define and establish the connection between our Spring Boot application and the Redis server instance.



@Bean
JedisConnectionFactory jedisConnectionFactory() {
	return new JedisConnectionFactory();
}

@Bean
public RedisTemplate<String, Object> redisTemplate() {
	final RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
	template.setConnectionFactory(jedisConnectionFactory());
	template.setValueSerializer(new GenericToStringSerializer<Object>(Object.class));
	return template;
}
1
2
3
4
5
6
7
8
9
10
11
12
@Bean
JedisConnectionFactory jedisConnectionFactory() {
	return new JedisConnectionFactory();
}
 
@Bean
public RedisTemplate<String, Object> redisTemplate() {
	final RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
	template.setConnectionFactory(jedisConnectionFactory());
	template.setValueSerializer(new GenericToStringSerializer<Object>(Object.class));
	return template;
}

3. Redis CRUD Operations with RedisTemplate
RedisTemplate provides DefaultHashOperations instance that can do hash-related operations for data manipulation.
To get DefaultHashOperations instance, we call RedisTemplate.opsForHash():

RedisTemplate<String, Object> redisTemplate;
// HashOperations<Key, HashKey, HaskValue>
HashOperations<String, Long, Customer> hashOperations;
 
//...
hashOperations = redisTemplate.opsForHash();
 
// save(Customer customer)
	hashOperations.put(KEY, customer.getId(), customer);
 
// Customer find(Long id)
	return hashOperations.get(KEY, id);
 
// Map<Long, Customer> findAll()
	return hashOperations.entries(KEY);
 
// update(Customer customer)
	hashOperations.put(KEY, customer.getId(), customer);
 
// void delete(Long id)
	hashOperations.delete(KEY, id);
	
III. Practice
1. Project Structure

– Customer is a Data Model Class.
– CustomerRepository is an interface which will be autowired in WebController for implementing repository methods and custom finder methods.
– WebController is a REST Controller which has request mapping methods for RESTful requests such as: save, findall, findbyid, update, delete.
– RedisConfig defines and establishes the connection between our Spring Boot application and the Redis server instance.
– Dependencies for Spring Boot and Spring Data Redis in pom.xml

2. Step by Step

1. Create Spring Boot project & add Dependencies

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
 
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>

2. Configure Spring Data Redis
3. Create DataModel Class
4. Create Repository
5. Create Web Controller
6. Run Spring Boot Application & Check Result
– Config maven build:
clean install
– Run project with mode Spring Boot App
– Check results:

Request 1
http://localhost:8080/save
The browser returns Done.

Request 2
http://localhost:8080/findall

Request 3
http://localhost:8080/find?id=2

Request 4.1
http://localhost:8080/uppercase?id=2
The browser returns Done.

Request 4.2: check update
http://localhost:8080/find?id=2

Request 5.1
http://localhost:8080/delete?id=3
The browser returns Done.

Request 5.2: check delete
http://localhost:8080/findall


