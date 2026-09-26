# AI Productivity Assistant

> **Security-first productivity assistant evolved from an AI-powered prototype**  
> AI 프로토타입에서 출발해, 공개 배포에서는 사용자 credential을 취급하지 않도록 재설계한 생산성 도우미

**Lipcoding 2026 · Challenge Project**  
`JavaScript` `Product Design` `Security` `Privacy` `GitHub Models (prototype)`

---

## Overview | 프로젝트 소개

When a task list becomes long, the difficult part is often not recording tasks but deciding **what should be done first**. The original prototype used GitHub Models to analyze tasks with AI. During preparation for public deployment, I reviewed the credential flow and decided that a static client-side portfolio should not ask visitors to provide a GitHub Personal Access Token (PAT).

할 일이 많아질수록 기록 자체보다 **무엇부터 해야 하는지 결정하는 것**이 더 어려워집니다. 초기 프로토타입에서는 GitHub Models API를 이용해 AI가 우선순위, 예상 시간, 분류와 이유를 분석하도록 구현했습니다. 그러나 공개 배포를 준비하면서 인증 정보의 흐름을 다시 검토했고, 정적 클라이언트 포트폴리오가 방문자의 GitHub PAT를 직접 요구하고 처리하는 구조는 적절하지 않다고 판단했습니다.

현재 공개판은 **credential-free, rule-based edition**입니다. 중요도·긴급도·예상 시간을 사용해 브라우저 안에서 명시적인 규칙으로 우선순위를 계산하며, 방문자의 토큰이나 API 키를 요구하지 않습니다.

## Product Intent | 서비스 의도

The product begins with one user question: **“What should I do first?”** My Service & Marketing background influenced the project to focus on the user's decision problem rather than exposing technology for its own sake.

서비스의 출발점은 **“그래서 지금 무엇부터 해야 하지?”**라는 사용자의 실제 질문입니다. Service & Marketing 도메인 경험을 바탕으로 기술 자체를 보여주는 것보다 사용자의 의사결정 부담을 줄이는 문제 정의와 흐름 설계에 집중했습니다.

## Public Features | 공개판 기능

- Add tasks | 할 일 추가
- Set importance and urgency | 중요도·긴급도 설정
- Add estimated completion time | 예상 소요 시간 설정
- Transparent priority scoring | 명시적인 규칙 기반 우선순위 계산
- Automatic priority sorting | 우선순위 자동 정렬
- Full in-memory reset | 현재 페이지 데이터 전체 초기화
- No credential input | 토큰·API 키 입력 없음
- No external AI/API request | 외부 AI/API 호출 없음
- Responsive layout | 데스크톱·태블릿·모바일 대응
- Light/Dark theme | 시스템 설정 연동 및 사용자 직접 전환

> **Important:** The public result is not presented as AI output. It is explicitly labeled as rule-based prioritization.
>
> **중요:** 공개판의 결과를 AI 분석이라고 표시하지 않습니다. 명시적인 규칙 기반 우선순위 계산임을 화면에서 분명하게 안내합니다.

---

## Security & Privacy | 보안·개인정보

Security was treated as a deployment requirement, not an afterthought.

보안은 배포 후 추가하는 옵션이 아니라 **공개 여부를 결정하는 핵심 요구사항**으로 다뤘습니다.

### Data storage & retention | 데이터 저장 및 보관

현재 공개판에서 데이터는 종류에 따라 다음과 같이 처리됩니다.

| Data | Where it exists | Retention | External transmission |
|---|---|---|---|
| 할 일 내용 | 현재 브라우저 탭의 JavaScript 메모리 | 새로고침·탭 종료 시 사라짐. `전체 초기화`로 즉시 삭제 가능 | 없음 |
| 중요도·긴급도·예상 시간 | 현재 브라우저 탭의 JavaScript 메모리 | 할 일과 동일 | 없음 |
| 계산된 우선순위 | 브라우저에서 즉시 계산 | 현재 페이지 세션 동안만 존재 | 없음 |
| Light/Dark 테마 선택 | 해당 브라우저의 `localStorage` (`pa-theme`) | 앱이 별도 만료기간을 설정하지 않음. 사용자가 테마를 다시 선택하거나 사이트 데이터를 삭제할 때 변경·삭제됨 | 없음 |
| PAT / API key | 수집·입력하지 않음 | 저장되지 않음 | 없음 |

즉, **할 일 데이터는 `localStorage`, `sessionStorage`, cookie 또는 서버 데이터베이스에 저장하지 않습니다.** 현재 탭의 메모리에서만 처리되므로 페이지를 새로고침하거나 탭을 닫으면 복구할 수 없습니다. 반면 테마 설정만 사용 편의를 위해 `localStorage`에 저장하며, 이 값은 `light` 또는 `dark`뿐입니다.

