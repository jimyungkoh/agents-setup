# agents-setup

에이전트 전역·프로젝트 스코프 설정 저장소입니다.

| 스코프 | 경로 | 용도 |
| --- | --- | --- |
| 전역 | [`global-scope/`](global-scope/) | 모든 프로젝트에 적용할 플러그인·스킬 |
| 프로젝트 | [`project-scope/`](project-scope/) | 개별 프로젝트에 복사할 에이전트 가이드 |

## 전역 — `global-scope/`

```text
global-scope/
├── plugins/README.md          플러그인 설치 절차
└── skills/show-me/
    ├── SKILL.md               시각적 설명 스킬
    └── templates/page.html    독립형 HTML 시각화 템플릿
```

- [플러그인 설치 지침](global-scope/plugins/README.md)
- [show-me 스킬](global-scope/skills/show-me/SKILL.md)

## 프로젝트 — `project-scope/`

```text
project-scope/
├── AGENTS.md                  에이전트 동작, JIT 문서 안내
└── docs/commit-convention.md  커밋 형식·검증·실행 규칙
```

- [AGENTS.md](project-scope/AGENTS.md)
- [커밋 컨벤션](project-scope/docs/commit-convention.md) — 커밋 요청 시에만 로드

## 전역 플러그인 설치

에이전트에게 [플러그인 설치 지침](global-scope/plugins/README.md)을 전달하고 설치를 요청하세요.

```text
이 문서를 읽고 지침에 따라 전역 플러그인을 설치해 줘.
```

| 플러그인 | 역할 |
| --- | --- |
| [context-mode](https://github.com/mksglu/context-mode) | 대용량 도구 출력을 샌드박스에서 처리하고, 필요한 정보만 검색해 컨텍스트를 줄입니다. 세션 작업 기록도 보존합니다. |
| [graphify](https://github.com/Graphify-Labs/graphify) | 코드·문서를 지식 그래프로 바꿔 구성 요소 관계를 탐색하고 질의합니다. |
| [ponytail](https://github.com/dietrichgebert/ponytail) | 기존 코드·표준 라이브러리·플랫폼 기본 기능을 우선하게 해 불필요한 추상화와 의존성을 줄입니다. |
