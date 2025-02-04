---
cssclasses:
  - img-grid
  - img-zoom
---
# 개요

본 글에서는 Spring AI를 활용하여 ChatGPT API를 연동하는 과정을 가볍게 다룹니다. Spring Boot 3.4.2와 Java 17을 기반으로 하며, OpenAI API를 통한 간단한 응답 확인까지 살펴봅니다.

- Spring AI 프로젝트 초기 설정
- OpenAI API 키 설정 및 환경 구성
- 기본적인 채팅 API 엔드포인트 구현
- 실제 ChatGPT 응답 테스트

![Welcome](assets/images/spring-ai/01.png)

# 프로젝트 생성

Spring Initializr를 통해 다음과 같은 스펙으로 프로젝트를 구성했습니다:

- **빌드 도구**: Gradle (Groovy)
- **Java 버전**: 17
- **Spring Boot**: 3.4.2 (Spring AI 요구사항 충족)
- **패키징**: War

## 주요 의존성
- `Spring Web`: RestController 사용을 위해 추가
- `Spring Boot Dev Tools`: 개발 생산성 향상
- `OpenAI`: OpenAI의 ChatGPT 연동을 위해 추가

> 💡 **참고**: Spring AI 공식 문서에 따르면 2025년 2월 기준 Spring Boot 3.2.x 이상 버전이 필수입니다.

# 개발 환경 설정

1. STS4에서 프로젝트 임포트:
   - `File > Import > Gradle` 경로로 프로젝트를 불러옵니다.

2. OpenAI API 키 설정:
   초기 실행 시 다음과 같은 에러가 발생할 수 있습니다:
   ```
   OpenAI API key must be set. Use the connection property: spring.ai.openai.api-key or spring.ai.openai.chat.api-key property.
   ```

3. `application.properties`에 API 키 추가:
```properties
spring.ai.openai.api-key=<YOUR OPENAI KEY>
```

## ChatGPT API 구현

간단한 채팅 기능을 위한 컨트롤러를 구현했습니다:

```java
@RestController
public class ChatApiController {
    private final ChatClient chatClient;
    
    public ChatApiController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    @GetMapping("/chat")
    public String completion() {
        String question = "대한민국의 수도가 어디에요?";
        String answer = chatClient.prompt()
            .user(question)
            .call()
            .content();
    
        return answer;
    }
}
```

ChatClient는 ChatClient.Builder 객체를 사용하여 생성됩니다.

실제 테스트 결과 "대한민국의 수도는 서울입니다."라는 응답을 받아 정상 작동을 확인했습니다.