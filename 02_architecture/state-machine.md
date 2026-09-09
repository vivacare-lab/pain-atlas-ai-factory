# 기묘한 통증도감 AI Factory State Machine

> Version: 0.1.0
> Status: Initial State Machine Baseline
> Parent: `02_architecture/internal-protocol.md`
> Description: 콘텐츠 상태와 상태 전이, Gate, 실패·복구 규칙

## 1. Purpose

State Machine은 콘텐츠 하나가 AI Factory 안에서 **현재 무엇을 해야 하는지, 다음에 어디로 이동할 수 있는지, 실패하면 어디로 돌아가는지**를 정의한다.

State는 단순한 진행 표시가 아니라 자동화의 제어 조건이다.

## 2. State Categories

```text
CONTROL
  DAILY_TRIGGER
  AWAITING_CONFIRMATION

PRODUCTION
  TOPIC_APPROVED
  RESEARCHING
  PLANNED
  FACT_CHECK
  SAFETY_CHECK
  BRAND_CHECK
  ASSET_GENERATION
  ASSET_QA
  RENDERING
  VIDEO_QA

DELIVERY
  SCHEDULING
  SCHEDULED
  PUBLISHING
  PUBLISHED

LEARNING
  ANALYTICS_LEARNING

EXCEPTION
  BLOCKED
  ERROR
  HUMAN_REVIEW
  ARCHIVED
```

## 3. Canonical State Flow

```text
DAILY_TRIGGER
    ↓
TREND_DISCOVERY
    ↓
TOPIC_RECOMMENDING
    ↓
AWAITING_CONFIRMATION
    ├─ APPROVE → TOPIC_APPROVED
    ├─ REJECT → ARCHIVED
    ├─ REQUEST_REVISION → TOPIC_RECOMMENDING
    └─ DEFER → AWAITING_CONFIRMATION / BACKLOG

TOPIC_APPROVED
    ↓
RESEARCHING
    ↓
PLANNED
    ↓
FACT_CHECK
    ↓
SAFETY_CHECK
    ↓
BRAND_CHECK
    ↓
ASSET_GENERATION
    ↓
ASSET_QA
    ↓
RENDERING
    ↓
VIDEO_QA
    ↓
SCHEDULING
    ↓
SCHEDULED
    ↓
PUBLISHING
    ↓
PUBLISHED
    ↓
ANALYTICS_LEARNING
```

## 4. State Definition Table

| State | Entry Condition | Main Action | Success Exit |
|---|---|---|---|
| DAILY_TRIGGER | Schedule reached | Start workflow | TREND_DISCOVERY |
| TREND_DISCOVERY | Source Registry available | Collect/normalize trends | TOPIC_RECOMMENDING |
| TOPIC_RECOMMENDING | Trend + history available | Score candidates | AWAITING_CONFIRMATION |
| AWAITING_CONFIRMATION | Recommendations ready | Human decision | APPROVE/REJECT/REVISION/DEFER |
| TOPIC_APPROVED | Human approval | Create Content Object | RESEARCHING |
| RESEARCHING | Topic valid | Gather evidence/claims | PLANNED |
| PLANNED | Research sufficient | Structure content | FACT_CHECK |
| FACT_CHECK | Claims extracted | Validate factual basis | SAFETY_CHECK |
| SAFETY_CHECK | Claims available | Medical risk/safety review | BRAND_CHECK |
| BRAND_CHECK | Safety passed | Check identity/tone | ASSET_GENERATION |
| ASSET_GENERATION | Scene plan approved | Generate assets/voice | ASSET_QA |
| ASSET_QA | Assets available | Validate assets | RENDERING |
| RENDERING | Inputs complete | Assemble video | VIDEO_QA |
| VIDEO_QA | Render complete | Validate final video | SCHEDULING |
| SCHEDULING | Video passed | Validate publishing package | SCHEDULED |
| SCHEDULED | Schedule accepted | Wait for publish time | PUBLISHING |
| PUBLISHING | Publish conditions met | Upload/publish/verify | PUBLISHED |
| PUBLISHED | Publication verified | Collect metrics | ANALYTICS_LEARNING |
| ANALYTICS_LEARNING | Metrics available | Update learning signals | COMPLETE/ARCHIVE |

