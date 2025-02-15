# 개요

LangGraph는 LangChain의 확장 라이브러리로, LLM 애플리케이션의 복잡한 상태 관리와 제어 흐름을 단순화하는 도구입니다. 이 글에서는 LangGraph를 사용하여 간단한 챗봇을 구현하는 방법을 단계별로 알아보겠습니다. 특히 상태 관리, 메시지 누적, 그리고 노드 간의 통신 방식에 대해 자세히 살펴볼 것입니다.

# Step 0. 환경 설정 및 라이브러리 임포트

```python
from typing import Annotated
from typing_extensions import TypedDict

from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langchain_openai import ChatOpenAI

from IPython.display import Image, display
from dotenv import load_dotenv

load_dotenv()
```

이 단계에서는 필요한 모든 의존성을 임포트합니다. 주요 컴포넌트들을 살펴보면:

- `StateGraph`: LangGraph의 핵심 클래스로, 상태 기반 워크플로우를 정의합니다.
- `START`, `END`: 그래프의 시작과 끝을 나타내는 특별한 노드입니다.
- `add_messages`: 메시지를 누적하는 데 사용되는 특별한 리듀서입니다.
- `ChatOpenAI`: LangChain의 OpenAI 챗봇 인터페이스입니다.

# Step 1. 그래프 상태 정의

```python
class GraphState(TypedDict):
	messages: Annotated[list, add_messages]
```

상태 관리는 챗봇 시스템에서 가장 중요한 부분 중 하나입니다. LangGraph에서는 `TypedDict`를 사용하여 상태를 정의하는데, 여기서 주목할 점들이 있습니다:

1. **메시지 누정 방식**
	- `add_messages` 리듀서는 새로운 메시지가 들어올 때마다 이전 메시지들을 보존합니다.
	- 대화 기록이 [`HumanMessage`, `AIMessage`, `HumanMessage`, ...]와 같은 형태로 누적됩니다.
2. **타입 안정성**
	- `Annotated`를 사용함으로써 타입 체커가 상태 객체의 구조를 이해할 수 있습니다.
	- 이는 개발 시 발생할 수 있는 실수를 미리 방지해줍니다.

# Step 2. 노드 정의

```python
def chatbot(state: State):
	llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)
	response = llm.invoke(state["messages"])
	
	return GraphState(messages=[response]) # 덮어쓰기 X, 누적 O
```

챗봇 노드는 실제로 LLM과 상호작용하는 부분입니다. 주요 특징들을 살펴보면:

1. **상태 처리**
	- 노드는 현재 상태(`state`)를 입력으로 받습니다.
	- `state["messages"]`를 통해 현재까지의 대화 기록에 접근합니다.
2. **LLM 설정**
	- `temperature=0`을 설정하여 결정론적인 응답을 유도합니다.
	- 응답은 `AIMessage` 객체로 반환되며, 이는 자동으로 메시지 히스토리에 추가됩니다.
3. **반환 값**
	- `add_messages` 리듀서에 의해 기존 메시지들과 자동으로 병합됩니다.
		- 메시지가 누적된다는 말은 노드와 노드 간 상태를 공유할 때를 의미합니다.