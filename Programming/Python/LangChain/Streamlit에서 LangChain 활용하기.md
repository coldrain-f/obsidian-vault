# 개요
LangChain과 Streamlit을 함께 사용할 때 알아두면 유용한 두 가지 핵심 기능을 소개합니다. 특히 ChatGPT와 같은 AI 모델의 응답을 스트리밍하고 채팅 인터페이스를 구현할 때 매우 유용한 기능들입니다.

# write_stream() - AI 응답 스트리밍하기

Streamlit의 `write_stream()` 함수는 LangChain의 ChatOpenAI 모델과 완벽하게 호환됩니다. 이를 통해 AI 응답을 실시간으로 타이핑되는 것처럼 표시할 수 있습니다.
## 기본 사용법

```python
import streamlit as st
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model="gpt-4o-mini", temperature=0)
message_chunk = model.stream("대한민국의 수도는?")

st.write_stream(message_chunk) # 정상 출력
```

`write_stream()` 을 사용하면 AI 모델의 응답을 실시간으로 확인 가능합니다.

## 전체 응답 반환받기

```python
import streamlit as st from langchain_openai
import ChatOpenAI

message_chunk = model.stream("대한민국의 수도는?")
response = st.chat_message("assistant").write_stream(message_chunk)

# 완성된 메시지 반환
st.write(response)  # 대한민국의 수도는 서울입니다.
```

Streamlit의 `write_stream()` 메서드는 스트리밍으로 출력되는 응답을 화면에 표시하면서, 동시에 전체 응답 텍스트를 문자열로 반환합니다. 이는 생성된 응답을 나중에 재사용하거나 저장해야 할 때 특히 유용합니다.

# chat_message() - 채팅 인터페이스 구현하기

Streamlit의 `chat_message()` 함수는 LangChain의 ChatMessage 구조와 잘 어울려서 직관적인 채팅 인터페이스를 만들 수 있습니다.

```python
import streamlit as st
from langchain_core.messages.chat import ChatMessage
from langchain_openai import ChatOpenAI
from typing import List

messages: List[ChatMessage] = []
messages.append(ChatMessage(role="assistant", content="Hello"))
messages.append(ChatMessage(role="human", content="Hi"))

for message in messages:
    st.chat_message(message.role).write(message.content)
```

이 방식의 특징:
- 깔끔한 채팅 UI 구현
- 역할별(사용자/어시스턴트) 메시지 구분
- 코드가 직관적이고 이해하기 쉬움

이러한 기능들을 활용하면 AI 챗봇이나 대화형 인터페이스를 쉽게 구현할 수 있습니다. 특히 실시간 응답과 대화 히스토리 관리가 필요한 애플리케이션에서 매우 유용하게 사용될 수 있습니다.