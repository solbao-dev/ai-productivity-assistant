# Deployment History | 배포 이력

This document preserves the deployment history of the project separately from the current public runtime configuration.

이 문서는 현재 공개 배포 설정과 과거 해커톤 배포 이력을 분리하여 기록하기 위한 문서입니다.

## 1. Hackathon deployment | 해커톤 당시 배포

During the hackathon phase, the project was deployed with **Azure Static Web Apps** as part of the deployment environment used for the challenge. The repository therefore contained Azure-specific deployment artifacts, including a GitHub Actions workflow and Static Web Apps configuration.

해커톤 진행 당시에는 과제/행사 환경에서 사용한 배포 방식에 맞춰 **Azure Static Web Apps**로 프로젝트를 배포했습니다. 이 때문에 저장소에는 Azure 배포용 GitHub Actions workflow와 Static Web Apps 설정 파일 등 Azure 관련 배포 흔적이 존재했습니다.

## 2. Post-hackathon transition | 해커톤 종료 후 전환

After the hackathon ended, the Azure-hosted deployment was retired. The public portfolio build was subsequently redesigned as a credential-free static application and moved to **GitHub Pages**.

해커톤 종료 후 기존 Azure 배포는 종료했습니다. 이후 공개 포트폴리오 버전은 방문자의 credential을 요구하지 않는 정적 애플리케이션으로 재설계했고, 공개 배포 대상도 **GitHub Pages**로 전환했습니다.

Current public deployment:

```text
GitHub repository (main)
        ↓
GitHub Pages
        ↓
Credential-free TabDone public build
```

## 3. Why the Azure workflow was removed | Azure workflow 제거 이유

The legacy Azure Static Web Apps GitHub Actions workflow continued to run after the Azure deployment had been retired. Because the former Azure deployment target/API credential was no longer available, the obsolete workflow produced failed checks on otherwise valid commits.

현재 공개 서비스는 GitHub Pages를 사용하므로 더 이상 필요하지 않은 Azure Static Web Apps workflow가 새 커밋에서도 실행되고 있었습니다. 종료된 Azure 배포 대상/API credential을 찾지 못해 정상적인 애플리케이션 커밋에도 실패 체크가 표시되었기 때문에, 운영 설정과 과거 이력을 분리하기 위해 해당 workflow를 `main`에서 제거했습니다.

Removal commit:

```text
chore: remove obsolete Azure deployment workflow
```

This was a deployment cleanup, not a deletion of project history.

이는 프로젝트 이력을 없앤 것이 아니라 **현재 운영 설정에서 종료된 배포 파이프라인을 분리한 정리 작업**입니다.

## 4. Archived evidence | 별도 보관한 흔적

Historical implementation/deployment context is intentionally preserved outside the current runtime path:

- `archive/original-ai-prototype` branch — pre-public-refactor implementation history, including the original AI prototype context
- Git history — previous Azure workflow/configuration changes remain traceable through repository history
- This document — records why Azure was used, why it was retired, and why GitHub Pages became the current public deployment

현재 실행 경로에는 불필요한 Azure CI/CD를 남겨두지 않되, **왜 Azure를 사용했고 왜 종료했는지 추적 가능한 형태로 보존**하는 것을 원칙으로 합니다.

## 5. Azure retirement verification | Azure 종료 확인

After removing the obsolete GitHub workflow, the Azure account was reviewed separately from the repository. At the time of the review, the portal showed:

- no Azure resources listed in the active directory
- no billing subscriptions listed
- current amount due: `₩0.00`
- current billing-period total: `₩0.00`
- no invoices shown for the previous six months
- trial credit expired

GitHub deployment configuration and Azure billing/resource state were therefore checked independently.

GitHub workflow 삭제만으로 Azure 과금 종료를 가정하지 않고 Azure Portal에서도 별도로 확인했습니다. 확인 당시 활성 디렉터리에 리소스와 청구 구독이 표시되지 않았고, 현재 지불액과 청구 기간 합계는 모두 `₩0.00`이었습니다.

---

**Current deployment:** GitHub Pages  
**Historical deployment:** Azure Static Web Apps (hackathon phase, retired)
