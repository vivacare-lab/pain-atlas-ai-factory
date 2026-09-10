# 기묘한 통증도감 AI Factory State Machine

> Version: 0.2.0
> Status: Canonical Content State Baseline
> Parent: `02_architecture/internal-protocol.md`
> Related: `02_architecture/content-object-design.md`, `02_architecture/service-flow.md`, `10_schemas/content-object.json`
> Description: Content Object의 Canonical State와 허용 전이를 정의

## 0. 이 문서를 왜 만드는가?

State Machine은 콘텐츠의 의미론적 라이프사이클 상태와 상태 전이를 정의한다. Workflow의 실행 단계, retry, provider error, repair branch는 별도 Runtime 정보다.

## 1. Canonical Content States

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

## 2. State Transition

```text
TOPIC_APPROVED
    ↓
RESEARCHING
    ↓
RESEARCH_COMPLETED
    ↓
SCRIPT_DRAFTED
    ↓
FACT_CHECKED
    ↓
SCRIPT_APPROVED
    ↓
SCENE_PLANNED
    ↓
ASSETS_READY
    ↓
VIDEO_QA_PASSED
    ↓
PUBLISHED
    ↓
ANALYTICS_READY
    ↓
LEARNING_COMPLETED
```

## 3. State Definitions

| State | Entry Condition | Success Condition |
|---|---|---|
| TOPIC_APPROVED | Human approval completed | Research workflow may start |
| RESEARCHING | Approved topic exists | Research package completed |
| RESEARCH_COMPLETED | Required evidence/claims collected | Script draft may be generated |
| SCRIPT_DRAFTED | Draft exists | Fact check can start |
| FACT_CHECKED | Claims validated | Script approval can start |
| SCRIPT_APPROVED | Final script approved | Scene planning can start |
| SCENE_PLANNED | Scene plan validated | Asset generation can start |
| ASSETS_READY | Required assets/voice passed QA | Render can start |
| VIDEO_QA_PASSED | Final video passed QA | Publication package can be scheduled |
| PUBLISHED | Publication verified | Analytics collection can start |
| ANALYTICS_READY | Required analytics snapshot collected | Learning can start |
| LEARNING_COMPLETED | Learning record finalized | Lifecycle cycle completed |

## 4. Workflow State와의 분리

Workflow Stage의 예:

```text
RESEARCHING
PLANNING
FACT_CHECK
SAFETY_CHECK
BRAND_CHECK
ASSET_GENERATION
ASSET_QA
RENDERING
VIDEO_QA
SCHEDULING
PUBLISHING
```

이 값들은 `lifecycle.current_state`가 아니라 `workflow.workflow_stage`에 기록한다.

예:

```json
{
  "lifecycle": {
    "current_state": "RESEARCHING"
  },
  "workflow": {
    "workflow_stage": "RESEARCH",
    "retry_count": 2
  }
}
```

## 5. Failure / Retry

```text
Current Content State
        ↓
Workflow Execution
        ↓
FAIL
   ├── Retry
   ├── Repair
   ├── Regenerate
   └── Human Review
        ↓
Re-run QA
        ↓
Valid Output
        ↓
Next Content State
```

실패했다고 `current_state = ERROR`로 변경하지 않는다.

오류는 `workflow.last_error`, `retry_count`, Error Queue, Audit Event에 기록한다.

## 6. State Transition Contract

모든 전이는 다음을 만족해야 한다.

```text
Current State
→ Preconditions
→ Action
→ Output Validation
→ Allowed Transition
→ Audit Event
```

허용되지 않은 전이는 거부한다.

## 7. Derived / Cancelled / Archived Content

`CANCELLED`, `ARCHIVED`, `FAILED`, `BLOCKED`는 Canonical Content State Enum에 포함하지 않는다.

이들은 lifecycle 상태와 별개인 운영/예외 상태로 관리한다.

- blocked: Workflow가 진행되지 못하는 실행 조건
- cancelled: 콘텐츠 제작/발행 의도 자체를 취소한 운영 결정
- archived: 추천/Topic 단계의 보관
- failed: 특정 Workflow Run 또는 Artifact의 실패
- human review: 검토가 필요한 실행 상태

이 구분을 통해 Content State의 의미를 안정적으로 유지한다.
