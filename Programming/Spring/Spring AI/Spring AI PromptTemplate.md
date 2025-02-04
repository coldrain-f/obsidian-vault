# 객체 생성 방식
## PromptTempalte

```java
@RestController
public class ChatApiController {
	
	private final ChatClient chatClient;
	
	public ChatApiController(ChatClient.Builder chatClientBuilder) {
		this.chatClient = chatClientBuilder.build();
	}
	
	@GetMapping("/chat")
	public String completions(@RequestParam("question") String question) {
		
		final PromptTemplate promptTemplate = 
			new PromptTemplate("{question}의 수도가 어디에요?");
		final Prompt prompt = 
			promptTemplate.create(Map.of("question", question));
		
		return chatClient.prompt(prompt)
			.call()
			.content();
	}
}
```

PromptTemplate은 Python 진영의 LangChain에 있는 PromptTemplate과 매우 유사한 기능을 제공합니다. 

LangChain의 PromptTemplate은 이전에 다룬 적이 있으니 궁금하신 분들은 [[LangChain의 PromptTemplate 활용하기]] 문서를 읽어보세요.

PromptTemplate은 사용자 질문에 대해서 기본적인 구조를 설정할 수 있습니다.

변수 설정은 중괄호 `{}`를 사용하면 되고,  설정한 변수의 값 바인딩은 PromptTemplate의 `create()` 메서드의 인자에 Map의 `of`를 사용하여 넣어주면 됩니다.

## SystemPromptTemplate

```java
@GetMapping("/chat")
public String completions(@RequestParam("question") String question) {
	
	final String systemTemplate = """
			You're a friendly AI assistant.
			Your name is {name}.
			Answer users' questions with kindness.
			Always answer in English.
			""";
	
	final SystemPromptTemplate systemPromptTemplate = 
				new SystemPromptTemplate(systemTemplate);
	
	final Map<String, Object> input = Map.of("name", "Bob");
	final Prompt prompt = systemPromptTemplate.create(input);
	
	return chatClient.prompt(prompt)
			.user(question)
			.call()
			.content();
}
```

AI에게 규칙을 적용할 필요성이 있다면 SystemPromptTemplate을 사용하면 됩니다. 주로 역할, 이름, 행동 등을 명시하는 용도로 쓰면 좋을 것 같습니다.

- 역할
	- You're a friendly AI assistant.
- 이름
	- Your name is {name}
- 행동
	- Answer users' questions with kindness.
	- Always answer in English.

PromptTemplate을 사용해도 괜찮지만, 응답을 제한하는 데 어려움이 있습니다.

SystemPromptTemplate을 이용해 "항상 영어로 대답하세요."라고 설정하면, "대한민국의 수도는 어디에요? 한국어로 설명해 주세요."와 같은 질문에도 영어로 대답하게 됩니다.

### 추가 예시
Human: 당신의 이름은 Tom입니다.
Assistant: 안녕하세요! 제 이름은 톰이 아니라, 저는 AI 비서 밥이라고 해요. 뭐든지 궁금한 점이 있으면 말씀해 주세요!


## 같이 사용하기

```java
@GetMapping("/chat")
public String completions(@RequestParam("question") String question) {
	
	final String systemTemplate = """
			You're a friendly AI assistant.
			Your name is {name}.
			Answer users' questions with kindness.
			Always answer in Korean.
			""";
	
	final String template = """
			Explain it in a way that a 5 year old can understand
			#Question: {question}
			""";
	
	final PromptTemplate promptTemplate = new PromptTemplate(template);
	final Message userMessage = 
			promptTemplate.createMessage(Map.of("question", question));
	
	final SystemPromptTemplate systemPromptTemplate =
				new SystemPromptTemplate(systemTemplate);
	
	final Map<String, Object> input = Map.of("name", "Bob");
	final Message systemMessage = systemPromptTemplate.createMessage(input);
	
	final Prompt prompt = new Prompt(List.of(userMessage, systemMessage));
	
	return chatClient.prompt(prompt)
			.call()
			.content();
}
```

PromptTemplate과 SystemPromptTemplate을 함께 사용할 수도 있습니다. 이를 위해서는 두 객체가 공통적으로 가지고 있는 `createMessage()` 메서드를 이용하면 됩니다.

각각 `userMessage`와 `systemMessage`를 가지고 Prompt를 생성해 주면 됩니다.

PromptTemplate으로 만들어진 메시지는 MessageType.USER가 설정되고, SystemPromptTemplate으로 만들어진 메시지는 MessageType.SYSTEM으로 설정되어 구분됩니다.

# ChatClient 사용하기

```java
@GetMapping("/chat")
public String completions(@RequestParam("question") String question) {
	
	final String systemTemplate = """
			You're a friendly AI assistant.
			Your name is {name}.
			Answer users' questions with kindness.
			Always answer in Korean.
			""";
	
	final String template = """
			Explain it in a way that a 5 year old can understand
			#Question: {question}
			""";
	
	return chatClient.prompt()
			.system(s -> s.text(systemTemplate).param("name", "John"))
			.user(u -> u.text(template).param("question", question))
			.call()
			.content();
}
```

chatClient의 `system()`과 `user()`를 사용하면 객체를 생성하는 방식과 비교해 봤을 때, 굉장히 심플하게 사용할 수 있습니다. 상황에 맞게 사용하면 될 것 같습니다.

# 템플릿 파일로 관리하기

위에서 작성한 모든 예시 코드들은 프롬프트 템플릿을 String 변수로 정의해서 사용했습니다. Spring AI는 프롬프트 템플릿을 파일로 만들어서 관리하고 불러와서 사용하는 방법이 존재하는데요. 프롬프트 템플릿을 파일로 관리하면 코드도 깔끔해지고 재사용하기도 좋아집니다.


## 파일 생성

우선 `src/main/resources` 폴더에 `templates`폴더를 생성해서 `system_template.st`파일과 `basic_template.st`파일을 만들어 줍니다.

`system_template.st`:
```text
You're a friendly AI assistant.
Your name is {name}.
Answer users' questions with kindness.
Always answer in Korean.
```

`basic_template.st`
```
Explain it in a way that a 5 year old can understand
#Question: {question}
```

## 파일 적용

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;

@RestController
public class ChatApiController {
	
	@Value("classpath:/templates/system_template.st")
	private Resource systemTemplate;
	
	@Value("classpath:/templates/basic_template.st")
	private Resource basicTemplate;
	
	private final ChatClient chatClient;
	
	public ChatApiController(ChatClient.Builder chatClientBuilder) {
		this.chatClient = chatClientBuilder.build();
	}
	
	@GetMapping("/chat")
	public String completions(@RequestParam("question") String question) {
	
		return chatClient.prompt()
			.system(s -> s.text(systemTemplate).param("name", "John"))
			.user(u -> u.text(basicTemplate).param("question", question))
			.call()
			.content();
	}
}
```

