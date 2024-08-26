# Spring Security

### Security Setting
@Configuration
@EnableWebSecurity      // 스프링 시큐리티 필터가 스프링 필터체인에 등록이 됨.
@EnableGlobalMethodSecurity(securedEnabled = true,  prePostEnabled = true)  // secure 어노테이션 횔성화.
public class SecurityConfig{

    // 해당 메서드의 리턴되는 오브젝트를 IOC로 등록헤줌
    @Bean
    public BCryptPasswordEncoder encoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
                .csrf(CsrfConfigurer::disable)
                .authorizeHttpRequests((authz) -> authz
                        .requestMatchers("/user/**").authenticated()
                        .requestMatchers("/manager/**").hasAnyRole("ADMIN, MANAGER")
                        .requestMatchers("/admin/**").hasRole("ADMIN")
                        .anyRequest().permitAll()
                )
                .formLogin(form -> form
                        .loginPage("/loginForm")
                        .loginProcessingUrl("/login")  // /login 주소가 호출이 되면 시큐리티가 낚아채서 대신 로그인을 진행.  controller에 login을 만들지 않아도 됨.
                        .defaultSuccessUrl("/")
                );

        return http.build();
    }
}

- 스프링 시큐리티를 사용허기 위해 @EnableWebSecurity 어노테이션을 붙여주면 스프링 시큐리티 필터가 스프링 필터체인에 등록이 된다.
- SecurityFilterChain에 권한 별 접근을 제어할 수 있다. 
    - authorizeHttpRequests에서 requestMatchers를 통해 지정 가능.


## Spring Security를 활용한 로그인
- 시큐리티가 기본적으로 제공하는 로그인 기능을 이용하려면 몇가지 설정이 필요.
    - security config에서 loginProcessingUrl("/login")을 작성
    .formLogin(form -> form
                        .loginPage("/loginForm")
                        .loginProcessingUrl("/login")  // /login 주소가 호출이 되면 시큐리티가 낚아채서 대신 로그인을 진행.  controller에 login을 만들지 않아도 됨.
                        .defaultSuccessUrl("/")
                );
- 위 코드와 같이 작성 시 시큐리티가 /login 요청을 낚아채서 로그인을 진행.

- 로그인 완료 시 시큐리티 세션을 만들어줌. (Security ContextHolder)
- 들어갈 수 있는 오브젝트가 정해져 있음 -> Authentication 타입 객체
- Authentication 안에 user정보가 있어야 함.
- User Object의 타입은 UserDetails 타입 객체 (정해져 있는)
- 즉 Security Session -> Authentication -> UserDetails

- 시큐리티 설정에서 loginProcessingUrl("/login") 지정
- /login요청이 오면 자동으로 UserDetailService 타입으로 IoC되어 있는 loadByUsername 함수가 실행
@Service
public class PrincipalDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    // 시큐리티 세션(Authentication 내부 (UserDetails)) 에 넣어줌.
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {

        User userEntity = userRepository.findByUsername(username);
        System.out.println(userEntity.getUsername());

        if(userEntity != null) {
            return new PrincipalDetails(userEntity);
        }

        return null;
    }
}
- 일치하는 사용자가 있을 경우 PrincipalDetails타입으로 사용자 정보를 저장한다.

## OAuth를 활용한 로그인
- 소셜 로그인을 활용하여 간편 로그인을 사용할 수 있다.
- 소셜 로그인을 사용하고자 하는 플랫폼에 서비스를 등록해야 한다.
- 구글을 통해 소셜 로그인을 구현 해보았다.
- 소셜 로그인 사용을 위해 OAuth2.0 의존성을 추가한다. 
    - implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
- 구글 API서비스에 서비스를 등록하면 Client Id와 Client Secret를 부여받는다. 

- 해당 정보를 .yml에 등록해준다. 
- scope를 지정하여 소셜에서 받아올 사용자 범위를 지정할 수 있다.
    security:
        oauth2:
            client:
                registration:
                    google:
                        client-id: (...)
                        client-secret: (...)
                        scope:
                            email
                            phone

- security Config에 Oauth2 Login 관련 설정을 지정해준다. 
    .oauth2Login((oauth) -> oauth
                        .loginPage("/loginForm")
                        .userInfoEndpoint(userInfoEndpoint -> userInfoEndpoint
                                .userService(principalOauth2UserService)
                        )
    );

- 구글 로그인이 완료되면 후처리가 필요하다.
1. 코드 받기(인증)    
2. 엑세스 토큰(권한)
3. 사용자 프로필 정보를 가져옴
4. 그 정보를 가지고 회원가입을 자동으로 진행시키기도 함.

- userInfoEndpoint -> userService(principalOauth2UserService)에서 후처리를 진행한다.

***
@Service
public class PrincipalOauth2UserService extends DefaultOAuth2UserService {


    // 구글로부터 받은 userReq 데이터에 대한 후처리되는 함수.
    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        System.out.println("getClientRegistration : " + userRequest.getClientRegistration());
        System.out.println("getAccessToken : " + userRequest.getAccessToken());
        System.out.println("getAttributes : " + super.loadUser(userRequest).getAttributes());

        return super.loadUser(userRequest);
    }
}
***

OAuth2UserRequest를 통해 사용자의 정보, 엑세스 토큰, 속성 정보들을 가져올 수 있다.