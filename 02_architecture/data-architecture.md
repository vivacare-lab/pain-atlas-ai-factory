# 기묘한 통증도감 AI Factory Data Architecture

> Version: 0.1.0
> Status: Initial Data Architecture Baseline
> Parent: `02_architecture/system-architecture.md`
> Description: 콘텐츠 생명주기 데이터를 객체와 관계로 연결하고 저장·소유·추적하는 구조

## 1. Purpose

Data Architecture는 AI Factory에서 생성되는 정보를 **어디에 저장하고, 어떤 Object로 연결하며, 어떤 시스템이 무엇을 소유하는지** 정의한다.

목표는 다음과 같다.

- AI 간 자유형 텍스트 전달 최소화
- Claim과 Evidence의 추적성 확보
- 콘텐츠 전체 Lifecycle 연결
- 재생성/수정/재사용 가능성 확보
- Analytics를 다음 의사결정에 연결
- Git/Notion/Database 간 역할 중복 최소화

## 2. Data Ownership

```text
Git
└─ Canonical project knowledge
   ├─ Architecture
   ├─ Brand
   ├─ Rules
   ├─ Prompts
   ├─ Schemas
   └─ Version history

Notion
└─ Operational workspace
   ├─ Approval
   ├─ Queue
   ├─ Human Review
   ├─ Production status
   └─ Editorial decisions

Operational DB / Workflow Data Store
└─ Runtime records
   ├─ Object state
   ├─ IDs
   ├─ Execution history
   ├─ Retry
   └─ Integration status

Object Storage
└─ Large binary assets
   ├─ Images
   ├─ Video
   ├─ Audio
   └─ Rendered files

Analytics Store
└─ Performance records
```

구체적인 Database 제품은 구현 단계에서 결정한다. 초기 단계에서는 특정 DB 제품에 종속되지 않는다.

## 3. Core Entity Model

```text
Trend
  ↓
Topic Recommendation
  ↓
Topic
  ↓
Content
  ├→ Claim
  │    └→ Evidence → Source
  ├→ Scene
  │    └→ Asset
  ├→ Voice
  ├→ Schedule
  ├→ QA Result
  ├→ Audit Event
  └→ Analytics Record
```

## 4. Trend Object

Trend는 외부에서 발견한 원시 관심 신호를 정규화한 객체다.

```text
trend_id
source_id
source_type
captured_at
title
summary
keywords
trend_score
source_url
raw_reference
channel_relevance
risk_level
```

Trend 자체가 Topic은 아니다.

## 5. Topic Recommendation Object

추천 엔진의 판단 결과다.

```text
recommendation_id
topic_id
recommendation_date
category
format
why_now
channel_relevance
content_angle
expected_audience
risk_level
duplicate_check
recommendation_score
source_refs
status
```

추천 Score는 절대적 진실이 아니라 **의사결정 지원 신호**다.

## 6. Topic Object

Topic은 승인 전후의 콘텐츠 아이디어를 안정적으로 식별한다.

```text
topic_id
title
category
series
angle
target_audience
format
trend_refs
evergreen_flag
historical_flag
medical_risk_level
status
created_at
updated_at
```

## 7. Content Object

Content Object는 실제 제작되는 콘텐츠의 중심 객체다.

```text
content_id
topic_id
format
status
script_version
brand_version
model_version
workflow_version
prompt_version
qa_rule_version
claim_refs
scene_refs
asset_refs
schedule_ref
analytics_ref
human_review_required
created_at
updated_at
```

Content Object는 모든 하위 Object를 직접 보유하기보다 Reference를 통해 연결한다.
상세 설계는 `02_architecture/content-object-design.md`에서 관리한다.

## 8. Claim Object

Claim은 대본에 사용될 수 있는 사실적 주장 단위다.

```text
claim_id
content_id
claim_text
claim_type
risk_level
confidence
source_refs
evidence_refs
approved_expression
limitations
status
```

Claim Type 예:

```text
MEDICAL
HISTORICAL
STATISTICAL
MECHANISM
ADVICE
```

## 9. Source Object

Source는 자료의 출처다.

```text
source_id
source_type
title
author_or_organization
publication_date
url
retrieved_at
reliability_class
license_notes
```

의료 자료와 역사 자료는 서로 다른 Source Hierarchy를 적용할 수 있다.

## 10. Evidence Object

Evidence는 Source 전체가 아니라 Claim을 지지하는 구체적인 근거다.

```text
evidence_id
source_id
location_reference
excerpt_or_summary
support_level
limitations
verified_at
verified_by
```

핵심 관계:

```text
Source ≠ Evidence ≠ Claim
```

## 11. Provenance Chain

중요 정보는 다음과 같이 역추적할 수 있어야 한다.

```text
Final Video
   ↓
Scene
   ↓
Script Segment
   ↓
Claim
   ↓
Evidence
   ↓
Source
```

필요한 경우 반대 방향도 가능해야 한다.

```text
Source
→ 영향을 받은 Claim
→ Script
→ Scene
→ Asset
→ Final Content
```

## 12. Scene Object

