## Chatbot API

### 질의에 대한 응답 생성

#### HTTP Request
`POST /api/v1/chatbot/completions`

#### Request Body

```json
{
    "question": "납입금을 연체한 경우 추후에라도 납입할 수 있나요?",
    "include_additional_info": true
}
```

#### Request Parameters
| Name                    | Type    | Required | Description             |
| ----------------------- | ------- | -------- | ----------------------- |
| question                | String  | Yes      | 사용자의 질문 내용              |
| include_additional_info | Boolean | No       | 추가 정보 포함 여부 (기본값: true) |

#### Response
```json
{
	"markdown_summary": "",
    "pdf_images_base64": [],
    "answer": "",
    "documents": []
}
```