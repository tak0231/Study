## Redis
- Springboot 2.0 기준으로 필요 설정 파일이 다름
- Springboot 2.0 이상 -> application.properties만 필요
- Springboot 2.0 미만 -> 설정 필요 : RedisConectionFactory / RedisTemplate /StringRedisTemplate
  - 2.0 이상부터는 AutoConfiguration이 이 설정들을 대신해줌


### application.properties
```
# server host
spring.redis.host=localhost
# server password
spring.redis.password= ㅇㄹ 
# connection port
spring.redis.port=6379

# pool settings ...
spring.redis.pool.max-idle=8
spring.redis.pool.min-idle=0
spring.redis.pool.max-active=8
spring.redis.pool.max-wait=-1
```

<spring boot 2.0 미만까지만 필요>
### RedisConfig.java
```
@Configuration
public class RedisConfig {


	@Bean    
	public RedisConnectionFactory redisConnectionFactory() {        
		LettuceConnectionFactory lettuceConnectionFactory = new LettuceConnectionFactory();        
		return lettuceConnectionFactory;    
	}     

	@Bean    
	public RedisTemplate<String, Object> redisTemplate() {        
		RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
		redisTemplate.setKeySerializer(new StringRedisSerializer());
		redisTemplate.setValueSerializer(new StringRedisSerializer());
		redisTemplate.setConnectionFactory(redisConnectionFactory());
		return redisTemplate;    
	}

	@Bean    
	public StringRedisTemplate stringRedisTemplate() {        
		StringRedisTemplate stringRedisTemplate = new StringRedisTemplate();        	
		stringRedisTemplate.setKeySerializer(new StringRedisSerializer());        
		stringRedisTemplate.setValueSerializer(new StringRedisSerializer());        	
		stringRedisTemplate.setConnectionFactory(redisConnectionFactory());        
		return stringRedisTemplate;    
	}
}

```

<다른 설정1>

```
@Configuration
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String host;

    @Value("${spring.redis.port}")
    private int port;

    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        return redisTemplate;
    }

    @Bean
    public StringRedisTemplate stringRedisTemplate() {
        final StringRedisTemplate stringRedisTemplate = new StringRedisTemplate();
        stringRedisTemplate.setKeySerializer(new StringRedisSerializer());
        stringRedisTemplate.setValueSerializer(new StringRedisSerializer());
        stringRedisTemplate.setConnectionFactory(redisConnectionFactory());
        return stringRedisTemplate;
    }

```
- redisConnectionFactory() 부분에서만 차이
- 작성방법이라기보다 host, port 전달하는 것에서의 차이
- 또, stringRedisTemplate()에서 stringRedstTemplate가 final로 되어있다는 정도 차이


<다른 설정2>

```
@Configuration
@EnableRedisRepositories
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String redisHost;

    @Value("${spring.redis.port}")
    private int redisPort;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(redisHost, redisPort);
    }

    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<byte[], byte[]> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        return redisTemplate;
    }
}
```

- host랑 port는 꼭 저렇게전달해야하나?
- RedisConnectionFacotry는 내장 or 외부 Redis에 연결시킬 수 있음
- RedisTemplate는 RedisConnectrion에서 넘겨준 byte 값을 객체 직렬화 함


## 직렬화 Serialization
- 객체를 파일이나 네트워크로 정상적으로 전달하기 위해 형태를 재구성하는 것
- 즉, 객체는 바이트형이 아니므로 파일에 저장할 수도 없고, 네트워크에 전송도 안됨
- 그래서 객체를 바이트배열로 변환해서, 데이터스트림으로 만드는 것을 말함


- 직렬화 방식이 다름
- 버전에 따라 사용 주의해야 함


<다른 방식3>
```
@RequiredArgsConstructor
@Configuration
@EnableRedisRepositories
public class RedisRepositoryConfig {

    private final RedisProperties redisProperties;

    // lettuce
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(redisProperties.getHost(), redisProperties.getPort());
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```
### EnableRedisRepositories
- Redis Repository를 사용하겠다는

[Redis 사용방식]
- Spring Data Redis -> Redis 접근방식 크게 2가지
  1) RedisTemplate를 이용한 방식  => Service 단에서의 RedisTemplate를 상성자 주입 방식으로 사용하는 방법
  2) RedisRepository를 이용한 방식  => 객체 저장 방식(VO, DTO 마냥)


- 두 방식 모두 Redis에 접근하기 위해 Redis 저장소와 연결하는 과정이 필요

- Spring boot 2 부터는 Lettuce(레터스)가 기본
- 자동설정에 의해 Redis Connection factory로 Letttus가 사용됨

<br/>

### 참고
- (설정) : https://oingdaddy.tistory.com/310
- https://oingdaddy.tistory.com/310
- https://velog.io/@kenux/Redis-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-Redis-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0
- http://tech.pick-git.com/embedded-redis-server/
- https://wildeveloperetrain.tistory.com/32
- https://wildeveloperetrain.tistory.com/32