브라우저 자체의 캐시, 방문 기록, 호스팅 인프라가 플랫폼 운영 목적으로 처리할 수 있는 네트워크·접속 정보는 이 애플리케이션의 JavaScript 데이터 저장 기능과 별개의 영역입니다. 이 프로젝트는 방문자의 할 일 데이터를 자체 서버로 수집하거나 보관하는 백엔드를 운영하지 않습니다.

### Security review process | 보안 검토 과정

공개 배포 전 다음 순서로 구조를 재검토했습니다.

1. **Credential inventory** — 어떤 인증 정보가 필요한지 확인했습니다. 초기 버전은 사용자의 GitHub PAT를 사용했습니다.
2. **Data-flow review** — PAT가 브라우저에서 입력되고 JavaScript가 읽어 GitHub Models API의 Authorization header로 전달되는 흐름을 확인했습니다.
3. **Persistence review** — 초기 구현에서 PAT가 `localStorage`에 저장되는 구조를 확인했습니다.
4. **Threat review** — 공개 정적 웹앱에서 브라우저가 credential을 직접 취급할 경우 XSS, 악성 스크립트, 브라우저 확장 프로그램, 공유 기기, 화면 공유/로그 노출 등 클라이언트 측 위험을 완전히 제거하기 어렵다고 판단했습니다.
5. **Alternative review** — 메모리에만 임시 보관하는 방식도 검토했지만, 브라우저가 credential을 취급한다는 근본적인 특성은 남는다고 판단했습니다.
6. **Deployment decision** — 포트폴리오 공개판에서는 PAT 입력·저장·AI API 호출을 모두 제거했습니다.
7. **Production alternative** — 실제 서비스화 시 AI 요청과 secret 처리를 backend/BFF로 이동하는 구조를 채택하는 것이 적절하다고 정리했습니다.

### Public-build guarantees | 공개판에서 지키는 원칙

현재 `main`의 공개판은 다음을 의도적으로 지킵니다.

- **No GitHub PAT input** — 방문자의 GitHub PAT를 요구하지 않습니다.
- **No API-key input** — 다른 API key도 요구하지 않습니다.
- **No credential persistence** — `localStorage`, `sessionStorage`, cookie 등에 credential을 저장하지 않습니다.
- **No task persistence** — 할 일 데이터는 현재 탭의 메모리에만 존재하며 브라우저 저장소나 서버에 영구 저장하지 않습니다.
- **No external AI request** — 공개판에서 GitHub Models 또는 다른 AI API를 호출하지 않습니다.
- **No task transmission** — 현재 우선순위 계산을 위해 할 일 내용을 외부 서버로 전송하지 않습니다.
- **No hidden AI claim** — 규칙 기반 결과를 AI 결과처럼 표현하지 않습니다.
- **Text-safe rendering** — 사용자 입력을 HTML로 삽입하지 않고 DOM `textContent`로 표시합니다.
- **Restrictive CSP** — 공개 페이지는 외부 네트워크 연결을 허용하지 않는 Content Security Policy를 포함합니다.

### What this does NOT guarantee | 보장하지 않는 것

보안을 중요하게 다뤘다는 것이 **절대적인 안전을 보장한다는 뜻은 아닙니다.** 브라우저, 운영체제, 호스팅 플랫폼, 사용자 기기 자체가 침해된 상황까지 이 정적 애플리케이션이 방어할 수는 없습니다.

현재 공개판은 credential을 제거했기 때문에 AI 기반 개인화 분석을 제공하지 않습니다. 이는 공개 포트폴리오에서 사용자 credential을 직접 취급하지 않기 위해 선택한 기능상의 trade-off입니다.

### Security trade-off | 선택의 장단점

| Decision | Benefit | Trade-off |
|---|---|---|
| PAT 제거 | 방문자 credential 노출 위험을 크게 줄임 | 공개판의 실제 AI 호출 제거 |
| 외부 API 호출 제거 | 작업 데이터가 분석을 위해 외부로 전송되지 않음 | 모델 기반 추론 사용 불가 |
| 규칙 기반 분석 | 계산 기준을 사용자가 이해할 수 있음 | AI의 유연한 문맥 판단 없음 |
| 정적 배포 | 구조와 운영이 단순하고 비용이 낮음 | 안전한 서버 측 secret 관리 기능 없음 |

---

## Architecture Decision | 아키텍처 결정

### Original prototype | 초기 프로토타입

```text
Browser
  ├─ User tasks
  ├─ User-provided GitHub PAT
  └─ JavaScript
          ↓ Authorization: Bearer <PAT>
     GitHub Models API
```

