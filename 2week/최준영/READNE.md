# 스프링 시큐리티 특강 

본 강의의 스프링부트 버전이 2.x 버전인 점에서 현재 3.x 이상 버전과 코드가 다름

# Spring Security Config

```java

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        
        http.csrf().disable();
        http.authorizeRequests()
            .antMatchers("/user/**").authenticated()
			.antMatchers("/manager/**").access("hasRole('ROLE_ADMIN') and hasRole('ROLE_MANAGER')")
			.antMatchers("/admin/**").access("hasRole('ROLE_ADMIN')")
            .anyRequest().permitAll()
        .and()
            .formLogin()
            .loginPage("/login")
            .loginProcessingUrl("/loginProc")
            .defaultSuccessUrl("/");
    }

    @Bean
    public BCryptPasswordEncoder encodePwd(){
        return new BCryptPasswordEncoder();
    }
```
Security 설정 클래스이다.  
2.x 버전에전 WebSecurityConfigurerAdapter를 상속받아 configure 클래스를 Override하여 작성하였다.  
3.x 버전 Spring Security 6.0 이상부턴 WebSecurityConfigurerAdapter를 상속하지 않고 SpringFilterChain 메서드를 사용한다.


.hasRole() .hasAnyRole() : 엔드포인트별 권한설정  
.permitAll(): 모두 접근가능  
.authenticated() : 인증된 사용자만 접근가능  


.loginPage() : Security에서 기본 사용되는 로그인 페이지대신 자제 로그인 페이지 사용  
.loginProcessingUrl() : Security에서 기본 사용되는 로그인 엔드포인트대신 자체 엔드포인트 사용 ( 로그인시 이 경로로 POST 요청 )
.defaultSuccessUrl() : 로그인 성공시 이동할 경로  

@Bean : 해당 메서드 IoC에 저장  
BCryptPasswordEncoder : 패스워드 해시 알고리즘을 통해 암호화

# 로그인 

```java
@Data
public class PrincipalDetails implements UserDetails{

	private User user;

	public PrincipalDetails(User user) {
		super();
		this.user = user;
	}
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		Collection<GrantedAuthority> collet = new ArrayList<GrantedAuthority>();
		collet.add(()->{ return user.getRole();});
		return collet;
	}

    ...
}
```

PrincipalDetails 클래스는 UserDetails를 상속해 오버로딩하여 작성  
getAuthorities 메서드는 사용자 권한을 리턴해주는 메서드이다.

``` java
@Service
public class PrincipalDetailsService implements UserDetailsService{

	@Autowired
	private UserRepository userRepository;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		User user = userRepository.findByUsername(username);
		if(user == null) {
			return null;
		}
		return new PrincipalDetails(user);
	}

}
```

로그인시 자동으로 loadUserByUsername 메서드가 실행되어 리포지토리에 저장되어있는 유저를 PrincipalDetails 객체로 리턴

# OAuth2 - Google


```java
.oauth2Login()
    .loginPage("")
    .userInfoEndpoint()
        .userService(OAuth2UserService)

```
Google 로그인을 사용하기 위해선 OAuth2-client dependency를 추가해줘야함

## 소셜로그인 순서
1. 인증코드 발급
2. 엑세스 토큰 발급 
3. 사용자 정보 요청 응답
4. 사용자 정보를 토대로 회원가입 등 처리

처리를 위해 DefalultOAuth2UserService를 상속해 사용

```java
@Override
public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException{
    return super.loadUser(userRequest)
}
```

구글 소셜 로그인시 loadUser 메서드가 자동실행되어 OAuth2UserRequest에 응답 (엑세스 토큰과 사용자 정보)

이 정보를 토대로 회원가입 등 후처리를 진행하면 된다.


