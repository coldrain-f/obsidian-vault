# 개요

- LangChain의 PromptTemplate을 활용하여 반복적인 프롬프트 작성을 최적화하는 방법을 알아봅니다
- 기본적인 템플릿 구성부터 다중 변수 활용까지 실용적인 예제를 통해 설명합니다
- 일본어-한국어 번역 사례를 통해 실제 적용 방법을 살펴봅니다

# 문제 상황

LLM을 활용할 때 우리는 종종 비슷한 패턴의 프롬프트를 반복해서 작성하게 됩니다. 특히 일본어를 한국어로 번역하는 작업에서는 다음과 같은 지시사항이 자주 등장합니다:

- "~를 한국어로 번역해 주세요"
- "경어체로 번역해 주세요"
- "자연스럽게 번역해 주세요"

예를 들어 다음과 같은 상황을 생각해볼 수 있습니다:

```
입력: "何をしている？"라는 문장을 한국어로 번역해줘. 번역문은 항상 경어를 사용해줘.
출력: "무엇을 하고 계신가요?"
```

만약 경어 사용을 지정하지 않았다면, ChatGPT는 "무엇을 하고 있어?"와 같은 반말로 번역했을 것입니다. 이러한 반복적인 작업을 더 효율적으로 관리할 방법이 필요합니다.

# LangChain의 PromptTemplate 소개

LangChain은 이러한 문제를 해결하기 위한 우아한 솔루션을 제공합니다. 템플릿을 미리 정의해두고 필요한 변수만 교체하여 사용할 수 있어, 프롬프트 작성의 일관성과 효율성을 크게 높일 수 있습니다.

## 기본 템플릿 구성하기

가장 기본적인 형태의 PromptTemplate 사용법은 다음과 같습니다:

```python
from langchain.prompts import PromptTemplate
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

# 템플릿 정의
template = PromptTemplate.from_template(
    """{sentence}라는 문장을 한국어로 번역해줘.
    번역문은 항상 경어를 사용해줘."""
)

# 모델 설정
model = ChatOpenAI(model="gpt-4-mini", temperature=0)

# 체인 생성 및 실행
chain = template | model
result = chain.invoke({"sentence": "何をしている？"})
print(result.content)
```

## 다중 변수를 활용한 고급 템플릿

더 유연한 사용을 위해 여러 변수를 활용할 수 있습니다. 

```python
# 다중 변수 템플릿
template = PromptTemplate.from_template(
    """{sentence}라는 문장을 {language}로 번역해줘.
    번역문은 항상 경어를 사용해줘."""
)

# 여러 변수 입력
input = {
    "sentence": "何をしている？",
    "language": "한국어"
}
result = chain.invoke(input)
print(result.content)
```

# 사용시 주의사항

1. **변수 완전성**: 템플릿에 정의된 모든 변수는 `chain.invoke()` 실행 시 반드시 제공되어야 합니다. 하나라도 빠지면 에러가 발생합니다.
2. **변수 구문**: 변수명은 반드시 중괄호(`{}`)로 감싸야 합니다. 이는 파이썬의 문자열 포매팅 문법과 유사합니다.
3. **리터럴 중괄호**: 템플릿 내에서 중괄호 자체를 텍스트로 사용하고 싶다면 두 번 작성하면 됩니다: `{{이렇게}}`

# 마무리

PromptTemplate을 활용하면 반복적인 프롬프트 작성 작업을 크게 줄일 수 있습니다. 또한 템플릿화를 통해 프롬프트의 품질을 일관되게 유지할 수 있으며, 필요에 따라 유연하게 수정할 수 있습니다.

이러한 도구를 활용하면 LLM 기반 애플리케이션 개발에서 보다 체계적이고 유지보수가 용이한 코드를 작성할 수 있습니다.