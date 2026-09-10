# 기묘한 통증도감 AI Factory Internal Protocol

> Version: 0.1.1
> Status: Canonical Handoff Protocol
> Parent: `02_architecture/system-architecture.md`
> Related: `02_architecture/state-machine.md`, `02_architecture/service-flow.md`, `02_architecture/content-object-design.md`
> Description: Workflow 간 데이터 전달, 상태 변경, 버전, 오류, 재처리 규칙

## 0. 이 문서를 왜 만드는가?

AI Factory의 각 Workflow가 동일한 데이터 계약을 사용하도록 Input/Output, State, Version, Retry, Idempotency, Audit 규칙을 정의한다.

## 1. 기본 원칙

- 자유형 텍스트보다 구조화된 Object를 우선한다.
- `Content State`와 `Workflow Stage`를 분리한다.
- 상태 변경은 State Machine의 허용 전이를 따른다.
- 외부 데이터는 untrusted input으로 취급한다.
- 실패한 단계만 재실행할 수 있어야 한다.
- Evidence, Safety, License 조건을 자동화보다 우선한다.

## 2. Canonical Handoff

```json
{
  "object_type": "content_object",
  "object_id": "CNT-20260910-0001",
  "schema_version": "0.1.0",
  "content_state": "RESEARCHING",
  "workflow_stage": "RESEARCH",
  "workflow_version": "0.1.0",
  "input_refs": [],
  "output_refs": [],
  "human_review_required": false,
  "created_at": "2026-09-10T09:00:00+09:00",
  "updated_at": "2026-09-10T09:30:00+09:00"
}
```

`content_state`는 Canonical Content State다.

`workflow_stage`는 현재 실행 중인 Workflow 단계다.

## 3. Object Identity

주요 Object는 안정적인 ID를 가진다.

```text
trend_id
recommendation_id
topic_id
content_id
research_id
source_id
evidence_id
claim_id
script_id
scene_plan_id
asset_id
asset_set_id
qa_id
publication_id
analytics_id
learning_id
decision_id
audit_event_id
```

재생성으로 기존 객체를 임의 덮어쓰지 않는다. 의미 있는 변경은 revision/version으로 추적한다.

## 4. Version Contract

가능한 경우 다음을 기록한다.

```text
schema_version
model_version
prompt_version
workflow_version
template_version
brand_version
qa_rule_version
config_version
```

## 5. State Transition Protocol

```text
Current Content State
→ Preconditions
→ Workflow Action
→ Output Validation
→ Allowed State Transition
→ Audit Event
```

검증에 실패하면 정상 다음 상태로 이동하지 않는다.

## 6. Input Contract

최소 확인 항목:

- Object ID
- Schema version compatibility
- Expected Content State
- Required fields
- Evidence / License / Safety conditions
- Idempotency
- Human approval requirement

## 7. Output Contract

```json
{
  "status": "PASS",
  "output_refs": [],
  "warnings": [],
  "errors": [],
  "next_content_state": "FACT_CHECKED"
}
```

`next_content_state`는 State Machine의 허용 전이여야 한다.

## 8. Retry / Idempotency

주요 작업에는 가능한 경우:

```text
idempotency_key
workflow_run_id
retry_count
last_error
```

를 기록한다.

반복 실패는 무한 retry하지 않고 Error Queue / Dead-letter로 이동한다.

## 9. Audit

상태 변경과 주요 행동은 Audit Event로 기록한다.

최소:

```text
event_id
content_id
event_type
actor
from_content_state
to_content_state
workflow_stage
reason
input_refs
output_refs
timestamp
```
