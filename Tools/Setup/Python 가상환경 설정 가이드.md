## 가상환경 생성

Windows 환경에서는 PowerShell 대신 CMD(Command Prompt)를 사용하는 것을 권장합니다. PowerShell에서는 권한 문제로 가상환경 생성이 실패할 수 있습니다.

### 1. 가상환경 생성하기

특정 Python 버전으로 가상환경을 생성합니다:

```bash
py -3.12 -m venv venv
```

### 2. 생성 확인

다음 명령어로 가상환경이 정상적으로 생성되었는지 확인할 수 있습니다:

```bash
dir venv
```

정상적으로 생성된 경우 다음과 같은 디렉토리 구조가 보입니다:

```
venv/
├── Include/
├── Lib/
├── Scripts/
└── pyvenv.cfg
```

## 가상환경 활성화 및 관리

### 1. 활성화

```bash
.\venv\Scripts\activate
```

### 2. 활성화 상태 확인

```bash
echo %VIRTUAL_ENV%
```

활성화된 경우 가상환경 경로가 출력됩니다.
예시: `C:\Users\user\Desktop\langchain-chatbot\server\venv`

### 3. pip 업그레이드

새로 생성된 가상환경의 pip을 최신 버전으로 업그레이드합니다. 이는 패키지 설치 시 발생할 수 있는 오류를 방지합니다.

```bash
python -m pip install --upgrade pip
```

## 패키지 관리

### 1. 개별 패키지 설치
```bash
pip install [패키지이름]
```

### 2. 패키지 일괄 설치

requirements.txt 파일이 있는 경우, 다음 명령어로 필요한 모든 패키지를 한 번에 설치할 수 있습니다:

```bash
pip install -r requirements.txt
```

## 가상환경 비활성화

작업이 완료된 후 가상환경을 비활성화합니다:

```bash
deactivate
```