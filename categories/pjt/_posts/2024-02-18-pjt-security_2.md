---
layout: post
title:  "[Spring Security] JWT"
description: >
    "Spring Security JWT를 이용한 인증/인가"

hide_last_modified: true
---
* TOC
{:toc}
## Setting
Spring Boot 3.2.1   
Spring Security 6.2.1   
jjwt 0.12.4

# JWT(Json Web Tokens)
## JWT 생성 & 검증
### 알고리즘
- signature에 들어갈 알고리즘으로 가장 범용적인 HMAC512 알고리즘을 사용
- 다른 알고리즘도 존재하지만 대규모 프로젝트에 유효하고 범용적으로 사용되는 것은 HMAC 알고리즘
- 256과 512의 차이는 512가 더 빠른 특징이 있지만 CPU 32bit 이하의 성능에서는 256이 권장된다고 한다.

### JWT 구조
- 토큰은 header, payload, signature로 이루어진다.
    - header에는 기본정보를 저장
    - payload에는 issuer, userno, issuedate 등 필요한 정보들을 저장
    - signature에는 선택한 알고리즘과 payload등 부분들을 이용 Secret Key값과 함께 암호화해서 검증할 수 있는 검증 데이터가 들어간다.


### JWT 생성
- 세부 설정을 숨기기 위해 yml 파일을 이용해 JWT에 들어갈 값들을 저장해준다. 
```yaml

app:
  JWT_COOKIE_A: "ACCESS"
  JWT_COOKIE_R: "REFRESH"
  JWT_KEY : "d1f33e03-f067-4315-af91-b03f91f4cb19"
  JWT_REFRESH_EXPIRATION : "604800000"
  JWT_ACCESS_EXPIRATION : "1800000"
```

```java
    // 실제 JWT 생성 코드
	private String createAccessToken(int memberNo) {
        Date date = new Date();
        SecretKey key = Keys.hmacShaKeyFor(JWT_KEY.getBytes(StandardCharsets.UTF_8));

        return Jwts.builder()
                .issuer("GoldZZok")
                .claim("id", memberNo)
                .issuedAt(date)
                .expiration(new Date(date.getTime() + Integer.parseInt(JWT_ACCESS_EXPIRE)))  // 30분 설정
                .signWith(key).compact();
    }

    private String createRefreshToken(int memberNo) {
        Date date = new Date();
        SecretKey key = Keys.hmacShaKeyFor(JWT_KEY.getBytes(StandardCharsets.UTF_8));

        return Jwts.builder()
                .issuer("GoldZZok")
                .claim("id", memberNo)
                .issuedAt(date)
                .expiration(new Date(date.getTime() + Integer.parseInt(JWT_REFRESH_EXPIRE)))  // 7일 설정
                .signWith(key).compact();
    }
```

### JWT 검증
```java
    public boolean validateToken(String token) {
        SecretKey key = Keys.hmacShaKeyFor(JWT_KEY.getBytes(StandardCharsets.UTF_8));
        try {
            Jws<Claims> claims = Jwts.parser()
                    .verifyWith(key)
                    .build()
                    .parseSignedClaims(token);

            return !claims.getPayload().getExpiration().before(new Date());
        } catch (JwtException e) {
            return false;
        }
    }
```

## JWT 이용한 인증
### 구현 코드
```java
    public class JwtAuthenticationFilter extends OncePerRequestFilter {
        @Override
        protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
            String accessToken = extractAccessToken(request);
            String refreshToken = extractRefreshToken(request);

            if (null != accessToken && jwtService.validateToken(accessToken)) {  // access token이 유효함
                GrantedAuthority authority = new SimpleGrantedAuthority("ROLE_USER");
                Authentication authentication = authenticationManager.authenticate(new UsernamePasswordAuthenticationToken(jwtService.userCheck(accessToken), null, Collections.singletonList(authority)));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            } else if (null != extractRefreshToken(request) && jwtService.validateToken(refreshToken)) {
                response.setStatus(HttpStatus.UNAUTHORIZED.value());  // Refreshtoken이 유효함
            } else {
                authenticationFailureHandler(response);
            }

            filterChain.doFilter(request, response);
        }
    }
```

- 내가 만든 Custom Filter를 Security Filter Chain 내부에 동작하게 걸어 인증이 완료된다면 Security Context 내부에 Authentication 객체를 만들어준다.

### 개발 과정 인증 절차 
- request header에 넣어주기 위해 instance header를 사용하게 된다면 SPA의 특성 상 페이지 이동이 없이 이루어져 토큰이 유지가 되지만 새로고침을 누르기만 하면 access token이 사라지는 문제점이 발생한다. 
이는 access token이 사라질 때마다 server에 접근하여 새롭게 발급받아야하는 상황이 생기고 이가 반복되면 서버에 과부하가 생기게 된다.
    
- https://jwt.io/introduction - 공식 소개글 참조

![jwt](</assets/img/pjt/jwt.png>)
    
- 당연히 이렇게 header에 넣어 내려준다는 생각만을 하고 있었다. 심지어 secure cookie와 header로 토큰을 각각 나누면 한 번 더 숨기는것이므로 탈취될 위험이 적다고 생각했다.  
- 하지만 개발자도구의 network를 까는 순간 모든 정보는 노출되어 보여지고 있었고 header는 cookie처럼 http only 설정을 진행할 수 없기 때문에 JS를 이용한 XSS 공격이 가능했다. 
→ 모든 걸 secure cookie로 만들어 내려주고 관리하도록 변경, 새로고침 문제부터 XSS 문제까지 해결가능 이제 쿠키를 이용하였을 경우 CSRF 공격이 가능하므로 알아볼 필요가 있다. 
    
1. 추후 인증 과정
- Access를 가지고 있다면 jwtAuthenticationFilter에서 인증을 바로 완료하고 Authentication 객체를 바로 생성해 권한을 줄 수 있다.
- 만약 없다면, refresh 토큰을 가지고 있는지 확인한다. refresh token이 존재한다면 프론트에 response error를 내려준다.

2. error 발생 시 
- 해당 에러가 발생 시 클라이언트에서(front application) interceptor가 동작하게 되고 해당 interceptor는 auth/silent-refresh request를 자동으로 보내준다.
- refresh token이 유효하다면 access token과 refresh token을 재발급해서 할당해준다.
- 만약 유효하지 않다면? 인증할 수 있는 방법이 없기 때문에 다시 로그인하라고 끝내야된다.