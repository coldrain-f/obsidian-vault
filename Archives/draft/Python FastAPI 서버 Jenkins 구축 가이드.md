
1. Python 설치
2. 가상환경 생성
	1. py -3.12 -m venv venv
3. 가상환경 활성화
	1. .\venv\Scripts\activate
4. pip 업그레이드
	1. python -m pip install --upgrade pip
5. 패키지 설치
	1. pip install -r packages.txt
6. FastAPI 실행
	1. uvicorn main:app --host 0.0.0.0 --port 9090


## 방화벽 설정

1. Windwos 방화벽
2. 고급 설정
3. 인바운드 규칙 설정
	1. 9090, TCP