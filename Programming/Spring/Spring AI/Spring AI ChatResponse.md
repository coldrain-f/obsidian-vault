# ChatResponse

```java
@RestController
public class ChatApiController {

	private final ChatClient chatClient;
	
	public ChatApiController(ChatClient.Builder chatClientBuilder) {
		this.chatClient = chatClientBuilder.build();
	}

	@GetMapping("/chat/metadata")
	public ChatResponse chatResponse() {
		return chatClient.prompt()
			.user("대한민국의 수도가 어디에요?")
			.call()
			.chatResponse();
	}
}
```

`ChatResponse`객체는 AI 모델에게 요청을 보내고 응답을 받을 때 생성되는 객체로, 응답이 생성된 방법에 대한 다양한 메타데이터가 포함되어 있습니다. 어떤 모델을 사용하고 있는지, 사용된 토큰 수는 얼마나 되는지 등의 중요한 정보를 확인해볼 수 있습니다.

## ChatResponse JSON

OpenAI의 ChatResponse를 JSON으로 확인한 결과:

```json
{
  "result": {
    "metadata": {
      "finishReason": "STOP",
      "contentFilters": [],
      "empty": true
    },
    "output": {
      "messageType": "ASSISTANT",
      "metadata": {
        "finishReason": "STOP",
        "refusal": "",
        "index": 0,
        "role": "ASSISTANT",
        "id": "chatcmpl-Ax2olTzBVxWzAmOEVkgIbcjTfPiyQ",
        "messageType": "ASSISTANT"
      },
      "toolCalls": [],
      "media": [],
      "content": "대한민국의 수도는 서울입니다.",
      "text": "대한민국의 수도는 서울입니다."
    }
  },
  "metadata": {
    "id": "chatcmpl-Ax2olTzBVxWzAmOEVkgIbcjTfPiyQ",
    "model": "gpt-4o-2024-08-06",
    "rateLimit": {
      "requestsLimit": 500,
      "requestsRemaining": 499,
      "requestsReset": "PT0.12S",
      "tokensLimit": 30000,
      "tokensRemaining": 29973,
      "tokensReset": "PT0.054S"
    },
    "usage": {
      "completionTokenDetails": {
        "reasoningTokens": 0,
        "acceptedPredictionTokens": 0,
        "audioTokens": 0,
        "rejectedPredictionTokens": 0
      },
      "rejectedPredictionTokens": 0,
      "promptTokensDetailsCachedTokens": 0,
      "promptTokensDetails": {
        "audioTokens": 0,
        "cachedTokens": 0
      },
      "acceptedPredictionTokens": 0,
      "reasoningTokens": 0,
      "audioTokens": 0,
      "totalTokens": 25,
      "generationTokens": 9,
      "promptTokens": 16
    },
    "promptMetadata": [],
    "empty": false
  },
  "results": [
    {
      "metadata": {
        "finishReason": "STOP",
        "contentFilters": [],
        "empty": true
      },
      "output": {
        "messageType": "ASSISTANT",
        "metadata": {
          "finishReason": "STOP",
          "refusal": "",
          "index": 0,
          "role": "ASSISTANT",
          "id": "chatcmpl-Ax2olTzBVxWzAmOEVkgIbcjTfPiyQ",
          "messageType": "ASSISTANT"
        },
        "toolCalls": [],
        "media": [],
        "content": "대한민국의 수도는 서울입니다.",
        "text": "대한민국의 수도는 서울입니다."
      }
    }
  ]
}
```