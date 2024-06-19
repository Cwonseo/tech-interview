# CORS(Cross-Origin Resource Sharing)

## 💡 CORS란?
- 보안은 중요하지만, 다른 출처간의 모든 상호작용을 허용하지 않는다면 인터넷 세계를 사용할 수 없을 것 → 그래서 생겨난 것이 CORS!
- CORS는 HTTP 헤더를 사용해 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 선택한 자원에 접근할 수 있는 권한**을 부여하도록 브라우저에 알려주는 체제
- Client에서 Server에 요청을 하고 응답을 받을 때, **출처가 같지 않으면** 브라우저에서 CORS 에러 발생

## 🌍 브라우저의 기본 동작

1. Client에서는 HTTP 요청 헤더에 Origin이라는 데이터를 담아 전달한다.

![image1](https://github.com/Cwonseo/tech-interview/assets/88311377/26186274-bc55-4142-b2a7-9a76392a636f)

2. Server는 응답 헤더에 Access-Control-Allow-Origin이라는 데이터를 담아 전달한다.

![image2](https://github.com/Cwonseo/tech-interview/assets/88311377/d5065cc9-d77a-4503-ab34-740309c869d1)

3. 이 경우, Origin(출처)과 Access-Control-Allow-Origin(접근이 허용된 출처)가 동일하기 때문에 브라우저에서 해당 응답을 사용하게 된다.
   
4. 만약 동일하지 않다면, 응답 데이터를 사용하지 않고 CORS 오류가 발생했을 것이다.


## ❓ CORS 해결 방법
> CORS는 **Client**와 **Server** 모두 해결을 할 수 있음!
- 직접 Server를 개발한다면 **Server** 측에서 해결할 수 있고, 만약 Open API를 사용하거나 Server를 수정할 수 없다면, **Client** 측에서 해결해야 함

### 1️⃣ Client 측 해결 방법
- **⭐️ 프록시 서버 사용**: 프록시 서버를 설정하여, CORS 정책을 **우회** 할 수 있다. 클라이언트는 프록시 서버에 요청을 보내고, 프록시 서버는 실제 서버에 요청을 전달한다.
- **브라우저 플러그인 사용**: CORS 문제를 우회하기 위해 브라우저 플러그인을 사용할 수 있다. 예를 들어, Chrome에서는 "CORS Unblock"과 같은 플러그인을 사용할 수 있다. → 개발 및 테스트 목적으로만 사용
- **JSONP 사용**: JSONP(JSON with Padding)는 GET 요청을 통해 다른 도메인에서 데이터를 요청할 수 있는 방법이다. → JSONP는 GET 요청만 가능하며, 보안상의 이유로 많이 사용되지 X

### 2️⃣ Server 측 해결 방법
- **CORS 헤더 설정**: 서버에서 CORS 헤더를 설정하여 특정 출처에서의 요청을 허용할 수 있다. (서버의 응답 헤더에 `Access-Contro-Allow-Origin` 설정)

1. `@CrossOrigin` 어노테이션 사용

**클래스 레벨에 적용)**
```java
@RestController
@CrossOrigin(origins = "http://example.com")
public class MyController {

    @GetMapping("/greeting")
    public String greeting() {
        return "Hello, World!";
    }
}
```


**메서드 레벨에 적용)**
```java
@RestController
public class MyController {

    @CrossOrigin(origins = "http://example.com")
    @GetMapping("/greeting")
    public String greeting() {
        return "Hello, World!";
    }
}
```


2. `WebMvcConfigurer` 사용하여 글로벌 설정
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") // 모든 경로에 대해
                .allowedOrigins("http://example.com") // 특정 출처 허용
                .allowedMethods("GET", "POST", "PUT", "DELETE") // 허용할 HTTP 메서드
                .allowedHeaders("*") // 허용할 헤더
                .allowCredentials(true); // 자격 증명 허용
    }
}
```

3. 스프링 부트 설정 파일에서 설정
- 스프링 부트 2.4 이상에서 가능
```yml
spring:
  mvc:
    cors:
      mappings: /api/**
      allowed-origins: http://example.com
      allowed-methods: GET,POST,PUT,DELETE
      allowed-headers: "*"
      allow-credentials: true
```

4. Preflight 요청 처리
브라우저는 단순 요청이 아닌 경우, 사전 요청(preflight request)을 보낸다. 서버는 이러한 사전 요청에 대해 적절히 응답해야 한다. 이는 OPTIONS 메서드를 처리하는 엔드포인트를 설정함으로써 이루어진다.

```java
@RestController
public class MyController {

    @RequestMapping(value = "/**", method = RequestMethod.OPTIONS)
    public void handleOptions() {
        // CORS 설정 처리
    }
}
```

- 클라이언트가 실제 요청을 보내기 전에 브라우저는 OPTIONS 메서드를 사용하여 서버에 사전 요청 전송. → 이 요청은 실제 요청이 안전하게 전송될 수 있는지 확인하기 위한 것
- 서버는 이 OPTIONS 요청에 대한 응답으로 CORS 관련 헤더를 포함하여 브라우저에 반환 → 요청이 허용되는 메서드와 헤더 등 포함
- 브라우저는 서버의 응답을 확인한 후, 실제 요청을 서버로 전송
- 사전 요청 실패 → 브라우저는 실제 요청을 보내지 않고 CORS 에러를 발생시킴