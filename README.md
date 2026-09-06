# agents-setup

에이전트 전역·프로젝트 스코프 설정 저장소입니다.

## 전역 플러그인 설치

에이전트에게 [플러그인 설치 지침](global-scope/plugins/README.md)을 전달하고 설치를 요청하세요.

```text
이 문서를 읽고 지침에 따라 전역 플러그인을 설치해 줘.
```

| 플러그인 | 역할 |
| --- | --- |
| [context-mode](https://github.com/mksglu/context-mode) | 대용량 도구 출력을 샌드박스에서 처리하고 필요한 정보만 검색해 컨텍스트 사용량을 줄이며, 세션 작업 기록을 보존합니다. |
| [graphify](https://github.com/Graphify-Labs/graphify) | 코드와 문서 등 프로젝트 자료를 지식 그래프로 변환해 구성 요소 간 관계를 탐색하고 질의할 수 있게 합니다. |
| [ponytail](https://github.com/dietrichgebert/ponytail) | 기존 코드·표준 라이브러리·플랫폼 기본 기능을 우선하도록 에이전트를 유도해 불필요한 추상화와 의존성을 줄이는 에이전트 플러그인입니다. |
