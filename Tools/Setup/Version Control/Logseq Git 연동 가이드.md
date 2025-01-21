# 1. Git 플러그인 설치하기

Logseq 애플리케이션에서 플러그인 마켓플레이스를 열고 Git 플러그인을 검색하여 설치합니다.

# 2. Git 초기 설정하기

1. Logseq 그래프 폴더로 이동합니다.
2. Git Bash를 실행하여 사용자 정보를 설정합니다:

```bash
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

# 브랜치 연결 및 초기 Push
git push --set-upstream origin master
# 또는 더 짧게:
git push -u origin master
```

## 다른 기기에서 설정 시

1. GitHub에서 기존 저장소의 HTTPS 주소를 복사합니다.
2. Logseq 그래프를 저장할 새 폴더를 생성합니다.
3. Git Bash에서 다음 명령어를 실행합니다:

```bash
# 저장소 복제
git clone [repository URL] .
```

# 4. 완료

위 과정을 모두 마치면 Logseq에서 작성하는 모든 마크다운 문서가 자동으로 Git으로 버전 관리됩니다. 이후 변경사항은 Logseq의 Git 플러그인을 통해 쉽게 커밋하고 푸시할 수 있습니다.

참고: 다른 기기에서는 저장소를 새로 만들 필요 없이, 기존 저장소를 복제(Clone)하여 사용하면 됩니다.