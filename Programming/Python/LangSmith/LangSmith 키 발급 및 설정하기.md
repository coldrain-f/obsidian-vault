---
cssclasses:
  - img-grid
  - img-zoom
---
# 키 발급받기

LangChain을 이용하여 LLM 시스템을 개발하다보면 계속해서 파이프라인이 복잡해지는 경우가 생깁니다. 복잡도가 올라가면 올라갈 수록 어떤 부분에서 문제가 생기는지 추적하기가 어려워 지는데 LangSmith를 활용하면 추적을 쉽게 할 수 있습니다.

[LangSmith](http://smith.langchain.com)에 들어가서 회원가입 후 로그인을 합니다.

왼쪽 사이드바에 톱니바퀴 아이콘(Settings)에 진입 후 우측 상단에 있는 Create API Key 버튼을 눌러서 키를 발급 후 외부로 유출되지 않도록 잘 보관해 주세요.

# 환경변수 설정

```.env
LANGSMITH_TRACING=true
LANGSMITH_ENDPOINT="https://api.smith.langchain.com"
LANGSMITH_API_KEY="당신의-API-KEY"
LANGSMITH_PROJECT="프로젝트-이름"
```

이후 자신의 프로젝트에 `.env`파일을 생성 후 위와 같이 복사해 줍니다.
`LANGSMITH_API_KEY` 부분에 발급 받은 LangSmith의 API 키를 입력 후 원하는 프로젝트 이름을 적어주시면 기본 설정이 완료됩니다.

`.env`파일에 들어가는 내용은 LangSmith가 업데이트되면서 바뀔 수 있으니 공식 사이트의 Get Started에서 Set up tracing 부분을 확인해 보시는 것을 권장드립니다.

# 동작 확인

```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

load_dotenv()

model = ChatOpenAI(model_name="gpt-4o-mini")
print(model.invoke("대한민국의 수도는?"))

```

LangChain으로 기본적인 코드를 작성해 주고 실행하면 추적이 시작됩니다.

![이미지1](assets/images/langsmith/01.png)

LangSmith 대시보드에 들어가서 확인해 보면 환경설정 변수에 설정한 프로젝트 이름으로 추적되는 것을 확인해볼 수 있습니다.

![이미지2](assets/images/langsmith/02.png)