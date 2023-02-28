## CORS(Cross-Origin Resource Sharing)

- 교차 출처(다른 출처) 간의 자원을 공유하는 정책

### 1. 출처(Origin)이란?

- Protocol + host + port를 의미
<img width="601" alt="스크린샷 2023-02-28 오후 7 21 22" src="https://user-images.githubusercontent.com/62919440/221825817-1ca790c2-3d2d-4ac2-8724-85e84d971de4.png">


## SOP(Same Origin Policy)

- 같은 출처만 허용하는 정책
- 최근에는 SOP의 예외조항인 CORS 정책을 둠

## CORS의 동작 과정

- 기본적으로 웹 클라이언트 애플리케이션이 다른 출처의 리소스를 요청할 때 HTTP 프로토콜을 사용해서 요청을 보냄
- 요청 헤더에 Origin 필드에 출처를 담아 보냄
- 이후 응답 헤더의 Access-Control-Allow-Origin이라는 값에 허용된 출처를 내려줌
- 이 것을 비교해 응답이 유효한 응답인지 아닌지 결정

### 1. Preflight Request

- 브라우저는 요청을 한번에 보내지 않고 예비 요청(Preflight) + 본 요청으로 나누어 전송
- OPTIONS 메서드 사용
- 본 요청을 보내기 전 스스로 이 요청을 보내는 것이 안전한지 확인하는 것
- 플로우 차트
<img width="647" alt="스크린샷 2023-02-28 오후 7 22 08" src="https://user-images.githubusercontent.com/62919440/221825989-8823c4cb-a73b-428e-986d-f5caadb80be3.png">

    
    

### 2. Simple Request

- 예비 요청을 보내지 않고 바로 서버에 본 요청 보냄
- 그 후 CORS 정책 위반 여부 검사
- 특정 조건을 만족하는 경우에만 사용 가능
    - 요청의 메소드는 GET, HEAD, POST중 하나여야 함
    - Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더 사용 불가
    - 만약 Content-Type을 사용하는 경우 application/x-www-form-urlencoded, multipart/form-data, text/plain만 허용
- 플로우 차트
<img width="640" alt="스크린샷 2023-02-28 오후 7 22 40" src="https://user-images.githubusercontent.com/62919440/221826137-7030eef8-00d5-428b-b7a5-86b6f8246f39.png">


### 3. Credentialed Request

- 인증된 요청을 사용하는 방법
- CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법

## CORS를 해결할 수 있는 방법

### 1. CrossOrigin 어노테이션 사용

- @CrossOrigin으로 출처 설정
    - 컨트롤러 클래스 단에서 설정
    - 메서드 단에서 설정

### 2. WebMvcConfigurer에서 설정

```java
@Bean
public WebMvcConfigurer corsConfigurer() {
    return new WebMvcConfigurer() {
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**").allowedOrigins("http://front-server.com");
        }
    };
}
```

- main함수에 Bean으로 configurer 추가
