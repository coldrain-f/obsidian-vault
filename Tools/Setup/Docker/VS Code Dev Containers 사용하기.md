# Docker Desktop 설치

[Docker Desktop](https://www.docker.com/products/docker-desktop/) 설치

# Dev Containers 설치

1. VS Code 실행
2. Extensions 진입 
3. Dev Containers Install
4. VS Code 재실행

# Step 1 컨테이너 생성하기

1. VS Code에서 F1(Show Command Palette)키를 클릭
2. `Dev Containers: Add Dev Container Configuration Files...` 검색 후 선택
3. `Add configuration to workspace` 선택
4. 원하는 템플릿 선택(여기서는 Python 3)
5. 원하는 Python 버전 선택(여기서는 3.11-bookworm)
6. 원하는 추가 기능 선택(여기서는 그냥 OK 버튼 클릭 후 스킵)

7. `.devcontainer` 폴더와 `devcontainer.json` 파일 생성 확인

```json
{
	"name": "Python 3",
	"image": "mcr.microsoft.com/devcontainers/python:1-3.11-bookworm"
	// "features": {},
	// "forwardPorts": [],
	// "postCreateCommand": "pip3 install --user -r requirements.txt",
	// "customizations": {},
	// "remoteUser": "root"
}

```

# Step 2 컨테이너 초기 빌드하기

1. VS Code에서 F1(Show Command Palette)키를 클릭
2. `reopen in container` 입력 후 선택
	-  컨테이너가 빌드되며 초기 빌드는 시간이 조금 걸릴 수 있습니다.


![[Pasted image 20250219194604.png]]

3. VS Code 왼쪽 하단에 `Dev Container: Python 3`가 표시되는지 확인
4. 터미널 창에서 `python --version`을 입력해서 설정한 Python 버전인지 확인
5. Docker Desktop에서 이미지와 컨테이너가 표시되는지 확인

# Step 3 컨테이너 재빌드

`.devcontainer`폴더의 내용이 변경 사항이 생겨 반영해야 한다면 재빌드를 하면 됩니다.

1. VS Code에서 F1(Show Command Palette)키를 클릭
2. `Dev Containers: Rebuild Container Without Cache` 검색 후 클릭


# Dev Container 종료

1. VS Code에서 F1(Show Command Palette)키를 클릭
2. `Close Remote Connection` 검색 후 선택

# 다른 컴퓨터에서 사용 시

컨테이너를 생성하고 빌드까지 진행이 끝났다면, 이후부터는 VS Code에서 Remote Explorer에 진입하여 `.devcontainer` 폴더와 `devcontainer.json` 파일이 있는 작업 공간을 `Open Folder in Container`버튼으로 열어주기만 하면 설정 정보를 읽어서 이미지와 컨테이너가 자동으로 생성되기 때문에 동일한 컨테이너 환경에서 개발을 할 수 있습니다.