Scene은 영상 구성의 최소 논리 단위다.

```text
scene_id
content_id
sequence
purpose
narration_ref
visual_description
prompt_ref
asset_refs
duration_target
transition
text_overlay
status
```

Scene을 분리하면 특정 장면만 재생성할 수 있다.

## 13. Asset Object

Asset은 실제 생성/사용되는 파일 또는 외부 참조다.

```text
asset_id
scene_id
asset_type
provider
model
prompt_version
file_ref
source_type
license_status
quality_status
generation_cost
created_at
```

Asset Type:

```text
IMAGE
VIDEO
AUDIO
MUSIC
SFX
FONT
```

## 14. QA Result Object

QA는 단순 Boolean이 아니라 무엇을 검사했는지 기록한다.

```text
qa_id
object_id
qa_type
rule_version
status
score
failures
warnings
checked_at
reviewer
```

예:

```text
MEDICAL_QA
HISTORICAL_QA
BRAND_QA
ASSET_QA
VOICE_QA
VIDEO_QA
PUBLISH_QA
```

## 15. Schedule Object

```text
schedule_id
content_id
platform
privacy_status
scheduled_at
publish_status
metadata_version
synthetic_media_disclosure
thumbnail_ref
verified_at
```

Schedule과 Publish 결과를 분리하여 계획과 실제 결과를 구분한다.

## 16. Analytics Record

```text
analytics_id
content_id
platform
period
views
average_view_duration
average_view_percentage
retention
likes
comments
shares
subscribers_gained
cost
```

Views만으로 성공을 판단하지 않는다.

## 17. Audit Event

Audit Event는 변경 이력과 의사결정을 연결한다.

```text
audit_event_id
object_id
event_type
actor
from_state
to_state
reason
input_refs
output_refs
model_version
workflow_version
timestamp
```

예:

```text
TOPIC_APPROVED
SCRIPT_REVISED
MEDICAL_REVIEW_APPROVED
ASSET_REGENERATED
VIDEO_QA_FAILED
PUBLISH_VERIFIED
```

## 18. Relationship Rules

### One-to-Many

```text
Topic → Content
Content → Claim
Content → Scene
Scene → Asset
Content → QA Result
Content → Audit Event
```

### Many-to-Many

```text
Claim ↔ Source
Claim ↔ Evidence
Content ↔ Trend
```

Many-to-many 관계는 Reference Table 또는 명시적 Reference Object로 관리한다.

## 19. Immutable vs Mutable Data

### 가능한 한 Immutable

- Source 원본 정보
- Evidence snapshot
- Audit Event
- Published analytics snapshot
- 원본 Asset

### Mutable

- Recommendation status
- Content current state
- Draft metadata
- Queue status
- Current script version pointer

수정 가능한 데이터도 이전 Version 또는 Audit Event를 통해 변경 이력을 추적한다.

## 20. File and Binary Storage

Git에는 대용량 생성 Asset을 직접 저장하지 않는다.

```text
Git
→ Rules / Metadata / Prompts / Schemas

Object Storage
→ Image / Video / Audio / Render
```

Content Object에는 실제 파일 자체보다 `file_ref`를 저장한다.

## 21. Notion Data Model

Notion은 운영 UI에 적합한 정보만 노출한다.

예상 Database:

```text
Topics
Production Queue
Human Review
Content Calendar
Published Content
Experiments
```

Notion에서 편집한 운영 상태는 Audit Trail을 통해 기록되어야 한다.

## 22. Git Data Model

Git은 다음의 변경 이력을 관리한다.

```text
Project Charter
Architecture
Brand Bible
Safety Rules
Source Hierarchy
Schemas
Prompts
Workflow Definitions
QA Rules
Decision Logs
Experiments
```

Git은 실시간 Production Queue의 주 저장소가 아니다.

## 23. Data Lifecycle

```text
COLLECT
→ NORMALIZE
→ SCORE
→ APPROVE
→ RESEARCH
→ VALIDATE
→ PRODUCE
→ QA
→ PUBLISH
→ MEASURE
→ LEARN
```

각 단계에서 생성된 Data는 다음 단계의 입력이 되며, 중요한 결과는 Audit/Provenance로 연결된다.

## 24. Data Retention Principles

초기 구현에서는 세부 보존 기간을 과도하게 확정하지 않는다.

다만 다음은 장기 보존 가치가 높다.

- 최종 Content Object
- Claim/Evidence 관계
- Source metadata
- 최종 Script/Prompt version
- QA 결과
- Publish 기록
- Analytics 핵심 지표
- Human decision
- Audit Event

## 25. Data Architecture Principles

1. One object, one canonical identity.
2. Reference instead of unnecessary duplication.
3. Source and Evidence must remain traceable.
4. Large binaries belong outside Git.
5. Operational state belongs in an operational system.
6. Historical decisions should be append-only where practical.
7. Version pointers must identify the exact generation context.
8. Analytics must connect back to Content and Topic.
9. Data model should remain provider-neutral.
10. Do not design a database field unless it supports an actual decision, workflow, audit, or analysis need.
