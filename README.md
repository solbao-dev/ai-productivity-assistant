# TabDone

> **One tab. One focus. Get it done.**  
> 한 탭에서 정하고, 집중하고, 끝내는 security-first productivity assistant

**Productivity Assistant · Focus Session**  
**Lipcoding 2026 · Challenge Project**  
`JavaScript` `Product Design` `Security` `Privacy` `GitHub Models (prototype)`

---

## Why I Built This | 개발 배경

This project started from a familiar digital habit: finding useful information on Instagram, saving it for later, and rarely returning to it. The same pattern appeared while working — many browser tabs stayed open as reminders of things to read or do, but the growing number of tabs created more distraction than progress.

이 프로젝트는 아주 일상적인 경험에서 시작했습니다. **인스타그램에서 유용한 정보를 발견해 저장해두지만 다시 보지 않는 경험**, 그리고 나중에 해야 할 일이나 읽어야 할 자료를 잊지 않으려고 **브라우저 탭을 여러 개 열어두지만 오히려 무엇에 집중해야 할지 흐려지는 경험**이 반복됐습니다.

문제는 정보를 더 많이 저장하는 것이 아니라, **저장한 것을 실제 행동으로 전환하고 한 번에 하나에 집중하는 것**이라고 생각했습니다. 그래서 또 하나의 장기 보관형 Todo 앱을 만드는 대신, 하나의 탭 안에서 지금 해야 할 일을 정하고 실행한 뒤 세션 자체를 끝내는 도구를 기획했습니다.

> **Save less. Decide what matters. Focus in one tab. Finish the session.**
>
> 더 많이 쌓아두기보다, 지금 중요한 일을 결정하고 한 탭에서 집중해 끝내는 경험을 목표로 합니다.

## Product Concept | 서비스 컨셉

**TabDone**이라는 이름은 `Tab + Done`에서 출발합니다. 여러 탭에 해야 할 일을 쌓아두는 대신 **하나의 탭을 하나의 집중 세션으로 사용하고, 그 안에서 일을 실제로 끝낸다**는 제품 철학을 담았습니다.

이 프로젝트의 핵심은 장기적인 할 일 보관이 아니라 **“한 번 앉았을 때 무엇부터 할지 결정하고, 집중해서 하나씩 끝내는 실행 세션”**입니다.

```text
Open one tab
   ↓
Write today's tasks
   ↓
Set importance / urgency / estimated time
   ↓
Check priority
   ↓
Start focus stopwatch
   ↓
Complete tasks one by one
   ↓
Close the tab and end the session
```

즉, **하나의 탭 = 하나의 집중 세션**입니다. 사용자는 탭을 열어 오늘 처리할 일을 정리하고, 우선순위와 스톱워치를 이용해 실행에 집중합니다. 완료한 작업은 즉시 제거할 수 있고, 세션을 마치면 탭을 닫아 작업 흐름도 함께 종료합니다.

## Product Perspective | 제품 관점

서비스의 출발점은 **“어떻게 더 많이 저장할까?”가 아니라 “어떻게 실제 실행으로 이어지게 할까?”**입니다. Service & Marketing 도메인 경험을 바탕으로 기술 자체를 전면에 내세우기보다, 사용자의 행동과 집중 흐름을 먼저 정의하고 이에 맞춰 기능과 데이터 보관 방식을 결정했습니다.

초기 프로토타입에서는 GitHub Models API를 이용해 AI가 우선순위, 예상 시간, 분류와 이유를 분석하도록 구현했습니다. 공개 배포를 준비하면서 credential과 데이터 흐름을 검토했고, 정적 클라이언트가 방문자의 GitHub PAT를 직접 요구하지 않도록 공개판을 재설계했습니다.

현재 공개판은 **credential-free, rule-based focus assistant**입니다. 중요도·긴급도·예상 시간을 기반으로 브라우저 안에서 우선순위를 계산합니다.

## Public Features | 공개판 기능

- Add and prioritize tasks | 할 일 추가 및 우선순위 계산
- Importance / urgency / estimated time | 중요도·긴급도·예상시간 설정
- Focus stopwatch | 집중 시간 스톱워치
- Session continuity across refresh | 같은 탭에서 새로고침해도 할 일·타이머 유지
- Complete tasks one by one | 작업별 완료 처리
- Full session reset | 전체 세션 초기화
- Responsive layout | 데스크톱·태블릿·모바일 대응
- Light/Dark theme | 시스템 테마 연동 및 사용자 직접 전환
- No credential input | PAT·API key 입력 없음
- No external AI/API request | 외부 AI/API 호출 없음

## Session Data Model | 세션 데이터 설계

서비스 컨셉에 맞춰 **할 일과 스톱워치는 `sessionStorage`**, 테마 취향은 `localStorage`로 분리합니다.

| Data | Storage | Retention | External transmission |
|---|---|---|---|
| 할 일 내용 | 현재 탭의 `sessionStorage` | 같은 탭 새로고침 시 유지. 완료/전체 초기화 시 삭제. 탭·브라우징 세션 종료 시 제거되는 세션 데이터 | 없음 |
| 중요도·긴급도·예상시간 | `sessionStorage` | 해당 할 일과 동일 | 없음 |
| 스톱워치 상태 | `sessionStorage` | 같은 세션에서 새로고침 후 이어짐. 초기화 또는 세션 종료 시 제거 | 없음 |
| 계산된 우선순위 | 브라우저에서 로컬 계산 | 작업 데이터와 함께 세션 범위에서 사용 | 없음 |
| Light/Dark 선택 | `localStorage` (`pa-theme`) | 별도 만료기간 없음. 사용자가 변경하거나 사이트 데이터를 삭제할 때까지 | 없음 |
| PAT / API key | 수집하지 않음 | 저장하지 않음 | 없음 |