초기 프로토타입의 구조를 공개 배포 관점에서 검토한 결과, 정적 웹사이트가 방문자의 PAT를 직접 취급하는 방식은 현재 공개판의 보안 원칙과 맞지 않았습니다.

### Public portfolio build | 현재 공개판

```text
Browser
  ├─ Task input (memory only)
  ├─ Importance / urgency / estimated time (memory only)
  └─ Local rule-based scoring
          ↓
     Priority result

Task persistence: none
Theme preference: localStorage only
Credentials: none
External AI requests: none
```

### Production-ready direction | 실제 서비스 확장 방향

```text
Browser
   ↓ authenticated request
Backend / BFF
   ├─ authorization & validation
   ├─ rate limiting / abuse controls
   ├─ logging with secret redaction
   └─ server-side secret management
             ↓
          AI provider
```

실제 서비스에서는 브라우저에 공용 AI secret을 제공하지 않고, backend/BFF가 인증·인가·입력 검증·rate limiting을 담당한 뒤 서버 측에서 관리되는 secret으로 AI provider를 호출하는 구조가 적절합니다. 서비스 요구사항에 따라 사용자의 OAuth 기반 권한 위임 등 별도의 인증 설계도 필요합니다.

## Why not store the PAT only in memory? | 메모리에만 저장하지 않은 이유

페이지 메모리에만 PAT를 보관하면 `localStorage`처럼 장기 지속되지는 않는다는 장점이 있습니다. 하지만 페이지에서 실행되는 JavaScript가 토큰에 접근할 수 있다는 사실 자체는 남습니다. 공개 포트폴리오의 핵심 경험에 방문자의 credential이 필수적이지 않았기 때문에, 위험을 일부 완화하는 대신 **credential 처리 자체를 공개판에서 제거**했습니다.

## Original AI Work | 기존 AI 구현 기록

초기 프로토타입에서는 다음을 구현하고 검토했습니다.

- GitHub Models API integration
- `gpt-4o-mini` task analysis
- User-provided PAT authentication
- Priority, estimated time, category, and reasoning output
- Credential-free sample/demo flow
- Browser persistence experiments

공개 배포를 위한 리팩터링 전에 원본 상태를 **`archive/original-ai-prototype` branch**로 보존했습니다. 이 브랜치는 구현 이력 확인을 위한 기록이며, 공개 서비스 사용을 권장하는 배포판이 아닙니다.

## Local Development | 로컬 실행

공개판은 별도의 토큰이나 환경변수가 필요하지 않은 정적 웹 프로젝트입니다.

```bash
git clone <repository-url>
cd ai-productivity-assistant
python3 -m http.server 8000
```

브라우저에서 `localhost:8000`으로 접속하면 됩니다. 단순 파일 열기보다 로컬 HTTP 서버 사용을 권장합니다.

### Local security note | 로컬 보안 유의사항

- 공개판 실행에는 PAT/API key가 필요하지 않습니다.
- 저장소에 `.env`, PAT, API key를 추가하거나 commit하지 마세요.
- `archive/original-ai-prototype`의 credential 입력 기능은 학습 이력이며 공개 배포용이 아닙니다.
- credential이 노출되었다고 의심되면 해당 provider에서 즉시 revoke/rotate 해야 합니다.
- 실제 사용자 데이터나 회사 기밀을 테스트 데이터로 사용하는 것은 피하는 것이 좋습니다.

## Project Structure | 프로젝트 구조

```text
ai-productivity-assistant/
├── index.html
├── README.md
├── SECURITY.md
├── PRD.md
├── AGENTS.md
└── staticwebapp.config.json   # legacy Azure deployment artifact
```

`staticwebapp.config.json`은 기존 Azure Static Web Apps 배포 이력을 보여주는 legacy artifact입니다. 현재 공개 배포 구조에서는 Azure credential이나 서비스 의존성을 사용하지 않습니다.

## What I Learned | 배운 점

This project reinforced that shipping a product is not only about making a feature work. A feature can be technically valid while still being inappropriate for a particular public deployment architecture if its trust and security model does not fit that environment.

이 프로젝트를 통해 **기능 구현과 공개 배포의 적합성은 별도로 검토해야 한다**는 점을 배웠습니다. credential 흐름, 데이터 흐름, 배포 구조와 사용자 신뢰를 함께 검토해 공개판의 기능 범위를 결정했습니다.

Service & Marketing 관점에서 중요하게 생각해 온 **사용자 신뢰와 서비스 경험**을 개발 의사결정에도 연결한 프로젝트입니다.

---

Built for **Lipcoding 2026** · `Challenges & Hackathons`