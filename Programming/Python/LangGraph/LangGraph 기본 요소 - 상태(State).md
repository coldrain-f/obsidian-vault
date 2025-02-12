LangGraph의 기본 구성 요소인 State(상태)에 대해 알아보겠습니다. State는 노드 간 정보 전달의 핵심 요소로, Python의 고급 타입 힌팅 기능을 활용하여 구현됩니다.

# 개요
- State의 기본 구조와 역할
- Annotated 타입 힌팅의 활용
- add_messages의 특별한 기능
- State 객체의 유연한 사용법

# State란?
LangGraph에서 State는 노드 간 정보 전달을 담당하는 핵심 객체입니다. TypedDict를 상속받아 구현되며, 다음과 같은 구조를 가집니다:

```python
from typing import Annotated, TypedDict
from langgraph.graph.message import add_messages

class GraphState(TypedDict):
    question: Annotated[list, add_messages]  # 질문 리스트
    context: Annotated[str, "Context"]       # LLM 참고 문서
    messages: Annotated[list, add_messages]  # 메시지 리스트
```

# Python 3.9의 Annotated 타입 힌팅
Annotated는 Python 3.9에서 도입된 고급 타입 힌팅 기능입니다. 변수의 타입과 함께 추가적인 메타데이터를 정의할 수 있습니다:

```python
context: Annotated[str, "Context"]
```
- 첫 번째 인자: 실제 타입 (str)
- 두 번째 인자: 타입에 대한 메타데이터 ("Context")

# add_messages의 특별한 기능
LangGraph의 add_messages는 메시지를 누적하는 특별한 기능을 제공합니다. 일반적인 리스트 할당과 비교해보겠습니다:

## 일반적인 리스트 할당
```python
from langchain_core.messages import AIMessage, HumanMessage

messages = []
messages = [HumanMessage(context="Hello", id=1)]  # 첫 번째 메시지만 저장
messages = [AIMessage(context="Hi", id=2)]        # 두 번째 메시지로 덮어쓰기
```

## add_messages 사용
```python
from langgraph.graph.message import add_messages

add_messages([HumanMessage(context="Hello", id=1)])  # 첫 번째 메시지 추가
add_messages([AIMessage(context="Hi", id=2)])        # 두 번째 메시지 누적
# 결과: [HumanMessage(...), AIMessage(...)]
```

add_messages는 일반적으로 메시지를 누적하지만, 동일한 id를 가진 메시지가 있을 경우 누적하지 않고 기존 메시지를 새로운 메시지로 대체합니다.

# State 객체의 유연성
State 객체의 또 다른 특징은 유연한 사용성입니다. GraphState에 정의된 세 가지 필드(question, context, messages)를 모두 채울 필요가 없습니다. 필요한 필드만 선택적으로 사용할 수 있어 효율적인 데이터 전달이 가능합니다.

이러한 State의 특징들은 LangGraph에서 복잡한 대화형 시스템을 구현할 때 매우 유용하게 활용됩니다. 특히 add_messages를 통한 메시지 누적 기능은 대화 히스토리를 관리하는 데 탁월한 솔루션을 제공합니다.