### Why sessionStorage? | 왜 sessionStorage인가?

`localStorage`에 할 일을 장기간 남기는 것은 이 서비스의 의도와 맞지 않습니다. 반대로 JavaScript 메모리에만 두면 실수로 새로고침했을 때 집중 세션 전체가 사라집니다.

`sessionStorage`는 **같은 탭에서 새로고침을 견디면서도 장기 보관을 목적으로 하지 않는 세션 데이터**라는 점에서 현재 제품 컨셉에 가장 잘 맞는 선택입니다.

> 브라우저의 세션 복원 기능이나 구현 방식에 따라 탭/브라우징 세션 복원 동작은 달라질 수 있으므로 “탭을 닫는 즉시 모든 환경에서 물리적으로 완전 삭제된다”고 보장하지 않습니다. 이 앱은 자체 서버에 할 일 데이터를 전송하거나 저장하지 않습니다.

---

## Security & Privacy | 보안·개인정보

보안은 배포 후 추가하는 옵션이 아니라 **공개 여부와 기능 범위를 결정하는 요구사항**으로 다뤘습니다.

### Security review process | 보안 검토 과정

1. **Credential inventory** — 초기 버전에서 사용자의 GitHub PAT가 필요함을 확인
2. **Data-flow review** — 브라우저에서 PAT를 읽어 GitHub Models API로 전달하는 흐름 검토
3. **Persistence review** — 초기 구현의 PAT `localStorage` 저장 구조 확인
4. **Threat review** — XSS, 악성 스크립트, 브라우저 확장 프로그램, 공유 기기 및 노출 위험 검토
5. **Alternative review** — 메모리 임시 보관 방식도 검토했으나 브라우저가 credential을 직접 취급하는 특성은 유지됨을 확인
6. **Deployment decision** — 공개판에서 PAT 입력·저장·AI API 호출 제거
7. **Production alternative** — 실제 서비스화 시 AI 요청과 secret 처리를 backend/BFF로 이동하는 방향 정리

### Public-build principles | 공개판 원칙

- 방문자의 GitHub PAT/API key를 요구하거나 저장하지 않음
- 할 일·타이머는 해당 집중 세션의 `sessionStorage`에만 저장
- 테마 설정 외 장기적인 브라우저 저장을 사용하지 않음
- 할 일 데이터를 외부 AI/API로 전송하지 않음
- 우선순위는 브라우저에서 명시적인 규칙으로 계산
- 사용자 입력은 HTML로 삽입하지 않고 안전한 텍스트로 렌더링
- CSP에서 외부 네트워크 연결을 제한

### Security trade-off | 선택의 장단점

| Decision | Benefit | Trade-off |
|---|---|---|
| PAT 제거 | 방문자 credential 노출 위험 감소 | 공개판의 실제 AI 호출 제거 |
| 외부 API 제거 | 작업 데이터 외부 전송 없음 | 모델 기반 문맥 추론 없음 |
| `sessionStorage` | 새로고침을 견디면서 장기 저장 최소화 | 세션 종료 후 장기 task manager로 사용 불가 |
| 규칙 기반 분석 | 계산 기준이 투명함 | AI의 유연한 판단 없음 |
| 정적 배포 | 구조·운영 단순화 | 서버 측 secret 관리 기능 없음 |

## Architecture | 아키텍처

### Original AI prototype

```text
Browser + User PAT
        ↓
GitHub Models API
```

### TabDone public build

```text
Browser tab
  ├─ Tasks → sessionStorage
  ├─ Stopwatch → sessionStorage
  ├─ Theme preference → localStorage
  └─ Local priority scoring

Credentials: none
External AI requests: none
Task server storage: none
```

### Production-ready direction

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

## Original AI Work | 기존 AI 구현 기록

초기 프로토타입에서는 GitHub Models API, `gpt-4o-mini` 기반 task analysis, user-provided PAT authentication, priority/time/category/reasoning output 등을 구현했습니다. 공개 배포 리팩터링 전 상태는 **`archive/original-ai-prototype` branch**에 구현 이력으로 보존했습니다.

## Local Development | 로컬 실행

```bash
git clone <repository-url>
cd ai-productivity-assistant
python3 -m http.server 8000
```

브라우저에서 `localhost:8000`으로 접속하면 됩니다. 공개판 실행에는 PAT/API key나 별도의 환경변수가 필요하지 않습니다.

## What I Learned | 배운 점

이 프로젝트를 통해 **사용자의 실제 행동 문제에서 제품을 정의하고, 그 제품 의도를 기술 선택으로 연결하는 과정**을 경험했습니다. 장기 task manager가 아니라 집중 실행 세션이라는 컨셉에 맞춰 `sessionStorage`를 선택했고, 공개 배포에서는 credential 위험을 검토해 기능 범위를 재설계했습니다. 이를 통해 기능을 더 많이 넣는 것보다 사용자의 행동 흐름, 보안, 데이터 보관 원칙이 일관되게 연결되는 것이 중요하다는 점을 학습했습니다.

---

**TabDone** · Built for **Lipcoding 2026** · `Challenges & Hackathons`