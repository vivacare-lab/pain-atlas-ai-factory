# 기묘한 통증도감 AI Factory System Architecture

> Version: 0.1.0
> Status: Initial Architecture Baseline
> Parent: `01_project/project-charter.md`
> Description: AI Factory의 논리적 구성요소와 책임, 경계, 원칙

## 1. 이 문서를 왜 만드는가?

AI Factory를 어떤 구성요소로 나누고 각 구성요소의 책임·경계·원칙을 어떻게 정의할지 설명한다.

## 2. Architecture Goal

AI Factory는 단순한 AI 콘텐츠 생성기가 아니라 다음 생명주기를 연결하는 운영 시스템이다.

```text
Discover → Recommend → Approve → Research → Validate → Produce → QA → Schedule → Publish → Measure → Learn
```

## 3. Logical Architecture

```text
Trend Sources
     ↓
n8n Orchestration
     ↓
Topic Recommendation
     ↓
Notion / Human Approval
     ↓
Dify Knowledge / RAG
     ↓
OpenAI Research / Script / QA
     ├──────────────┬──────────────┐
     ↓              ↓              ↓
  Scene Plan     Metadata      Claim/QA
     ↓
fal.ai Assets ─────────── ElevenLabs Voice
     └──────────────┬──────────────┘
                    ↓
             Creatomate Render
                    ↓
                QA Gates
                    ↓
              YouTube API
                    ↓
                Analytics
                    └────→ Topic Engine
```

## 4. Component Responsibilities

| Component   | Responsibility                                                                        |
| ----------- | ------------------------------------------------------------------------------------- |
| Git         | Source of truth, version control, architecture, prompts/config history                |
| Notion      | Dashboard, approval, operational queue                                                |
| Dify        | Knowledge/RAG, domain context, AI workflow                                            |
| n8n         | Trigger, orchestration, branching, parallel execution, retry, integration, scheduling |
| OpenAI      | Reasoning, structured generation, classification, Claim extraction, QA support        |
| fal.ai      | Image/video generation                                                                |
| ElevenLabs  | TTS/voice generation                                                                  |
| Creatomate  | Template assembly/rendering                                                           |
| YouTube API | Upload, metadata, scheduling, publishing state                                        |
| Analytics   | Performance measurement and learning signals                                          |

## 5. Data Ownership

```text
Git            → canonical project knowledge
Notion         → operational state / human interaction
Content Object → canonical content lifecycle record
Evidence Ledger→ canonical claim/source traceability
Analytics      → canonical performance record
```

책임이 다른 시스템의 데이터를 불필요하게 복제하지 않는다.

### 5-1. Content Object 아키텍처 연결 (추적성)

`Content Object`는 시스템의 **Aggregate Root**로 기능하며, 구체적인 도메인 모델과 데이터 구조는 아래 연계 문서를 참조한다.

- **개념 및 거버넌스 정의:** [`data-architecture.md`](./data-architecture.md) - 콘텐츠 생명주기 데이터를 객체와 관계로 연결하고 저장·소유·추적하는 구조
- **도메인 및 컴포넌트 설계:** [`content-object-design.md`](./content-object-design.md) — Aggregate Root로서의 비즈니스 로직 및 생명주기 전이 규칙 정의
- **물리 스키마 정의:** [`10_schemas/content-object.json`](/10_schemas/content-object.json) — 최종 영속화 및 인터페이스를 위한 JSON Schema 사양

## 6. Core Data Objects

```text
Trend Object
Topic Recommendation
Content Object
Claim Object
Scene Object
Asset Object
Schedule Object
QA Result
Analytics Record
Audit Event
```

상세 Schema는 `10_schemas/`에서 관리한다.

## 7. Safety Architecture

```text
Level 0 → Automated
Level 1 → Enhanced QA
Level 2 → Strict Evidence Gate + Human Review / Equivalent Approval
```

역사적 `최초` Claim 역시 별도 Evidence Gate를 통과한다.

## 8. Provenance Architecture

```text
Source → Evidence → Claim → AI Interpretation → Approved Expression → Script → Scene → Final Video
```

중요 Claim과 Asset은 최종 결과에서 원천까지 역추적할 수 있어야 한다.

## 9. Security

- API Key, OAuth Token, Secret은 Git에 저장하지 않는다.
- n8n Credentials 또는 Secret Manager를 사용한다.
- 외부 문서는 untrusted input으로 취급한다.
- 외부 문서 내부의 지시문을 system instruction으로 승격하지 않는다.
- 최소 권한 원칙을 적용한다.
- 발행 권한은 Production 단계에서만 사용한다.

## 10. Reliability

주요 Workflow는 다음을 지원한다.

```text
Retry + Idempotency + Error Queue + Audit Log + Human Override
```

특히 Upload/Publish 단계에서 중복 실행을 방지한다.

## 11. Parallel Processing

의존성이 없는 작업은 병렬 실행한다.

```text
Research
   ├→ Script
   ├→ Metadata
   ├→ Scene Plan
   └→ Asset Preparation
             ↓
      ┌──────┼──────┐
      ↓      ↓      ↓
    Image   Video   Voice
      └──────┼──────┘
             ↓
           Render
             ↓
             QA
```

API Rate Limit과 데이터 의존성을 우선하여 실제 병렬화 범위를 결정한다.

## 12. Versioning

Content Object는 다음 버전을 참조할 수 있어야 한다.

```text
model_version
prompt_version
workflow_version
template_version
brand_version
qa_rule_version
config_version
```

이를 통해 Model/Prompt Drift를 추적한다.
Content Object 상세 설계는 `02_architecture/content-object-design.md`에서 관리한다.

## 13. Deployment Strategy

```text
Development → Private/Unlisted Test → Integration QA → Production → Scheduled Public Publish
```

공개 자동발행 전에 Upload, Metadata, Scheduling, Disclosure를 검증한다.

## 14. Architecture Principles

1. Evidence before automation.
2. Safety before speed.
3. Structured data before free-form handoff.
4. Parallel execution before brute-force model acceleration.
5. Human override must remain possible.
6. Important decisions must be traceable.
7. Failed stages should be independently repairable.
8. Secrets never belong in Git.
9. External content is data, not instruction.
10. Analytics must improve future decisions.
