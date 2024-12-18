## Spring Security
- 스프링 기반 웹 애플리케이션의 인증과 인가 처리를 담당하는 프레임워크


- 주요 기능
  - 인증 및 권한 관리
    - 사용자 로그인과 특정 리소스에 대한 접근 권한을 검사한다.
    - 폼 로그인, OAuth2, JWT 등 지원
  - 보안 설정
    - 요청 URL과 메서드에 대한 접근 제어를 설정할 수 있다.
    - permitAll, hasRole
  - 필터 체인
    - 모든 요청을 필터 체인을 통과하며, 각 필터는 보안 작업을 수행한다.
    - UsernamePasswordAuthenticationFilter, CsrfFilter 등 ...
  

- 주요 필터
  - SecurityContextPersistenceFilter : 세션으로부터 인증 정보 처리
  - UsernamePasswordAuthenticationFilter : 폼로그인과 ID/PW 검증 및 인증 처리
  - AnonymousAuthenticationFilter : 미인증 사용자를 익명으로 설정하여 접근 가능하도록 처리
  - BearerTokenAuthenticationFilter : JWT 토큰에 대한 인증 처리
  - LogoutFilter : 로그아웃을 처리하고 SecurityContext와 세션을 정리


- 인증과 인가의 흐름
1. SecurityContextPersistenceFilter가 세션에서 SecurityContext(인증된 사용자 정보)를 가져온다.
2. 이때 인증된 정보가 없다면 새로운 SecurityContext를 생성, 있다면 기존 인증 정보를 SecurityContextHolder에 저장한다.
3. 인증 방식에 해당하는 필터가 SecurityContextHolder에서 Authentication 객체가 존재하는지 확인한다.
4. 폼 로그인인 경우 UsernamePasswordAuthenticationFilter, JWT인 경우 BearerTokenAuthenticationFilter가 인증 정보 유무를 확인한다.
```java
Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
// authentication.isAuthenticated() 인지를 확인
```
5. 만약 인증 정보가 없다면 AuthenticationManager에 인증 요청을 보낸다.
6. AuthenticationManager는 AuthenticationProvider를 통해 인증을 처리한다.
7. AuthenticationProvider는 UserDetailsService(사용자 조회 서비스)를 통해 실제 사용자를 DB에서 조회하며 인증을 처리한다.
```java
public interface AuthenticationManager {
    Authentication authenticate(Authentication authentication) throws AuthenticationException;
}
public interface AuthenticationProvider {
    Authentication authenticate(Authentication authentication) throws AuthenticationException;
    boolean supports(Class<?> authentication);
}
```
7. 인증 성공 시, 반환된 Authentication 객체를 SecurityContextHolder에 저장해 후속 필터가 사용하도록 한다.
8. 마지막 필터인 FilterSecurityInterceptor에서는 요청 리소스에 대한 접근 권한을 검사하여 인가를 처리한다.
9. 모든 필터를 통과하면 이제 애플리케이션 로직에 접근할 수 있다.


## 인증(Authentication)과 인가(Authorization)
- 인증
  - 사용자가 누구인지 신원을 확인하는 과정
  - 로그인과 비밀번호를 받아 사용자를 확인
  - JWT와 같은 토큰을 받아 사용자를 확인

- 스프링 시큐리티의 인증과정
  - UsernamePasswordAuthenticationFilter -> AuthenticationManager -> AuthenticationProvider 의 과정을 의미한다.
  - 반환된 Authentication 객체를 SecurityContextHolder에 저장
  - 사용자의 인증 정보와 권한을 담고 있어 이후 인가 필터에서 사용됨

- 인가
  - 인증된 사용자가 무엇을 할 수 있는지 확인하는 과정
  - 권한에 따라 특정 리소스에 대한 액세스를 결정한다.

- 스프링 시큐리티의 인가과정
  - FilterSecurityInterceptor -> AccessDecisionManager 의 과정을 의미한다.
  - Authentication에 담긴 권한을 바탕으로 리소스 접근 허용 여부를 결정한다.
  - 실패 시 403 Forbidden 또는 401 Unauthorized

- 인증 및 인가 세팅
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http
            .authorizeHttpRequests((requests) -> requests
                    .requestMatchers("/", "/home", "/signup", "/css/**").permitAll() // 인증이 필요하지 않음
                    .requestMatchers("/admin/**").hasRole("ADMIN") // 인증 + ADMIN 권한이 필요함
                    .anyRequest().authenticated() // 인증이 필요함
            );

    return http.build();
}
```