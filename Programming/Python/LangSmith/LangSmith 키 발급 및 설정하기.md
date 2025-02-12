---
cssclasses:
  - img-grid
  - img-zoom
---
# 개요

LangSmith는 LangChain 생태계에서 LLM 애플리케이션의 개발, 테스트, 모니터링을 위한 강력한 도구입니다. 이 글에서는 LangSmith를 시작하는 데 필요한 기본적인 설정 방법을 알아보겠습니다.

- LangSmith 소개 및 필요성
- API 키 발급 방법
- 환경 변수 설정하기
- 실제 구현 및 확인

# LangSmith 소개

LLM 시스템 개발 시 파이프라인이 복잡해질수록 디버깅과 성능 최적화가 어려워집니다. LangSmith는 이러한 문제를 해결하기 위한 도구로, 개발 과정에서 발생하는 다양한 이슈를 추적하고 분석할 수 있게 해줍니다.

# API 키 발급받기

1. [LangSmith 웹사이트](http://smith.langchain.com)에 접속
2. 회원가입 및 로그인
3. 왼쪽 사이드바의 Settings(톱니바퀴 아이콘) 클릭
4. 우측 상단의 'Create API Key' 버튼을 통해 키 발급
5. 발급받은 키는 보안을 위해 안전하게 보관

# 환경 변수 설정하기

프로젝트 루트 디렉토리에 `.env` 파일을 생성하고 다음 내용을 입력합니다:

```env
LANGSMITH_TRACING=true
LANGSMITH_ENDPOINT="https://api.smith.langchain.com"
LANGSMITH_API_KEY="당신의-API-KEY"
LANGSMITH_PROJECT="프로젝트-이름"
```

*주의사항: LangSmith의 업데이트에 따라 환경 변수 설정이 변경될 수 있으므로, 공식 문서의 'Get Started' 섹션에서 최신 설정 방법을 확인하시기 바랍니다.*

# 동작 확인하기

다음과 같은 간단한 Python 코드로 LangSmith 설정이 제대로 되었는지 확인할 수 있습니다:

```python
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv

load_dotenv()

model = ChatOpenAI(model_name="gpt-4o-mini")
print(model.invoke("대한민국의 수도는?"))
```

코드를 실행하면 LangSmith 대시보드에서 설정한 프로젝트 이름으로 추적 정보가 기록되는 것을 확인할 수 있습니다. 이를 통해 모델의 응답, 실행 시간, 토큰 사용량 등 다양한 메트릭을 모니터링할 수 있습니다.

# 마무리

LangSmith는 LLM 애플리케이션 개발 과정에서 발생하는 다양한 문제를 효과적으로 추적하고 해결할 수 있게 해주는 도구입니다. 위의 설정을 완료하면 본격적인 LangSmith 사용을 시작할 수 있습니다.