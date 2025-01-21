# 1. Git 플러그인 설치하기

Obsidian에서 Git 플러그인을 설치한다.

2025년 1월 기준으로 다운로드 수가 가장 많은걸 설치하면 되는데, 개발자가 Vinzent, (Denis Olehov)로 되어있다.

Git 초기화
```bash
git init
```

Obsidian 폴더로 이동 후 git init을 해준다.

```bash
# Git 사용자 정보 설정
git config user.email "your.email@example.com"
git config user.name "Your Name"
```

사용자 정보를 설정해준다.

```bash
# 원격 저장소 연결
git remote add origin [repository URL]

# 현재 존재하는 모든 폴더를 추가
git add .
git commit -m "Initial commit"

# 브랜치 연결 및 초기 Push
git push --set-upstream origin master
# 또는 더 짧게:
git push -u origin master
```

저장소를 연결해 준다.