# 개요

이 게시물에서는 LangChain의 CharacterTextSplitter를 활용하여 텍스트를 효과적으로 분할하는 방법에 대해 알아보겠습니다. 텍스트 분할은 대규모 언어 모델(LLM)을 활용할 때 매우 중요한 전처리 과정입니다.

- CharacterTextSplitter의 기본 작동 원리
- 구분자(separator) 설정과 활용
- chunk_size와 chunk_overlap 매개변수의 이해
- 실제 사용 사례와 주의사항

# 실습용 텍스트 파일
```text
커피 추출 방법
커피를 추출하는 방법에는 여러 가지가 있으며, 각각의 방식에 따라 독특한 맛과 향이 만들어집니다. 에스프레소 머신으로 고압으로 추출하거나, 드립 방식으로 천천히 우려내는 등 다양한 방법이 있습니다.

도구 준비
커피 추출에는 기본적으로 필요한 도구들이 있습니다. 그라인더로 원두를 갈고, 추출 도구와 물의 온도를 측정할 수 있는 온도계, 그리고 정확한 양을 측정하기 위한 저울이 필요합니다.

추출 과정
커피 추출은 정확한 비율과 시간을 지켜야 합니다. 원두와 물의 비율은 보통 1:15에서 1:18 정도를 사용하며, 추출 시간은 방법에 따라 30초에서 4분까지 다양합니다.

원두 선택
좋은 커피를 만들기 위해서는 원두 선택이 중요합니다. 원두는 크게 다음과 같이 구분됩니다:

라이트 로스팅: 산미가 강하고 과일향이 특징
미디엄 로스팅: 균형 잡힌 맛과 향
다크 로스팅: 진한 맛과 쓴맛이 특징

보관 방법
원두는 적절한 보관이 매우 중요합니다. 공기와 직사광선을 피해 밀봉 용기에 보관하며, 가급적 구매 후 한 달 이내에 사용하는 것이 좋습니다. 신선한 원두일수록 더 풍부한 향과 맛을 즐길 수 있습니다.
```

# CharacterTextSplitter 기본 원리

CharacterTextSplitter는 기본적으로 `\n\n`(두 개의 줄바꿈)을 구분자로 사용합니다. 이는 일반적인 텍스트 문서의 단락 구분과 일치하여 자연스러운 분할을 가능하게 합니다.

```text
커피 추출 방법\n
커피를 추출하는 방법에는 여러 가지가 있으며, 각각의 방식에 따라 독특한 맛과 향이 만들어집니다. 에스프레소 머신으로 고압으로 추출하거나, 드립 방식으로 천천히 우려내는 등 다양한 방법이 있습니다.\n
\n

...
```

예를 들어, 커피 추출 방법에 대한 텍스트는 `\n\n`문자가 연속되는 부분이 분할되어 다음과 같이 자연스럽게 분할됩니다:

```text
커피 추출 방법
커피를 추출하는 방법에는 여러 가지가 있으며, 각각의 방식에 따라 독특한 맛과 향이 만들어집니다. 에스프레소 머신으로 고압으로 추출하거나, 드립 방식으로 천천히 우려내는 등 다양한 방법이 있습니다.
```

# 주요 매개변수 설정

CharacterTextSplitter를 초기화할 때 다음과 같은 중요한 매개변수들을 설정할 수 있습니다:

```python
text_splitter = CharacterTextSplitter(
    chunk_size=90,      # 청크 크기 (글자 수)
    chunk_overlap=0,    # 청크 간 중복되는 글자 수
    length_function=len # 길이 계산 함수
)
```

## 주의사항

1. chunk_size를 초과하는 경우:
   - 설정된 chunk_size보다 큰 텍스트 블록이 발생할 수 있습니다
   - 이 경우 경고 메시지가 출력됩니다
   - 예: "Created a chunk of size 119, which is longer than the specified 90"

2. 길이 계산:
   - 기본적으로 Python의 `len` 함수를 사용합니다
   - 한 글자당 1의 길이로 계산됩니다
   - 필요한 경우 커스텀 length_function을 정의할 수 있습니다

# 실제 사용 예시

```python
from langchain_text_splitters import CharacterTextSplitter

with open("./files/text.txt", encoding="utf-8") as f:
    text = f.read()

splitter = CharacterTextSplitter(chunk_size=90, chunk_overlap=0)
documents = splitter.create_documents([text])
```

이렇게 설정된 CharacterTextSplitter는 텍스트를 의미 있는 단위로 분할하여, LLM이 효과적으로 처리할 수 있는 크기의 청크로 만들어 줍니다.

이후에 `Loader`를 사용해서 `List<Document>`를 반환받는 후에 텍스트 분할을 하려면 `text_splitter.split_documents()`를 사용하면 되고, 문자열(`str`)을 분할 하려면 `text_splitter.split_text()`함수를 사용하면 됩니다.

# 활용 팁

- 문서의 특성에 따라 chunk_size를 조절하세요
- 문맥 유지가 중요한 경우 chunk_overlap을 활용하세요
- 경고 메시지가 발생하면 chunk_size를 적절히 조정하세요
- 특수한 구분자가 필요한 경우 커스텀 구분자를 설정할 수 있습니다

이러한 설정들을 통해 더욱 효과적인 텍스트 처리가 가능해집니다.