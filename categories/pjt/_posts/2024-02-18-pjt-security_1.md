---
layout: post
title:  "[Spring Security] Security 기본 설정"
description: >
    "Spring Security 기본 구조 이해"

hide_last_modified: true
---
* TOC
{:toc}
## Setting
Spring Boot 3.2.1   
Spring Security 6.2.1   
jjwt 0.12.4

# Spring Security
## 왜 사용해야 하는가?
- Spring Security는 자체적으로 인증/인가 로직을 도와주고 외부 공격을 방어해주는 기능을 수행한다.

> 현재 Spring Security는 7버전을 준비하면서 Lambda DSL을 사용하는 것으로 바뀌었다.   
Deprecated 되어있는 Method들이 상당히 많기 때문에 찾아보고 사용하는 것을 권장한다.

## Security Architecture
### 어떻게 동작하는가?
- Spring Security는 Servlet 내 FilterChain에서 들어오는 요청을 처리할 때 동작한다.
- 그러면 어떻게 Security는 Servlet Container에서 Spring Container에 접근해서 요청을 처리할 수 있을까?

![filter chain](</assets/img/pjt/filter_chain.png>)

- DelegatingFilterProxy에서는 예외적으로 Spring container에 Servlet Container가 접근할 수 있도록 도와준다. 
- 해당 DelegatingFilterProxy에 FilterChainProxy는 Spring Container 내의 SecurityFilterChain 이름으로 등록된 Bean을 찾아서 사용할 수 있게 해주는 것이다. 

## Security Filter Chain
### Filter Chain 내부 모습
```terminal
2023-06-14T08:55:22.321-03:00  INFO 76975 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will secure any request with [
org.springframework.security.web.session.DisableEncodeUrlFilter@404db674,
org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@50f097b5,
org.springframework.security.web.context.SecurityContextHolderFilter@6fc6deb7,
org.springframework.security.web.header.HeaderWriterFilter@6f76c2cc,
org.springframework.security.web.csrf.CsrfFilter@c29fe36,
org.springframework.security.web.authentication.logout.LogoutFilter@ef60710,
org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter@7c2dfa2,
org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter@4397a639,
org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter@7add838c,
org.springframework.security.web.authentication.www.BasicAuthenticationFilter@5cc9d3d0,
org.springframework.security.web.savedrequest.RequestCacheAwareFilter@7da39774,
org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@32b0876c,
org.springframework.security.web.authentication.AnonymousAuthenticationFilter@3662bdff,
org.springframework.security.web.access.ExceptionTranslationFilter@77681ce4,
org.springframework.security.web.access.intercept.AuthorizationFilter@169268a7]
```

- Security Filter Chain의 내부 구조를 Info level에서 확인했을 때의 구조이다. 
- 순서를 알고있을 필요는 없다고 얘기하지만 Custom Filter를 사용하기 위해서는 어느정도 알아두는 것을 추천한다.

### Security Config
```java
    @Configuration
    @EnableWebSecurity
    @RequiredArgsConstructor
    public class SecurityConfig {
        private final JwtAuthenticationFilter jwtAuthenticationFilter;

        @Bean
        public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
            http
                    .cors(cors -> cors.configurationSource(configurationSource()))
                    .csrf(AbstractHttpConfigurer::disable)
                    .authorizeHttpRequests(authorize -> authorize
                            .dispatcherTypeMatchers(FORWARD, ERROR).permitAll()
                            .requestMatchers("/member/login", "/member/signup", "/member/idcheck/**",
                                    "/article/list/**", "/article/detail/**", "comment/list/**", "/article/size",
                                    "/file/detail/**",
                                    ("/auth/silent-refresh"))
                            .permitAll()
                            .anyRequest().authenticated()
                    )
                    .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

            return http.build();
        }

        @Bean
        public CorsConfigurationSource configurationSource() {
            CorsConfiguration config = new CorsConfiguration();
            config.setAllowedOrigins(Arrays.asList("https://i10a608.p.ssafy.io"));
            config.setAllowedMethods(Arrays.asList("GET", "PUT", "POST", "DELETE", "OPTIONS", "PATCH"));
            config.setAllowCredentials(true);
            config.setAllowedHeaders(Arrays.asList("Authorization", "content-type", "x-requested-with"));
            config.setMaxAge(1800L);

            UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
            source.registerCorsConfiguration("/**", config);

            return source;
        }

        @Bean
        public PasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }
    }
```

- 해당 코드는 진행했던 프로젝트의 Security Config 설정이다.
- SecurityFilterChain 부분 코드를 보면 알 수 있듯이 Cors(Cross Origin Resource Sharing) 설정과 CSRF(Cross Site Request Forgery) 설정을 진행할 수 있다. 

- 마지막으로 짧게 프로젝트 진행 간 CORS와 CSRF 설정에서 있었던 일을 정리

- CORS  
    + 프로토콜이 다르거나(http, https), 도메인이 다르거나, 포트 번호가 다르다면 접근을 막아버린다.
    + Credentials 부분, 만약 false로 되어있다면 내가 인증 토큰을 원해도 request에 주지 않는다. 이는 로그인 유지가 안되는 치명적 결함으로 이어지기 때문에 반드시 true로 설정해주어야한다.
    + 만약 Header에 Authorization에 토큰 값을 내려보낸다면 해당 부분도 추가를 해줘야 사용이 가능하다. 

- CSRF
    + 토큰으로 서버와 클라이언트 사이에 공유되는 인증값이다.
    + Rest Api 서버의 경우 stateless하다. 즉, 서버에 클라이언트의 상태를 저장해놓지 않는다. 이는 CSRF 토큰을 이용해서 세션 인증을 확인할 필요가 없다는 것이다.
    + api 서버는 jwt 혹은 auth를 이용해 받기 때문에 csrf를 사용하지 않고 보안 상의 이점을 가져갈 수 있다.


> 💡 브라우저에서 바로 요청을 보내면 브라우저에서 서버에 바로 요청을 보내고 브라우저에서 응답을 받기 때문에 동일 출처가 되는거고 내가 클라이언트를 거쳐서 서버에 요청을 보내는 경우는 브라우저가 아닌 클라이언트 서버를 거치는 과정이 생기기 때문에 브라우저 딴에서는 cors 정책 위반을 한다고 생각하는거야?   
> 브라우저에서 직접 요청을 보내는 경우, 브라우저는 이를 "same-origin" 요청으로 간주합니다. 이는 브라우저가 요청을 보내는 출처와 요청이 보내지는 대상이 동일하다고 판단하기 때문입니다. 따라서 이 경우에는 CORS 정책이 적용되지 않습니다.   
> 그러나 웹 애플리케이션을 통해 요청을 보내는 경우, 브라우저는 이를 "cross-origin" 요청으로 간주합니다. 이는 웹 애플리케이션(클라이언트)의 출처와 요청이 보내지는 대상(서버)이 다르다고 판단하기 때문입니다. 따라서 이 경우에는 CORS 정책이 적용되며, 서버는 CORS 헤더를 포함한 응답을 반환하여 브라우저에게 해당 도메인의 리소스에 접근을 허용한다는 것을 알려야 합니다.   
> 따라서, 클라이언트를 거쳐서 서버에 요청을 보내는 경우에는 CORS 설정이 필요하며, 이 설정은 서버 측에서 이루어집니다. 이를 통해 서버는 특정 도메인의 클라이언트에서의 요청을 허용하도록 설정할 수 있습니다.

### 출처
[Spring Security](https://docs.spring.io/spring-security/reference/)