## 5. Gate Rules

각 Gate는 단순 생성 성공이 아니라 **다음 단계에 들어갈 자격**을 판단한다.

### Evidence Gate

```text
Required Claim
→ Evidence Available?
→ Source Quality Acceptable?
→ Uncertainty Preserved?
```

### Medical Safety Gate

```text
Risk Level
→ Required Evidence
→ Red Flag / Contraindication Check
→ Wording Check
→ Human Review if required
```

### Brand Gate

```text
Identity
→ Tone
→ Visual Style
→ Information Density
→ Ending / Brand Line
```

### Asset Gate

```text
Scene Match
→ Anatomical/Historical Accuracy
→ Visual Quality
→ License
→ Safety
```

### Video Gate

```text
Content
→ Medical/History
→ Audio
→ Subtitle
→ Technical
→ Brand
```

## 6. Repair Loops

실패한 단계 전체를 처음부터 다시 실행하지 않는다.

```text
FACT_CHECK FAIL
→ RESEARCHING / HUMAN_REVIEW

SAFETY_CHECK FAIL
→ PLANNED / HUMAN_REVIEW

BRAND_CHECK FAIL
→ PLANNED

ASSET_QA FAIL
→ ASSET_GENERATION

VIDEO_QA FAIL
→ RENDERING or earlier required stage

SCHEDULING FAIL
→ SCHEDULING

PUBLISHING FAIL
→ PUBLISHING / HUMAN_REVIEW
```

실패 원인이 이전 단계의 데이터에 있으면 해당 단계까지 되돌아간다.

## 7. Human Review State

Human Review는 어느 단계에서도 진입할 수 있는 예외 상태다.

```text
ANY STATE
   ↓
HUMAN_REVIEW
   ├→ APPROVE → Resume designated next state
   ├→ REVISE → Return to designated repair state
   ├→ REJECT → ARCHIVED
   └→ HOLD → BLOCKED
```

## 8. Blocked vs Error

### BLOCKED
사람 또는 외부 조건의 판단이 필요하여 의도적으로 진행을 멈춘 상태.

예:
- 출처 충돌
- License 확인 대기
- 사용자 승인 대기
- 중요한 의료 표현 검토 대기

### ERROR
시스템 실행 자체가 실패한 상태.

예:
- API timeout
- Schema validation failure
- Render failure
- Upload failure

## 9. Retry Rules

```text
Transient Error
→ Retry
→ Retry Limit
→ ERROR QUEUE

Content / Safety Error
→ Repair
→ QA

Unresolved High-Risk Issue
→ HUMAN_REVIEW
```

Retry는 State를 무조건 초기화하지 않는다. 현재 Object와 실패 Stage를 유지한다.

## 10. Idempotent Transition

같은 Transition Event가 반복되어도 결과가 중복 생성되지 않아야 한다.

```text
transition_id = object_id + from_state + to_state + operation_id
```

특히 Publish 관련 Transition은 중복 실행 방지를 강제한다.

## 11. Parallel States

하나의 콘텐츠 안에서 독립적인 작업은 병렬로 실행할 수 있다.

```text
PLANNED
   ├→ Script QA preparation
   ├→ Scene Plan
   ├→ Metadata
   └→ Asset preparation

ASSET_GENERATION
   ├→ Image generation
   ├→ Video generation
   └→ Voice generation
```

모든 병렬 작업이 완료되고 필요한 QA를 통과해야 다음 단계로 이동한다.

## 12. Completion Definition

`PUBLISHED`는 업로드 요청 성공이 아니라 실제 YouTube 상태가 확인된 경우에만 인정한다.

최종적으로 다음을 모두 확인해야 한다.

```text
Upload Success
+ Metadata Verified
+ Privacy Verified
+ Publish/Schedule Verified
+ Synthetic Media Disclosure Verified when applicable
+ Audit Event Recorded
```

## 13. State Machine Design Principle

상태 수를 불필요하게 늘리지 않는다.

```text
State = 운영상 의사결정을 바꾸는 상태
Event = 상태를 변경시키는 사건
Metadata = 상태 자체가 아닌 부가 정보
```

이 원칙을 지켜야 시스템이 복잡해져도 사용자가 전체 흐름을 통제할 수 있다.
