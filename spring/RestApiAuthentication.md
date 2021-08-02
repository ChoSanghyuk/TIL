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



## step 3. 인증요청 클래스와 인증응답 클래스 생성

​	인증 요청 클래스는 사용자 id와 password를 속성으로 가지며, 추후 인증 메서드에 입력 형태로 사용됩니다.

```java
package com.shHair.reservation.models;

// input structure
public class AuthenticationRequest {

	private String username;
	private String password;

	public AuthenticationRequest() {
	}
	
	public AuthenticationRequest(String username, String password) {
		this.username = username;
		this.password = password;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	
	
}

```

​	인증 응답 클래스는 jwt 토큰만을 속성으로 가지고, 추후 인증 메서드에서 return 값이 됩니다.

```java
package com.shHair.reservation.models;

// output structure
public class AuthenticationResponse {

	private final String jwt;

	public AuthenticationResponse(String jwt) {
		this.jwt = jwt;
	}

	public String getJwt() {
		return jwt;
	}	
}
```





## step 4. "/authenticate"에 인증 토큰 생성 메소드 매핑

"/authenticate" Api endpoint 을 생성하고, 해당 url에서 유저 정보를 받고 JWT 토큰을 반환하는 메소드를 만들어 줍니다. 이를 통해 사용자가 유효한지 검사 합니다.

```java
@RestController
public class DemoController {
	
	@Autowired
	private AuthenticationManager authenticationManager;
	
	@Autowired
	private MyUserDetailsService userDetailsService;
	
	@Autowired
	private JwtUtil jwtTokenUtil;
	
	@RequestMapping(value = "/authenticate", method = RequestMethod.POST)
	public ResponseEntity<?> createAuthenticationToken(
        			@RequestBody AuthenticationRequest authenticationRequest) throws Exception{
		
		try {
			authenticationManager.authenticate(
					new UsernamePasswordAuthenticationToken(
                        authenticationRequest.getUsername(), authenticationRequest.getPassword())
				);
		} catch(BadCredentialsException e) {
			throw new Exception("Incorrect username or password", e);
		}
		
		final UserDetails userDetails = userDetailsService.loadUserByUsername(
            													authenticationRequest.getUsername());
		
		final String jwt = jwtTokenUtil.generateToken(userDetails);
		
		return ResponseEntity.ok(new AuthenticationResponse(jwt));
	}
}

```

1. 방금 생성한 인증요청 클래스의 객체가 파라미터로 들어옵니다.
2. authenticationManager 객체를 통해서, 해당 요청이 인증된 사용자인지 검증합니다. 만약 인증된 사용자가 아니라면 `BadCredentialsException` 이 발생됩니다.
3. 인증이 통과된다면, `userDetailsService` 객체의 `.loadUserByUsername` 메소드에 username을 넣어, 유저 정보를 가져옵니다.
4. 해당 유저 정보로 jwt 토큰을 생성합니다.
5. 생성된 토큰을 인증응답 클래스 객체에 담아 return 합니다. 



​	또한 authenticationManager가 Autowired될 수 있게, SecurityConfig 파일에 해당 Bean을 추가해 줍니다. 

```java
	@Override
	@Bean
	public AuthenticationManager authenticationManagerBean() throws Exception {
		return super.authenticationManagerBean();
	}
```

- 여기서 super은 SecurityConfig가 extends하고 있는 `WebSecurityConfigurerAdapter` 클래스를 의미합니다. 



## step 5. 해당 인증 endpoint에 사용자가 접근할 수 있게 설정

​	인증에 "/authenticate" endpoint에서 실행되는데, 기타 설정 없이는 secuirty에 막혀 해당 endpoint까지 접근이 막혀있는 상태입니다. 사용자가 인증을 요청할 수 있게, 해당 endpoint로의 접근을 열어 줍니다. 

​	SecurityConfig 파일에서 설정을 해줍니다.

``` java
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http.csrf().disable()
				.authorizeRequests().antMatchers("/authenticate").permitAll()
				.anyRequest().authenticated();
				
	}
```

- "/authenticate" endpoint로의 접근은 모두 허용해 줍니다.
- 다른 요청들은 인증을 실시합니다. 



## step 6. 들어오는 모든 요청 검사하기

​	이제 들어오는 모든 요청마다 header에서 JWT 토큰을 꺼내, 유효한지 확인하고, 해당 토큰을 등록해야 합니다. 이 작업을 위해서는 filter가 필요합니다.

```java
@Component
public class JwtRequestFilter extends OncePerRequestFilter{
	
	@Autowired
	private MyUserDetailsService userDetailsService;
	
	@Autowired
	private JwtUtil jwtUtil;

	@Override
	protected void doFilterInternal(
        			HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {
		
		final String authorizationHeader = request.getHeader("Authorization");
		
		String username = null;
		String jwt = null;
		
		if(authorizationHeader != null && authorizationHeader.startsWith("Bearer ")) {
			jwt = authorizationHeader.substring(7);
			username = jwtUtil.extractUsername(jwt);
		}
		
		if(username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
			UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);
			if(jwtUtil.validateToken(jwt, userDetails)) {
				
				UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken 
                    									= new UsernamePasswordAuthenticationToken(
						userDetails, null, userDetails.getAuthorities());
				
				usernamePasswordAuthenticationToken.setDetails(
						new WebAuthenticationDetailsSource().buildDetails(request));
				
				SecurityContextHolder.getContext().setAuthentication(usernamePasswordAuthenticationToken);
						
						
			
			}
			
		}
		filterChain.doFilter(request, response);
	}

}
```

1. `OncePerRequestFilter` 클래스를 상속받는 클래스를 생성해 줍니다.
2. 토큰 인증에 UserDetailsService와 JwtUtil 객체가 필요함으로 필드로 선언하고, autowired 해줍니다.
3. doFilterInternal() 메소드를 override하고, 작성해 줍니다.
   1. 요청 Header에서 "Authorization" 부분을 가져옵니다.
   2. 해당 부분이 "Bearer" 로 시작하면 그 이후 부분을 저장합니다. (토큰 앞에 Bearer을 붙여서 요청보내기 때문) 또한 해당 토큰을 가지고 유저 이름을 받아옵니다.
   3. SecurityContextHolder 에 유효한 인증 정보들을 저장시킵니다. (Spring Security의 Default 과정)
   4. filterChain.doFileter() 을 통해 계속해서 filter을 수행합니다.



## step 7. 필터 적용시키게 하기

​	요청이 들어올 때 위의 필터를 적용시키게 설정합니다. 앞서 endpoint 설정했던 SecurityConfig 클래스의 configure 메소드에 설정을 추가해 줍니다.

```java
@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		http.csrf().disable()
				.authorizeRequests().antMatchers("/authenticate").permitAll()
				.anyRequest().authenticated()
				.and().sessionManagement()
				.sessionCreationPolicy(SessionCreationPolicy.STATELESS);
		
		http.addFilterBefore(jwtRequestFiler, UsernamePasswordAuthenticationFilter.class); // A가 B보다 먼저 호출
		
```

- Security가 별도의 Session을 생성하지 않도록 설정합니다.
- 위에서 생성한 jwtRequestFiler 가 Security의 UsernamePasswordAuthenticationFilter 보다 먼저 동작하도록 설정합니다.



## 요청보내기

1. 설정한 endpoint에 json 형태로 "username" 과 "password" 을 보내고 토큰을 받아옵니다.
2. 이후 다른 endpoint에 접근 및 요청하고 싶을 시, header에 "Authorization" Key에 `Bareaer + 토큰`을 value로 넣어 보내줍니다.
