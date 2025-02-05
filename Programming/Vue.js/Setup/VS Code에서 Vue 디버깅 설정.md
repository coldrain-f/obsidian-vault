### 1. VS Code에서 Vue.js 프로젝트 열기[](https://langchain-tutorial.streamlit.app/~/+/basic_llm_multi_turn#506c13c2)

- VS Code에서 Vue.js 프로젝트를 엽니다.

### 2. 디버깅 설정 파일 생성

- VS Code의 왼쪽 사이드바에서 "Run and Debug" 아이콘(재생 버튼과 벌레 아이콘)을 클릭합니다.
- "create a launch.json file"을 클릭하여 디버깅 설정 파일을 생성합니다.
- "Chrome"을 선택하여 Chrome 브라우저에서 디버깅할 수 있도록 설정합니다.

### 3. `launch.json` 파일 수정

- 생성된 `launch.json` 파일에서 다음과 같은 설정을 추가하거나 수정합니다:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:8080", // Vue 애플리케이션의 URL
      "webRoot": "${workspaceFolder}", // 소스 코드 경로
      "sourceMaps": true // 소스 맵 사용
    }
  ]
}
```

### 4. Vue 애플리케이션 실행

- 터미널에서 `npm run serve` 명령어를 사용하여 Vue 애플리케이션을 실행합니다. 기본적으로 `http://localhost:8080`에서 실행됩니다. 프로젝트에 맞게 포트 번호를 변경해 주세요.

### 5. 브레이크포인트 설정

- VS Code의 소스 코드 파일에서 디버깅하고 싶은 줄 번호를 클릭하여 브레이크포인트를 설정합니다. 빨간 점이 나타나면 브레이크포인트가 설정된 것입니다.

### 6. 디버깅 시작

- "Run and Debug" 사이드바에서 "Launch Chrome against localhost"를 선택하고, 녹색 재생 버튼을 클릭하여 디버깅을 시작합니다.
- Chrome 브라우저가 열리고, 설정한 URL로 이동합니다. 브레이크포인트에 도달하면 코드 실행이 멈추고, VS Code에서 변수 값, 호출 스택 등을 확인할 수 있습니다.

### 7. 디버깅 도구 사용

- VS Code의 디버깅 도구를 사용하여 코드 실행을 단계별로 진행하거나, 변수 값을 확인하고, 호출 스택을 탐색할 수 있습니다.

이러한 단계를 통해 VS Code에서 Vue.js 애플리케이션의 브레이크포인트를 설정하고 디버깅할 수 있습니다. 디버깅 과정에서 발생하는 문제를 해결하고, 애플리케이션의 동작을 더 잘 이해하는 데 도움이 될 것입니다.