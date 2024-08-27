# Week2 스프링 시큐리티 강의정리

스프링 시큐리티 필터를 이용해 권한이 없는 사용자가 접근 시, 로그인 페이지로 리디렉션하는 설정을 추가한다음

CSRF 보호를 비활성화하고, HTTP 요청에 대한 권한 설정을 통해 사용자 접근을 제어한다.

또 설정된 권한에 따라 페이지 접근을 제한하고, 로그인 페이지로 리디렉션하는 방법을 배웠다.

```java
package com.cos.security1.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    @Bean
    public BCryptPasswordEncoder encodePwd() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf(AbstractHttpConfigurer::disable) // CSRF 보호를 비활성화
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/user/**").authenticated()
                        .requestMatchers("/admin/**").hasRole("ADMIN")
                        .anyRequest().permitAll()
                )
                .formLogin(login -> login
                        .loginPage("/login")
                        .loginProcessingUrl("/loginProc")
                        .defaultSuccessUrl("/")
                );
        return http.build();
    }
}
/*스프링6에 맞게 리펙터링*/
```

---

- 스프링부트 시큐리티를 활용하여 로그인 페이지를 제작하고, 로그인 폼을 구현한다.
- 회원가입 기능을 구현하기 위해 유저 테이블을 생성하고, 회원 정보를 데이터베이스에 저장하는 모델을 만든다.
- 회원가입 폼을 통해 사용자가 입력한 정보를 처리하고, 회원가입 절차를 진행한다.
- 비밀번호는 단순한 텍스트로 저장되지 않고, BCrypt 해싱을 통해 암호화하여 안전하게 저장한다.

```java
@Bean
    public BCryptPasswordEncoder encodePwd() {
        return new BCryptPasswordEncoder();
    }
```

- 회원가입이 완료되면 로그인 페이지로 리다이렉트되며, 다음 강의에서 로그인 기능 구현을 다룬다.

---

1. 스프링 시큐리티의 로그인 프로세스에서는 특정 URL로 요청이 오면 시큐리티가 자동으로 낚아채어 로그인 처리를 한다. 이를 통해 개발자는 별도의 로그인 컨트롤러를 구현할 필요가 없다.

2. 로그인 시 사용자의 세션은 시큐리티가 관리하며, 특정 객체 타입인 `Authentication`에 사용자 정보와 권한 등이 저장된다. 이를 통해 세션에 안전하게 사용자 정보를 저장할 수 있다.

3. 사용자 정보를 저장하기 위해 `UserDetails`와 `GrantedAuthority` 등을 구현한 클래스를 사용해야 하며, 이 클래스들은 시큐리티가 필요로 하는 객체 타입을 충족시켜야 한다.

4. 로그인 과정에서 `UserDetailsService`를 구현하여 사용자 정보를 로드하고, 해당 정보는 시큐리티의 세션에 저장되어 인증된 사용자로서의 역할을 수행하게 된다.

5. 구현한 내용을 테스트하기 위해 회원가입 후 로그인 절차를 수행하며, 발생하는 오류를 수정하고 필요한 설정을 조정하여 로그인 기능을 완성할 수 있다.

---

**시큐리티 설정과 권한 처리**: 강의에서는 Spring Security를 이용해 권한을 설정하고, 매니저와 어드민 계정을 생성하여 권한을 부여하고 로그인 테스트를 진행한다.

**롤 기반 접근 제어**: 롤(Role) 기반으로 접근을 제어하는 방법을 설명하며, 특정 메소드에 권한을 걸어 접근을 제한하는 방법

**어노테이션 활용**: `@EnableGlobalMethodSecurity`, `@PreAuthorize`, `@PostAuthorize` 등의 어노테이션을 사용해 메소드 단위로 권한을 설정하는 방법을 다룬다.

**JWT와 OAuth2 준비**: 이후 강의에서 JWT 토큰을 이용한 서버 구축과 OAuth2 클라이언트 설정을 통해 구글, 페이스북, 네이버 로그인 기능을 구현할 것

---

**OAuth 클라이언트 설정**: 구글 로그인 기능을 위해 필요한 OAuth 클라이언트 ID와 보안 비밀번호 설정 방법을 다룬다. 이 과정에서는 승인된 리디렉션 URI와 클라이언트 정보 관리가 중요하다.

**스프링 시큐리티와 OAuth 라이브러리 사용**: 강의에서는 스프링 시큐리티와 OAuth 라이브러리를 활용하여 구글 로그인 기능을 구현하는 방법을 설명한다. 이 라이브러리들이 어떻게 요청을 처리하고, 필요한 인증 토큰을 얻는지에 대해 다룬다.

**인증 과정과 서버와의 연동**: 구글 인증이 완료된 후, 서버 측에서 인증 정보를 처리하고 사용자 정보를 가져오는 과정에 대해 설명한다. 특히, 엑세스 토큰을 통해 사용자 정보를 접근하는 방법을 다룬다.

---

1. 구글 로그인이 완료되면, 서버가 엑세스 토큰을 통해 사용자 정보에 접근할 수 있는 권한을 얻는다.
2. 엑세스 토큰을 사용하여 구글 사용자 프로필 정보를 가져오고, 추가 회원가입이 필요할 경우 추가적인 정보를 요청한다.
3. 서버에서 받은 사용자 정보를 활용하여 자동으로 회원가입을 진행하거나, 필요에 따라 회원가입 절차를 추가로 진행할 수 있다.
4. 사용자의 ID, 이메일 등의 정보를 서버에 저장하고, 이를 통해 회원 관리를 한다.

---

## 마무리

저번 강의에서는 Oauth2.0을 배우며 토큰을 교환하고 시스템간의 소통하는 법을 배웠는데 실제로 구현하는법은 아직 배우지 않았었다. 이번 강의를 들으며 그게 좀더 구체화되는 느낌이라 좋았다.

하지만 4년전 강의라 그런가 버전이 맞지 않는게 많았고 후반으로 갈수록 실습은 못하고 원리만 눈으로 본거같다.

나중에 날잡고 리펙토링을 하고싶다.