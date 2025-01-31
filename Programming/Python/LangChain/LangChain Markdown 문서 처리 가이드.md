# 개요
LangChain을 사용하여 Markdown 문서를 처리하는 방법을 설명하는 가이드입니다. 본 문서는 다음과 같은 주요 내용을 다룹니다:

1. 환경 설정
   - Python 가상환경 설정 (Windows 환경 기준)
   - LangChain 핵심 패키지 및 Markdown 처리용 추가 패키지 설치

2. 기본 기능
   - UnstructuredMarkdownLoader를 사용한 기본적인 마크다운 문서 로딩
   - Document 객체의 구조와 주요 속성 (page_content, metadata)

3. 처리 모드
   - Single Mode: 전체 문서를 하나의 Document로 처리
   - Elements Mode: 문서를 의미 단위로 분리하여 구조적 분석 수행

이 가이드를 통해 사용자는 LangChain을 사용하여 Markdown 문서를 쉽게 로드하고 처리하는 방법을 학습할 수 있습니다. 특히 Elements Mode를 활용한 고급 문서 분석 기능을 제공하여 목차 생성, 특정 섹션 추출 등 다양한 문서 처리 작업을 수행할 수 있습니다.

# Python 가상환경 설정
가상환경 분리는 프로젝트별 의존성 충돌을 방지하고 깔끔한 개발환경을 위해 필수적입니다. [[Programming/Python/Setup/Python 가상환경 설정 가이드|Python 가상환경 설정 가이드]]를 참고해 설정하세요. 가상환경 설정이 처음이시라면 가이드의 [[Python 가상환경 설정 가이드#1. 가상환경 생성|1. 가상환경 생성]]부터 [[Python 가상환경 설정 가이드#2. 가상환경 활성화 및 관리|2. 가상환경 활성화 및 관리]]까지만 진행해도 충분합니다.


> [!WARNING]
> - 본 문서는 Windows 환경에서 Python venv를 사용한 가상환경 설정을 기준으로 작성되었습니다
> - Linux/Mac에서는 virtualenv나 conda 사용을 권장하며, 패키지 설치 명령어가 다를 수 있습니다
> - 특히 nltk-data 설치 과정이 OS별로 상이할 수 있으니 주의하세요

# 필수 패키지 설치
## Langchain 기본 패키지 
아래는 LangChain 사용을 위한 핵심 패키지들입니다:
```txt
langchain        
faiss-cpu       
openai            
langchain-openai 
langchain-community 
```
모든 패키지를 `packages.txt`에 기록하고 일괄 설치하면 편리합니다. 자세한 방법은 [[Programming/Python/Setup/Python 가상환경 설정 가이드#패키지 일괄 설치|패키지 일괄 설치 가이드]]를 참고하세요.

## Markdown Loader 추가 패키지
Markdown 문서 처리를 위한 패키지입니다:
```bash
pip install "unstructured[md]"
```

# 기본 실습
가장 기본적인 마크다운 로더 사용법입니다:
```python
from langchain_community.document_loaders import UnstructuredMarkdownLoader  
from langchain_core.documents import Document
import os

markdown_path = "./files/SAMPLE.md"

if os.path.exists(markdown_path):
    loader = UnstructuredMarkdownLoader(markdown_path)
    data = loader.load()
    print(data[0].page_content)  # 전체 문서 내용 출력
```

> [!WARNING]
> - NLTK 관련 에러(`LookupError`)가 발생하면 `pip install nltk-data`로 필요한 언어 데이터를 설치하세요
> - 이는 공식 문서에 명시되지 않은 요구사항입니다 (2025-01-26 기준)

## Document 구조
Langchain의 Document 객체는 문서 처리의 기본 단위입니다:
- `loader.load()`는 Document 객체들의 리스트를 반환
- 각 Document는 두 가지 주요 속성을 가짐:
  - `page_content`: 실제 문서 내용을 담는 문자열
  - `metadata`: 파일 경로, 생성일 등 문서 관련 정보
- 이 구조는 PDF, CSV 등 다른 형식의 문서에도 동일하게 적용됨

# Mode 설정

## Single Mode (기본값)
전체 문서를 하나의 Document로 처리하는 기본 모드입니다:
```python
loader = UnstructuredMarkdownLoader(markdown_path, mode="single")
data = loader.load()

print(len(data))  # 항상 1
print(data[0].metadata)  # 기본 메타데이터만 포함
```

## Elements Mode
문서를 의미 단위로 분리해 처리하는 고급 모드입니다:
```python
loader = UnstructuredMarkdownLoader(markdown_path, mode="elements")
data = loader.load()

print(len(data))  # 문서 구조에 따라 여러 개
print(data[0].metadata)  # 상세한 메타데이터 포함
```

### Elements Mode 활용 예시
문서의 특정 부분만 추출하고 싶을 때 유용합니다:
```python
loader = UnstructuredMarkdownLoader(markdown_path, mode="elements")
documents = loader.load()

# 제목만 추출하는 예시
for doc in documents:
    category = doc.metadata.get("category")
    if category == "Title":
        print(doc.page_content)
```

Elements Mode는 문서를 제목, 본문, 코드 블록 등으로 자동 분리하여 구조적 분석이 필요할 때 특히 유용합니다. 예를 들어 목차 생성, 특정 섹션 추출, 문서 구조 분석 등의 작업에 활용할 수 있습니다.