---
layout: post
title:  "[Spring] HTTP 다양한 통신 방법"
description: >
    "HttpURLConnection & Rest Template & web client"

hide_last_modified: false
---
* TOC
  {:toc}
***
## HttpURLConnection
***
### 정의
***
public abstract class인 URLConnection의 Subclass로 URL 간의 통신 링크를 나타낸다.   
각 HttpURLConnection instance는 단일 요청을 만드는 데 사용되며 요청 후 InputStream 혹은 OutputStream에서 close() 메소드를 호출 시 네트워크 리소스를 해제할 수 가 있다.
또한, 서버에 다른 요청을 더 이상 보낼 필요가 없을 경우 disconnect() 메소드를 사용해서 연결을 해제할 수가 있다.   
HttpURLConnection은 Java에서 기본적으로 제공하는 클래스이기 때문에 스프링 프레임워크를 사용할 필요성이 없다.

### 예시 코드
***
```java
    @GetMapping("url_connection")
    public ResponseEntity<Map<String, Object>> getHttpURLConnection() throws IOException {
        Map<String, Object> map = new HashMap<>();
        try {
            String param = URLEncoder.encode("설현", "UTF-8");
            String apiUrl = "https://openapi.naver.com/v1/search/news.json?query=";
            URL url = new URL(apiUrl + param);

            HttpURLConnection con = (HttpURLConnection) url.openConnection();
            con.setRequestMethod("GET");

            con.setRequestProperty("X-Naver-Client-Id", clientId);
            con.setRequestProperty("X-Naver-Client-Secret", clientSecret);

            BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));

            String inputLine;
            StringBuilder response = new StringBuilder();
            while ((inputLine = br.readLine()) != null) {
                response.append(inputLine);
            }

            map.put("response", response);

            con.disconnect();
            br.close();
        } catch (Exception e){
            e.printStackTrace();
        }

        return new ResponseEntity<>(map, HttpStatus.OK);
    }

```

### 결과 화면
***
![urlConnection](/assets/img/javaStudy/httpconnection.png)


## RestTemplate
***
### 정의
Public class RestTemplate은 Spring 프레임워크에서 제공하는 HTTP 클라이언트 라이브러리로 간단하게 HTTP 통신을 진행할 수가 있는 동기식 클라이언트이다.

### 예시 코드
***
```java
    @GetMapping("rest_template")
    public ResponseEntity<Map<String, Object>> getRestTemplate(){
        Map<String, Object> map = new HashMap<>();
        try {
            String param = URLEncoder.encode("설현", StandardCharsets.UTF_8.toString());
            String apiUrl = "https://openapi.naver.com/v1/search/news.json?query=" + param;

            HttpHeaders headers = new HttpHeaders();
            headers.set("X-Naver-Client-Id", clientId);
            headers.set("X-Naver-Client-Secret", clientSecret);

            RestTemplate restTemplate = new RestTemplate();
            HttpEntity<String> httpEntity = new HttpEntity<>(headers);
            ResponseEntity<String> responseEntity = restTemplate.exchange(apiUrl, org.springframework.http.HttpMethod.GET, httpEntity, String.class);
            String responseBody = responseEntity.getBody();

            map.put("response", responseBody);
        } catch (Exception e){
            e.printStackTrace();
        }

        return new ResponseEntity<>(map, HttpStatus.OK);
    }

```

### 결과 화면
***
![restTemplate](/assets/img/javaStudy/restTemplate.png)

## WebClient
***
### 정의
***
WebClient는 스프링 5에서 소개된 비동기 및 논블록킹 웹 요청을 수행하기 위해 사용되는 Reactive web 클라이언트이다.    

### 예시 코드
***
```java
    @GetMapping("web_client")
    public Mono<ResponseEntity<Map<String, Object>>> getWebClient() {
        Map<String, Object> map = new HashMap<>();

        try {
            String param = URLEncoder.encode("설현", StandardCharsets.UTF_8.toString());
            String apiUrl = "https://openapi.naver.com/v1/search/news.json?query=" + param;

            WebClient webClient = WebClient.builder().baseUrl(apiUrl)
                    .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                    .defaultHeader("X-Naver-Client-Id", clientId)
                    .defaultHeader("X-Naver-Client-Secret", clientSecret)
                    .build();

            String responseBody = webClient.get()
                    .retrieve()
                    .bodyToMono(String.class)
                    .block(); // blocking for simplicity, consider using subscribe instead

            map.put("response", responseBody);
        } catch (Exception e) {
            e.printStackTrace();
        }

        return Mono.just(new ResponseEntity<>(map, HttpStatus.OK));
    }
```

### 결과 화면
***
![webclient](/assets/img/javaStudy/webclient.png)


### 추가(Spring WebFlux)
***
Spring WebFlux는 Spring5에서 새롭게 추가된 모듈로 Reactive 스타일의 개발을 도와준다.   

- Flux: 0 ~ N개의 데이터 전달
- Mono: 0 ~ 1개의 데이터 전달