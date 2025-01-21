# 1. Git 플러그인 설치하기

Obsidian 애플리케이션에서 설정(Settings) > 커뮤니티 플러그인(Community plugins)으로 이동하여 Git 플러그인을 검색하고 설치합니다. 2025년 1월 기준으로 다운로드 수가 가장 많은 개발자 Vinzent(Denis Olehov)의 플러그인을 설치하시면 됩니다.

# 2. Git 초기 설정하기

1. Obsidian 볼트 폴더로 이동합니다.
2. Git Bash를 실행하여 다음 명령어들을 순서대로 입력합니다:

```bash
# Git 저장소 초기화
git init

# Git 사용자 정보 설정
git config user.email "your.email@example.com"
git config user.name "Your Name"
```

# 3. GitHub 저장소 연결하기

## 최초 설정 시 (첫 번째 컴퓨터)

1. GitHub에서 새로운 저장소를 생성합니다.
2. 생성된 저장소의 HTTPS 주소를 복사합니다.
3. Git Bash에서 다음 명령어를 실행합니다:

```bash
# 원격 저장소 연결
git remote add origin [repository URL]

# 현재 존재하는 모든 파일 추가
git add .
git commit -m "Initial commit"

# 브랜치 연결 및 초기 Push
git push --set-upstream origin master
# 또는 더 짧게:
git push -u origin master
```

## 다른 기기에서 설정 시

1. GitHub에서 기존 저장소의 HTTPS 주소를 복사합니다.
2. Obsidian 볼트를 저장할 새 폴더를 생성합니다.
3. Git Bash에서 다음 명령어를 실행합니다:

```bash
# 저장소 복제
git clone [repository URL] .
```

# 4. 완료

위 과정을 모두 마치면 Obsidian Git 플러그인을 통해 자동으로 변경사항이 추적되고 버전 관리됩니다. 플러그인 설정에서 자동 백업 주기와 커밋 메시지 형식 등을 설정할 수 있으며, Command palette(Ctrl/Cmd + P)를 통해 수동으로 커밋과 푸시를 수행할 수도 있습니다.

참고: 다른 기기에서는 저장소를 새로 만들 필요 없이, 기존 저장소를 복제(Clone)하여 사용하면 됩니다.