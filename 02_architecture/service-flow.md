# 기묘한 통증도감 AI Factory Service Flow

> Version: 0.2.0
> Status: Initial Service Flow Baseline
> Parent: `01_project/project-charter.md`
> Description: 콘텐츠 발견부터 발행·분석·학습까지의 Workflow 실행 흐름과 Canonical Content State의 관계

## 0. 이 문서를 왜 만드는가?

Service Flow는 AI Factory가 실제로 어떤 Workflow를 실행하는지 정의한다. 단, Workflow 실행 단계와 `Content Object.lifecycle.current_state`는 서로 다른 계약이므로 명시적으로 분리한다.

## 1. Workflow Execution Stages

```text
DAILY_TRIGGER
→ TREND_DISCOVERY
→ TOPIC_RECOMMENDING
→ AWAITING_CONFIRMATION
→ TOPIC_APPROVED
→ RESEARCHING
→ PLANNING
→ FACT_CHECK
→ SAFETY_CHECK
→ BRAND_CHECK
→ ASSET_GENERATION
→ ASSET_QA
→ RENDERING
→ VIDEO_QA
→ SCHEDULING
→ SCHEDULED
→ PUBLISHING
→ PUBLISHED
→ ANALYTICS_LEARNING
```

위 값은 Workflow/Runtime의 실행 단계다. `Content Object.lifecycle.current_state`에 직접 저장하지 않는다.

## 2. Canonical Content State

`Content Object.lifecycle.current_state`는 다음 Enum만 사용한다.

```text
TOPIC_APPROVED
RESEARCHING
RESEARCH_COMPLETED
SCRIPT_DRAFTED
FACT_CHECKED
SCRIPT_APPROVED
SCENE_PLANNED
ASSETS_READY
VIDEO_QA_PASSED
PUBLISHED
ANALYTICS_READY
LEARNING_COMPLETED
```

### 2.1 의미

```text
Content State
= 콘텐츠가 의미론적으로 어느 완료 지점까지 도달했는가

Workflow Stage
= 자동화가 현재 어떤 실행 작업을 수행하고 있는가
```

예:

```text
current_state   = RESEARCHING
workflow_stage  = RESEARCH
workflow_run_id = RUN-...
retry_count     = 2
```

Workflow가 실패해도 `current_state`를 임의의 ERROR 상태로 바꾸지 않는다. 실패와 복구 정보는 `workflow`와 `audit_summary`에 기록한다.

## 3. State / Workflow Mapping

| Workflow Stage | 성공 시 Content State |
|---|---|
| TOPIC_APPROVED | TOPIC_APPROVED |
| RESEARCHING | RESEARCH_COMPLETED |
| PLANNING / Script Draft | SCRIPT_DRAFTED |
| FACT_CHECK | FACT_CHECKED |
| SCRIPT Approval | SCRIPT_APPROVED |
| SCENE Planning | SCENE_PLANNED |
| ASSET_GENERATION + ASSET_QA | ASSETS_READY |
| RENDERING + VIDEO_QA | VIDEO_QA_PASSED |
| PUBLISHING | PUBLISHED |
| ANALYTICS_LEARNING | ANALYTICS_READY → LEARNING_COMPLETED |

`SAFETY_CHECK`, `BRAND_CHECK`, `SCHEDULING`, `SCHEDULED`는 중요한 Workflow Gate/Execution 단계지만 별도의 Canonical Content State로 승격하지 않는다. 결과는 QA/Publication/Audit 객체에 기록한다.

## 4. Human Confirmation

```text
APPROVE
REJECT
REQUEST_REVISION
DEFER
```

- APPROVE → `TOPIC_APPROVED`
- REJECT → Recommendation/Topic archive workflow
- REQUEST_REVISION → Recommendation revision workflow
- DEFER → Backlog

승인 이벤트는 Audit Event로 기록한다.

## 5. Research

```text
Topic
→ Source Collection
→ Evidence Extraction
→ Claim Candidates
→ Research Package
→ Research Completion
```

외부 자료는 untrusted input으로 취급하며, Source hierarchy와 provenance를 적용한다.

## 6. Validation

### Fact Check

```text
Claim
→ Source
→ Evidence
→ Approved Expression
```

`최초`, `가장 오래된`, `인류 최초` 등의 표현은 강화 검증한다.

### Medical Safety

```text
LEVEL 0 → Standard QA
LEVEL 1 → Enhanced Safety QA
LEVEL 2 → Strict Evidence Gate + Human Review / Equivalent Approval
```

## 7. Planning / Script

```text
Content Angle
→ Structure
→ Script Draft
→ Fact Check
→ Script Approval
→ Scene Plan
```

## 8. Asset / Voice

```text
Scene Plan
→ Image / Video Generation
→ Voice Generation
→ Asset QA / Voice QA
→ Assets Ready
```

가능한 작업은 병렬화하되 API rate limit과 데이터 의존성을 우선한다.

## 9. Rendering / Video QA

```text
Assets Ready
→ Render
→ Video QA
→ VIDEO_QA_PASSED
```

실패 시 필요한 artifact만 재생성하고 전체 pipeline을 불필요하게 재실행하지 않는다.

## 10. Scheduling / Publishing

```text
VIDEO_QA_PASSED
→ Schedule Validation
→ SCHEDULED
→ PUBLISHING
→ PUBLISHED
```

Publish는 idempotency key와 audit event를 사용한다.

## 11. Analytics / Learning

```text
PUBLISHED
→ Analytics Collection
→ ANALYTICS_READY
→ Learning
→ LEARNING_COMPLETED
```

성과 데이터는 다음 Topic Recommendation, Hook, Format, Long-form Candidate, Production Cost에 반영한다.

## 12. Failure Flow

```text
Workflow Stage
→ FAIL
→ Error Classification
├─ Retry
├─ Repair / Regenerate
├─ Re-run QA
└─ Error Queue → Human Review
```

Workflow 실패 자체는 Canonical Content State를 변경하는 근거가 아니다.

## 13. Completion

운영 완료는 다음 조건을 모두 충족해야 한다.

```text
Evidence Validated
+ Safety Validated
+ Brand Validated
+ Assets Passed
+ Video Passed
+ Schedule Validated
+ Published / Scheduled
+ Audit Recorded
```
