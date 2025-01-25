## 경로(Path) 이해하기
### 절대 경로 vs 상대 경로

`os.path` 모듈은 파일 시스템 경로를 조작하는 다양한 함수를 제공합니다.

절대 경로는 루트 디렉토리부터 시작하는 전체 경로입니다:
```
# Windows
C:\Users\username\Documents\project\files

# Linux/Mac
/home/username/Documents/project/files
```

상대 경로는 현재 작업 디렉토리(`os.getcwd()`로 확인 가능) 기준의 경로입니다:
```python
# 현재 디렉토리 기준
"files/data.txt"
"./files/data.txt"  # ./ 는 현재 디렉토리를 명시
"../files/data.txt" # ../ 는 상위 디렉토리
```

> [!WARNING]
> - Windows는 백슬래시(`\`), Unix 계열은 슬래시(`/`) 사용
> - Python에서는 슬래시(`/`)를 사용해도 자동 변환됨
> - 경로 조작은 `os.path.join()` 사용 권장

## 경로 존재 확인
`os.path.exists()`는 파일이나 디렉토리의 존재 여부를 확인합니다.
`os.path.isfile()`과 `os.path.isdir()`로 파일과 디렉토리를 구분할 수 있습니다.

```python
import os

path = "files"
if os.path.exists(path):    # True/False 반환
    print(f"{path} 경로가 존재합니다")
    
    if os.path.isfile(path):    # 파일이면 True
        print("파일입니다")
    elif os.path.isdir(path):    # 디렉토리면 True
        print("디렉토리입니다")
```

## 폴더 생성
### mkdir vs makedirs
`os.mkdir()`은 단일 디렉토리만, `os.makedirs()`는 중간 경로까지 모두 생성합니다.

```python
# mkdir: 부모 폴더 없으면 에러
os.mkdir('parent/child')     

# makedirs: 부모 폴더도 함께 생성
os.makedirs('parent/child')  
```

### exist_ok 옵션 활용
```python
# 안전한 폴더 생성 방법
os.makedirs(path, exist_ok=True)

# 1. exist_ok=True
# - 폴더 존재시 무시
# - 기존 내용 보존
# - 에러 없음

# 2. exist_ok=False (기본값)
# - 폴더 존재시 FileExistsError
```

## 경로 결합
`os.path.join()`은 여러 경로를 OS에 맞는 구분자로 결합합니다.

```python
import os
from typing import List

def save_images() -> List[str]:
    paths = []
    for i in range(3):
        # OS에 맞는 구분자로 자동 변환
        path = os.path.join("images", f"img_{i}.png")
        paths.append(path)
        
        # image 저장 로직 생략
        ...
        
    return paths

# Windows: images\img_0.png
# Linux/Mac: images/img_0.png
```

## 디렉토리 탐색
### 기본 사용
`os.listdir()`은 지정된 경로의 하위 파일과 폴더명을 리스트로 반환합니다.

```python
# 둘 다 동일한 결과
files1 = os.listdir('files')    # 끝에 / 없음
files2 = os.listdir('files/')   # 끝에 / 있음
```

### 경로 시작 표현
```python
os.listdir('files')     # 현재 디렉토리 /files 내용 반환
os.listdir('./files')   # 동일: 현재 디렉토리 /files 내용 반환
```

> [!WARNING] 
> - `listdir()`은 직계 항목만 반환(=바로 아래 항목만 반환)
> - 마침표(`./`)는 생략 가능
> - 끝의 슬래시(`/`)는 선택사항

### 예시 구조
```
project/
├── images/
│   ├── img_1.png
│   └── img_2.png
├── data/
│   └── user/
│       └── info.txt
└── main.py

# 예시 코드
os.listdir('images')  # ['img_1.png', 'img_2.png']
os.listdir('data')    # ['user']
```

## 유용한 경로 함수들
```python
# 현재 작업 디렉토리 반환
current = os.getcwd()

# 상대 경로를 절대 경로로 변환
abs_path = os.path.abspath('files')

# 경로에서 디렉토리 경로와 파일명 분리
dir_path = os.path.dirname('/path/to/file.txt')  # /path/to
file_name = os.path.basename('/path/to/file.txt')  # file.txt
```