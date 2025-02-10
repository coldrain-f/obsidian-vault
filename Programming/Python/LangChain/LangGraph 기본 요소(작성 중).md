
# 상태(State)

```python
from typing import Annotated, TypedDict
from langgraph.graph.message import add_messages

class GraphState(TypedDict):
	question: Annotated[list, add_messages] # 질문
	context: Annotated[str, "Context"] # LLM이 참고할 문서
	messages: Annotated[list, add_messages] # 메시지
	
```

LangGraph에서 노드와 노드 간에 정보를 전달할 때에는 상태(State)라는 객체를 이용합니다.

`Annotated`는 굉장히 생소한 문법인데, 찾아보니 Python 3.9부터 도입된 타입 힌트에 대한 메타데이터를 지정하는 방법입니다.

`context: Annotated[str, "Context"]`를 보면 첫 번째는 타입을 지정해 주고 두 번째는 타입에 대한 메타데이터를 지정해 주면 되는데, 메타데이터는 타입에 대한 설명 정도로 생각하면 될 것 같습니다.

## 첫 번째 특징

`question: Annotated[list, add_messages]`코드를 보면 두 번째에 문자열이 아닌, LangGraph의 `add_messages`함수가 들어가 있는데 이는 LangGraph에서 사용하는 독특한 형태입니다. 

첫 번째 인자에 `list`를 지정하고 두 번째 인자에 `add_messages`를 지정해 주면 question은 값을 덮어 씌우는 게 아니라 계속해서 list에 추가가 된다는 특징이 있습니다.

### LangChain add_messages 미사용

```python
from langchain_core.messages import AIMessage, HumanMessage

add_messages = []
message1 = [HumanMessage(context="Hello", id=1)]
message2 = [AIMessage(context="Hi", id=2)]

add_messages = message1 # [HumanMessage(context="Hello", id=1)]
add_messages = message2 # [AIMessage(context="Hi", id=2)]

```

add_messages에 기존 message1은 사라지고 message2로 덮어 씌워집니다.

### LangChain ad_messages 사용

```python
from langchain_core.messages import AIMessage, HumanMessage
from langgraph.graph.message import add_messages

message1 = [HumanMessage(context="Hello", id=1)]
message2 = [AIMessage(context="Hi", id=2)]

# [HumanMessage(context="Hello", id=1)] 
add_messages(message1) 

# [HumanMessage(context="Hello", id=1), AIMessage(context="Hi", id=2)]
add_messages(message2) 

```

LangGraph의 `add_messages`를 사용하면 계속해서 누적됩니다.


## 두 번째 특징

question, context, messages 3개로 정의가 되어있지만 노드에 상태를 전달할 때에는 이 세 개를 전부 채우지 않아도 된다는 특징이 있습니다.
