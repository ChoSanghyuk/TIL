# Rest Api 인증에 따른 접근 권한 부여하기



 spring 프로젝트를 진행 중 spring security와 rest api를 동시에 사용하려 하니, get 이외의 request가 작동하지 않는다. 원인을 찾아보니 spring security에서 get 이외의 요청은 자동으로 막는다고 한다. 그럼 권한을 가진 사람에 한해서 api를 이용할 수 있게끔 설정을 해보록 하자.



## step 1. SecurityConfig 에 user 정보 및 암호화 방법 추가

​	우선 SecurityConfig를 설정해야 한다. 기본 설정은 `WebSecurityConfigurerAdapter`을 extend해서 `configure` 메소드를 @Override 해주기만 하면 된다. `configure` 메소드는 인증된 유저 정보를 받아와 `AuthenticationManagerBuilder`에 등록해 두는 역할을 한다. 

​	또한 추가로 PasswordEncoder를 작성해 @Bean으로 등록해 두어, 암호화 알고리즘에 따른 비밀번호를 반환할 수 있다.

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter{

	@Autowired
	private MyUserDetailsService myUserDetailsService;
	
	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		
		
		auth.userDetailsService(myUserDetailsService);
	}

	@Bean
	public PasswordEncoder passwordEncoder() {
		
		// 암호화할 방법을 정해서 암호화된 비밀번호를 반환
		return PasswordEncoderFactories.createDelegatingPasswordEncoder();
	}
	
}
```

​	user정보는 여기다가 직접적으로 설정 할 수도 있지만, 나는 새로운 class를 만들어서 작성하도록 하겠다. user정보는 `UserDetailsService` 객체를 사용하며, id, password, 권한으로 유저를 설정할 수 있다. 

```java
@Service
public class MyUserDetailsService implements UserDetailsService{

	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

		// 비밀먼호를 암호화할 알고리즘을 명시. noop은 암호화하지 x음
		return new User("jo" , "{noop}1234" , new ArrayList<>());
	}
	
	

}
```



## step2. jwt token 설정

​	security 인증으로 token을 사용한다. token에는 사용자 정보, 인증 받은 시간 등이 입력되어, 해당 사용자가 권한이 있는지, 혹은 인증 기간이 만료되진 않았는지 등을 설정한다. 

```java
@Service
public class JwtUtil {
	// 참고. secrete key는 복잡해야 보안 수준이 높아짐.
	private String SECRET_KEY = "secret";
	
	public String extractUsername(String token) {
		return extractClaim(token, Claims::getSubject);
	}
	
	public Date extractExpiration(String token) {
		return extractClaim(token, Claims::getExpiration);
	}
	
	public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
		final Claims claims = extractAllClaims(token);
		return claimsResolver.apply(claims);
	}
	
	private Claims extractAllClaims(String token) {
		return Jwts.parser().setSigningKey(SECRET_KEY).parseClaimsJws(token).getBody();
	}

	private Boolean isTokenExpired(String token) {
		return extractExpiration(token).before(new Date());
	}
	
	public String generateToken(UserDetails userDetails) {
		Map<String, Object> claims = new HashMap<>();
		return createToken(claims, userDetails.getUsername());
	}
	
	private String createToken(Map<String, Object> claims, String subject) {
		return Jwts.builder().setClaims(claims).setSubject(subject).setIssuedAt(new Date(System.currentTimeMillis()))
				.setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10))
				.signWith(SignatureAlgorithm.HS256, SECRET_KEY).compact();
	}
	
	public Boolean validateToken(String token, UserDetails userDetails) {
		final String username = extractUsername(token);
		return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));
	}
	
	
}

```

 위의 메소드들 중 가장 중요한 것은 `generateToken` 메소드이다. 코드를 천천히 살펴보면, 우선 유저정보를 받아 , Map 객체를 만들고, 유저 이름과 함께 `createToken`에 넘겨준다. 그러면 Jwts builder를 통해 유저 이름, 현재 시간, 인증 만료 시간을 설정하고, 암호화 알고리즘을 걸쳐 token을 생성한다. 이후 이렇게 생성된 token은 `validateToken` 을 통해 해당 token이 유효한지 검사를 받을 것이다. 



## step 3. 

step 1

A/authenticate Api endpoint

- Accepts user ID and password
- Returns JWT as response



사용자 요청 => header안에 JWT => 유효한지 검사 

