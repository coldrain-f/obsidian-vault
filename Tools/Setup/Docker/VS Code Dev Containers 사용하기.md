# 개요

Dev Containers는 개발 환경을 일관되게 유지하고 팀원들과 동일한 환경에서 작업할 수 있게 해주는 강력한 도구입니다. 이 글에서는 VS Code에서 Dev Containers를 설정하고 사용하는 방법을 상세히 알아보겠습니다.

- Docker Desktop 설치 방법
- VS Code Dev Containers 확장 프로그램 설치
- Python 개발 환경 컨테이너 생성 및 설정
- 컨테이너 관리 및 재사용 방법
- 팀 프로젝트에서의 활용 방안

# Docker Desktop 설치

개발 환경 구축의 첫 단계로 [Docker Desktop](https://www.docker.com/products/docker-desktop/)을 설치해야 합니다. Docker Desktop은 컨테이너를 관리하고 실행하는 데 필요한 핵심 도구입니다.

# Dev Containers 설치하기

VS Code에서 Dev Containers를 사용하기 위한 확장 프로그램 설치 과정입니다:

1. VS Code를 실행합니다
2. 확장 프로그램 탭으로 이동합니다
3. Dev Containers를 검색하여 설치합니다
4. 설치 완료 후 VS Code를 재시작합니다

# 컨테이너 생성하기

Python 개발 환경을 위한 컨테이너 설정 방법입니다:

1. VS Code에서 F1키를 눌러 명령 팔레트를 엽니다
2. `Dev Containers: Add Dev Container Configuration Files...`를 검색하여 선택합니다
3. `Add configuration to workspace` 옵션을 선택합니다
4. Python 3 템플릿을 선택합니다
5. Python 3.11-bookworm 버전을 선택합니다
6. 필요한 추가 기능을 선택하거나 기본 설정으로 진행합니다

이 과정을 통해 `.devcontainer` 폴더와 다음과 같은 `devcontainer.json` 설정 파일이 생성됩니다:

```json
{
    "name": "Python 3",
    "image": "mcr.microsoft.com/devcontainers/python:1-3.11-bookworm"
    // 추가 설정 옵션들이 주석 처리되어 있습니다
}
```

# 컨테이너 빌드 및 실행

컨테이너를 처음 실행하는 방법입니다:

1. F1키를 눌러 명령 팔레트를 엽니다
2. `reopen in container`를 입력하고 선택합니다
   - 첫 빌드는 이미지 다운로드로 인해 시간이 소요될 수 있습니다
3. VS Code 왼쪽 하단에 `Dev Container: Python 3` 표시를 확인합니다
4. 터미널에서 `python --version` 명령어로 설정한 버전이 정상적으로 설치되었는지 확인합니다
5. Docker Desktop에서 생성된 이미지와 컨테이너를 확인합니다

# 컨테이너 관리

### 재빌드하기
설정 변경 사항을 적용하려면 다음 과정으로 컨테이너를 재빌드합니다:

1. F1키로 명령 팔레트를 엽니다
2. `Dev Containers: Rebuild Container Without Cache`를 실행합니다

### 컨테이너 종료하기
작업을 마치고 컨테이너를 종료하는 방법입니다:

1. F1키로 명령 팔레트를 엽니다
2. `Close Remote Connection`을 선택합니다

# 팀 프로젝트에서 활용하기

다른 팀원들과 동일한 개발 환경을 공유하는 방법입니다:

1. `.devcontainer` 폴더와 설정 파일을 프로젝트에 포함시킵니다
2. 다른 팀원들은 VS Code의 Remote Explorer에서 해당 프로젝트 폴더를 열기만 하면 됩니다
3. `Open Folder in Container` 버튼을 클릭하면 자동으로 동일한 환경이 구성됩니다

이러한 방식으로 모든 팀원이 일관된 개발 환경에서 작업할 수 있으며, "내 컴퓨터에서는 작동합니다"와 같은 환경 차이로 인한 문제를 방지할 수 있습니다.