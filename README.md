# AI Productivity Assistant

> **AI-powered task prioritization assistant**  
> 해야 할 일이 많을 때 **무엇부터 해야 하는지** 판단할 수 있도록 돕는 AI 생산성 도우미

**Lipcoding 2026 · Challenge Project**  
`JavaScript` `GitHub Models API` `AI` `localStorage` `Azure Static Web Apps` `UX`

---

## Overview | 프로젝트 소개

When a task list becomes long, the real problem is often not recording tasks but deciding **what matters now**. This project analyzes tasks with AI and returns priority, estimated time, category, and reasoning.

할 일이 많아질수록 단순히 기록하는 것보다 **지금 무엇을 먼저 해야 하는지 결정하는 것**이 더 어려워집니다. 이 프로젝트는 AI가 할 일을 분석하여 우선순위, 예상 소요 시간, 분류와 판단 이유를 제공하도록 구현한 웹앱입니다.

기술 구현뿐 아니라 서비스·마케팅 경험에서 쌓은 **사용자 문제 정의와 서비스 관점**을 함께 적용했습니다. 사용자의 의사결정 부담을 줄이고, API 토큰이 없는 심사 환경에서도 핵심 경험을 확인할 수 있도록 전체 흐름을 설계했습니다.

## Target Users | 타겟 사용자

- Students and professionals managing multiple tasks | 여러 할 일을 동시에 관리하는 학생·직장인
- Early adopters interested in AI-assisted productivity | AI 기반 생산성 도구에 관심 있는 사용자
- Users comfortable using a GitHub PAT for live analysis | 실제 AI 분석을 위해 GitHub PAT를 사용할 수 있는 사용자

## Key Features | 주요 기능

- Task creation, completion, and deletion | 할 일 추가·완료·삭제
- AI priority and time estimation | AI 기반 우선순위·예상 시간 분석
- Voice input for tasks | 할 일 음성 입력
- Demo analysis without credentials | 토큰 없이 체험 가능한 데모 분석
- Persistent state with `localStorage` | 새로고침 후에도 상태 유지
- GitHub token save/delete controls | 토큰 저장·삭제
- In-app validation feedback | 단계별 검증 상태 표시
- Full local-data reset | 로컬 데이터 전체 초기화

## Product & UX Decisions | 서비스·UX 설계

### 1. User-centered flow | 사용자 중심 흐름

The interface starts from one simple user question: **“What should I do first?”** Instead of exposing raw model output, the result is organized into actionable cards.

인터페이스는 **“그래서 지금 무엇부터 해야 하지?”**라는 사용자의 실제 질문에서 출발했습니다. AI 응답 원문을 그대로 노출하기보다 바로 행동으로 옮길 수 있도록 결과를 카드 형태로 구조화했습니다.

### 2. Demo-first evaluation | 데모 우선 설계

Reviewers may not have an API token. A separate demo-analysis path therefore allows the full product flow to be experienced without credentials.

심사자나 리뷰어가 API 토큰을 준비하지 못한 상황에서도 서비스의 핵심 흐름을 확인할 수 있도록 **토큰 없는 데모 분석 경로**를 별도로 제공했습니다.

### 3. Privacy-aware data handling | 개인정보와 데이터 처리

Tasks remain in the user's browser through `localStorage`. The reset action is placed in the privacy/data-management context so users can understand what is being deleted.

할 일 데이터는 별도 DB가 아니라 사용자 브라우저의 `localStorage`에 저장합니다. 전체 초기화 기능도 개인정보·로컬 데이터 영역에 배치하여 **사용자가 어떤 데이터가 삭제되는지 맥락을 이해하도록 UX를 설계**했습니다.

## Architecture | 구조

```text
ai-productivity-assistant/
├── index.html
├── staticwebapp.config.json
├── README.md
├── PRD.md
└── AGENTS.md
```

The project intentionally uses a lightweight static architecture. AI requests use GitHub Models API with `gpt-4o-mini`, and deployment is handled through Azure Static Web Apps.

빠르게 검증 가능한 프로토타입을 목표로 정적 웹 구조를 사용했습니다. AI 분석은 GitHub Models API의 `gpt-4o-mini`를 활용하고 Azure Static Web Apps로 배포했습니다.

## AI & Token Handling | AI·토큰 처리

- Tokens are never hardcoded in the repository. | 토큰을 저장소 코드에 하드코딩하지 않습니다.
- Live analysis runs only with a user-supplied token. | 실제 분석은 사용자가 직접 입력한 토큰으로만 실행됩니다.
- Demo mode works without credentials. | 인증 정보 없이도 데모 흐름을 확인할 수 있습니다.
- A production service should protect shared credentials behind a server-side proxy such as Azure Functions. | 실제 서비스에서는 Azure Functions 등의 서버사이드 프록시를 통해 공용 비밀키를 보호하는 구조가 필요합니다.

## Validation | 검증 흐름

1. Token save/delete | 토큰 저장·삭제
2. Task creation | 할 일 추가
3. Complete/delete task | 완료·삭제
4. Voice input | 음성 입력
5. Refresh and persistence check | 새로고침 후 상태 유지 확인
6. Demo/live AI analysis | 데모·실제 AI 분석
7. Result-card validation | 우선순위·시간·분류·이유 출력 확인
8. Analysis restoration after refresh | 분석 결과 복원 확인

## Live Demo | 배포

**Azure Static Web Apps:** https://red-smoke-003835800.7.azurestaticapps.net/

## What I Learned | 배운 점

This project reinforced that building a useful product is not only about implementing features. It requires identifying the user's actual decision problem and balancing UX, privacy, security, deployment simplicity, and technical constraints.

이 프로젝트를 통해 기능 구현 자체보다 **사용자의 실제 문제를 어떻게 정의하고 서비스 경험으로 변환할 것인가**가 중요하다는 점을 다시 확인했습니다. 동시에 빠른 프로토타입을 만들면서 UX, 개인정보, 보안, 배포 편의성과 기술적 제약 사이의 균형을 고민했습니다.

특히 기존 **Service & Marketing** 경험을 개발 과정에 연결하여, 사용자 관점에서 문제를 정의하고 이를 실제 동작하는 제품으로 구현하는 연습을 한 프로젝트입니다.

---

Built for **Lipcoding 2026** · `Challenges & Hackathons`