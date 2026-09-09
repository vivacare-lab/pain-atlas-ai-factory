# 기묘한 통증도감 AI Factory Service Flow
> Version: 0.1.0
> Status: Initial Service Flow Baseline
> Parent: `01_project/project-charter.md`
> Description: 콘텐츠가 발견부터 발행·학습까지 이동하는 서비스 흐름

## 1. 이 문서를 왜 만드는가?

콘텐츠가 발견부터 발행·분석·학습까지 어떤 서비스 단계를 거치는지 정의한다.

## 2. End-to-End Flow

```text
DAILY_TRIGGER
→ TREND_DISCOVERY
→ TOPIC_RECOMMENDING
→ AWAITING_CONFIRMATION
→ TOPIC_APPROVED
→ RESEARCHING
→ PLANNED
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

## 3. Daily Trend Discovery

**Input:** Scheduled trigger, Trend Source Registry, previous content, analytics learning data.

**Process:** n8n 실행 → 승인된 Source 조회 → Raw Data 정규화 → 중복 제거 → Trend Object 생성 → 채널 적합도 분석.

**Output:** `Trend Objects`

## 4. Topic Recommendation

```text
Trend + Brand + Content History + Analytics + Risk Rules
→ Candidate Topics
→ Semantic Duplicate Check
→ Recommendation Score
→ Top 3–5
```

추천에는 Topic, Why Now, Channel Relevance, Content Angle, Expected Format, Risk Level, Source Summary, Duplicate Check, Recommendation Score를 포함한다.

## 5. Human Confirmation

사용자는 다음 중 하나를 선택한다.

```text
APPROVE
REJECT
REQUEST_REVISION
DEFER
```

- APPROVE → Production
- REJECT → Archive
- REQUEST_REVISION → Recommendation Revision
- DEFER → Backlog

승인 이벤트는 Audit Trail에 기록한다.

## 6. Research

승인된 Topic을 Research Workflow로 전달한다.

원칙:
- 외부 자료는 untrusted input이다.
- Source hierarchy를 적용한다.
- Claim 후보를 추출한다.
- Source와 Evidence를 연결한다.
- 불확실성을 보존한다.

**Output:** `Research Package + Source Provenance + Claim Candidates`

## 7. Fact Check

### Medical

```text
Claim → Source → Evidence Level → Population → Limitation → Approved Expression
```

### Historical

```text
Claim → Source → Primary/Secondary → Date → Context → Interpretation → Confidence → Approved Wording
```

`최초` 계열 표현은 강화 검증한다.

## 8. Medical Safety

```text
LEVEL 0 → Standard QA
LEVEL 1 → Enhanced Safety QA
LEVEL 2 → Strict Evidence Gate → Human Review / Equivalent Approval
```

진단 단정, 보장성 치료 표현, 금기 및 Red Flag 누락을 검사한다.

## 9. Content Planning & Script

검증된 Claim을 기반으로:

```text
Content Angle → Structure → Script → Voice Script → Scene Plan
```

일반 시청자가 이해할 수 있는 표현과 채널의 Brand Voice를 유지한다.

## 10. Brand Check

검사 항목:
- Channel identity
- Tone
- Brand statement
- Video brand line
- Visual direction
- Information density
- Medical/history positioning

FAIL 시 Planning 또는 Script 단계로 되돌린다.

## 11. Asset Generation

```text
Scene → Prompt → Image/Video → Asset Metadata → License Status
```

가능한 경우 Scene별 생성은 병렬 처리한다.

## 12. Asset QA

검사:
- Scene/Prompt 일치
- Anatomical accuracy
- Historical consistency
- Visual style
- Distortion
- Gore level
- Text contamination
- License status

FAIL 시 해당 Asset만 재생성한다.

## 13. Voice Generation

```text
Approved Script → Voice Script → TTS → Voice QA
```

발음, 속도, 자연스러움, 대본 일치, 길이와 타이밍을 검사한다.

## 14. Rendering

Creatomate Template에 다음을 결합한다.

```text
Scene Assets + Voice + Subtitle + Music/SFX + Brand Elements
```

고정 Template과 재사용 Asset을 활용하여 제작시간을 줄인다.

## 15. Video QA

### Content
- Script consistency
- Claim consistency
- Medical safety
- Historical accuracy

### Technical
- Duration
- Aspect ratio
- Resolution
- Audio
- Subtitle
- Frame errors

### Brand
- Visual consistency
- Typography
- Brand line
- Ending structure

FAIL 시 필요한 단계로 되돌린다.

## 16. Schedule

`READY → Schedule Validation → SCHEDULED`

검사:
- Title
- Description
- Metadata
- Privacy
- Publish time
- Synthetic Media Disclosure
- Thumbnail/Cover requirements

## 17. Publishing

```text
Upload → Metadata → Privacy → Schedule → Verify
```

Idempotency Key와 Publish Audit Event를 사용하여 중복 발행을 방지한다.

## 18. Analytics Learning

발행 후 Retention, Completion, Engagement, Subscribers, Topic Performance, Cost를 수집한다.

결과를 Topic Recommendation, Hook, Format, Long-form Candidate, Production Cost에 반영한다.

## 19. Failure Flow

```text
Stage → FAIL → Classify Error
```

복구 가능한 오류는 `Retry → Repair → Re-run QA`, 복구 불가능한 오류는 `Error Queue → Human Review`로 보낸다.

## 20. Human Review Flow

```text
Risk / Ambiguity Detected
↓
HUMAN_REVIEW_REQUIRED
↓
Approve / Revise / Cancel
```

사람의 결정은 Audit Trail에 남긴다.

## 21. Parallelization Strategy

가능한 작업을 다음과 같이 병렬화한다.

```text
Research
↓
┌──────── Script ────────┐
├──────── Metadata ──────┤
├──────── Scene Plan ────┤
└──────── Asset Prep ────┘
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

실제 병렬화 여부는 데이터 의존성과 API 제한을 고려한다.

## 22. Human Time KPI

사용자는 AI Factory의 모든 작업을 직접 수행하는 대신 `Review + Approve + Exception Handling`에 집중한다.

초기 목표:
- Shorts: `< 5–10 min human time`
- Long-form: `< 15–20 min human time`

## 23. Service Flow Completion Definition

콘텐츠는 다음 조건을 충족하면 완료로 본다.

```text
Approved
+ Evidence Validated
+ Safety Validated
+ Brand Validated
+ Assets Passed
+ Video Passed
+ Schedule Validated
+ Published / Scheduled
+ Audit Recorded
```
