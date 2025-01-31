# 개요

- LangChain에서 프롬프트 템플릿을 YAML 파일로 관리하는 방법을 소개합니다
- 템플릿 파일의 기본 구조와 작성 방법을 살펴봅니다
- 실제 Python 코드에서 템플릿 파일을 활용하는 방법을 다룹니다
- 파일 기반 템플릿 관리의 장점과 한계, 주의사항을 설명합니다

# 프롬프트 템플릿 파일 관리의 필요성

프롬프트 엔지니어링을 하다 보면 템플릿이 점점 복잡해지고 수가 많아지게 됩니다. 이런 상황에서 모든 템플릿을 코드 안에서 직접 관리하면 유지보수가 어려워질 수 있습니다. LangChain은 이러한 문제를 해결하기 위해 YAML 형식의 템플릿 파일 관리 기능을 제공합니다.

# YAML 템플릿의 기본 구조

YAML 템플릿 파일은 다음과 같은 핵심 요소로 구성됩니다:

1. `_type`: 프롬프트의 유형을 명시 (일반적으로 "prompt"로 설정)
2. `template`: 실제 프롬프트 템플릿의 내용
3. `input_variables`: 템플릿에서 사용될 변수들의 목록

## 단순 템플릿 예시

기본적인 질문-답변 형식의 템플릿은 다음과 같이 작성할 수 있습니다:

```yaml
_type: "prompt"
template: "Make it easy to answer questions. #Question: {question}"
input_variables: ["question"]
```

## 복잡한 템플릿 예시

더 상세한 지시사항이 필요한 경우, 여러 줄 템플릿을 사용할 수 있습니다:

```yaml
_type: "prompt"
template: |
  You are an assistant for question-answering tasks.
  If you don't know the answer, just say that you don't know.
  Answer in Korean.

  #Question: {question}

  #Answer:
input_variables: ["question"]
```

# Python에서의 실제 활용

템플릿 파일을 Python 코드에서 활용하는 방법은 다음과 같습니다:

```python
from langchain_core.prompts import load_prompt
from langchain_openai import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

# YAML 파일에서 템플릿 로드
prompt = load_prompt("prompts/basic.yaml")

# ChatGPT 모델 초기화
model = ChatOpenAI(model="gpt-4-mini", temperature=0)

# 실행 체인 구성
chain = prompt | model | StrOutputParser()

# 체인 실행
answer = chain.invoke({"question": "대한민국의 수도는?"})
print(answer)
```

#  템플릿 파일 관리의 장점

1. **재사용성**
   - 동일한 템플릿을 여러 프로젝트에서 쉽게 공유
   - 팀 내 템플릿 표준화 용이

2. **효과적인 버전 관리**
   - Git과 같은 버전 관리 시스템을 통한 체계적인 이력 관리
   - 여러 버전의 템플릿을 동시에 유지/관리 가능
   - 변경 사항에 대한 코드 리뷰 프로세스 적용 가능

# 실전 사용시 주의사항

1. **YAML 문법 준수**
   - 들여쓰기는 일관성 있게 2칸 사용
   - 여러 줄 문자열 템플릿은 `|` 문자로 시작

2. **변수 관리**
   - `input_variables`에 선언된 모든 변수는 템플릿에서 반드시 사용되어야 함
   - 템플릿에서 사용되는 모든 변수는 `input_variables`에 명시되어야 함
   - 변수 구조 변경 시 관련 코드도 함께 수정 필요

3. **파일 구조화**
   - 프로젝트의 규모에 따라 적절한 디렉토리 구조 설계 필요
   - 관련된 템플릿들은 같은 디렉토리에 모아서 관리

# 마무리

프롬프트 템플릿을 YAML 파일로 관리하는 것은 프롬프트 내용 수정의 편의성을 제공하지만, 변수 구조 변경 시에는 코드 수정이 불가피합니다. 이러한 한계에도 불구하고, 파일 기반 템플릿 관리는 프로젝트의 확장성과 유지보수성을 크게 향상시킬 수 있는 효과적인 방법입니다. 특히 팀 단위의 프로젝트에서는 일관된 프롬프트 관리와 효율적인 협업을 가능하게 합니다.