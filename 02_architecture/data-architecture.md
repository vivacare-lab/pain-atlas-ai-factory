# 기묘한 통증도감 AI Factory Data Architecture

> Version: 0.1.1
> Status: Initial Data Architecture Baseline
> Parent: `02_architecture/system-architecture.md`
> Description: AI Factory의 핵심 데이터 객체, 소유권, 관계, 수명주기 및 Content Object 연결 규칙

---

## 0. 이 문서를 왜 만드는가?

AI Factory는 Trend, Topic, Research, Claim, Evidence, Script, Scene, Asset, QA, Publication, Analytics, Learning 등 서로 다른 데이터를 여러 단계에서 생성한다.

이 문서는 이 데이터들이 **어떤 객체로 분리되고, 서로 어떻게 연결되며, 어떤 시스템이 무엇을 책임지는지** 정의한다.

특히 `Content Object`를 콘텐츠 생명주기의 중심 연결점으로 명확히 정의하여, 이후 `10_schemas/`의 실제 JSON Schema가 임의로 설계되지 않도록 하는 것이 목적이다.

---

## 1. 이 문서에서 반드시 이해해야 하는 것

### 1.1 Content Object는 모든 데이터를 담는 거대한 JSON이 아니다.

`Content Object`는 콘텐츠의 정체성, 현재 상태, 목적, 주요 참조와 요약 정보를 관리하는 **Aggregate Root**다.

상세 데이터는 독립 객체로 분리하고 Content Object에서는 참조한다.

```text
Content Object
    ↓
Research / Claim / Evidence / Script / Scene / Asset
    ↓
QA / Publication / Analytics / Learning
```

### 1.2 현재 상태와 과거 이력을 분리한다.

Content Object는 현재 사용 중인 Script, Scene, Asset, QA 등의 참조를 관리한다.

과거 버전과 상세 실행 기록은 각 독립 객체 또는 Audit Log에서 보존한다.

### 1.3 가장 중요한 추적 경로

```text
Script Segment
      ↓
Claim
      ↓
Evidence
      ↓
Source
```

```text
Script Segment
      ↓
Scene
      ↓
Asset
```

```text
Publication
      ↓
Analytics
      ↓
Learning
      ↓
Decision
```

---

## 2. AI Factory에서 이 문서가 담당하는 역할

Data Architecture는 다음을 결정한다.

1. 핵심 데이터 객체의 경계
2. 객체 간 참조 관계
3. 데이터 소유권
4. Content Object의 위치와 책임
5. 현재 값과 이력 데이터의 분리 원칙
6. Schema 설계의 상위 규칙
7. Git / Notion / Dify / n8n 등 실행 시스템과 데이터의 관계

실제 필드 타입과 JSON Schema의 상세 제약은 `10_schemas/`에서 정의한다.

---

## 3. 실제 시스템에서는 어떻게 사용되는가?

### 3.1 전체 데이터 흐름

```text
Trend Source
    ↓
Trend Object
    ↓
Topic Recommendation
    ↓
Human Approval
    ↓
Content Object 생성
    ↓
Research Package
    ↓
Source / Evidence / Claim
    ↓
Script Version
    ↓
Scene Plan
    ↓
Asset Set
    ↓
QA Record
    ↓
Publication Record
    ↓
Analytics Snapshot
    ↓
Learning Record
    ↓
Next Decision
```

### 3.2 Content Object의 위치

Content Object는 위 흐름 전체를 하나의 `content_id`로 연결한다.

```text
Content Object
├── identity
├── lifecycle
├── classification
├── intent
├── references
├── current_outputs
├── quality_summary
├── publication_summary
├── performance_summary
├── learning_summary
├── cost_summary
├── provenance_summary
├── workflow
└── audit_summary
```

상세 설계는 다음 문서를 따른다.

`02_architecture/content-object-design.md`

---

## 4. 구현 세부사항

### 4.1 Core Data Objects

```text
Trend Object
Topic Recommendation
Content Object
Research Package
Source
Evidence
Claim
Script Version
Scene Plan
Asset
Asset Set
QA Record
Publication Record
Analytics Snapshot
Learning Record
Decision Record
Audit Event
```

### 4.2 객체별 기본 책임

| Object | 책임 |
|---|---|
| Trend Object | 외부/내부 데이터에서 발견된 트렌드의 정규화된 기록 |
| Topic Recommendation | 채널 적합도와 추천 근거를 포함한 제작 후보 |
| Content Object | 하나의 콘텐츠 생명주기를 연결하는 중심 객체 |
| Research Package | 특정 콘텐츠의 조사 실행 결과와 조사 상태 |
| Source | 외부 근거의 출처와 메타데이터 |
| Evidence | Source에서 확인된 구체적인 근거 |
| Claim | 콘텐츠에서 사용할 수 있는 검증 대상 주장 |
| Script Version | 특정 버전의 대본 |
| Scene Plan | Script Segment와 시각적 표현의 연결 |
| Asset | 실제 이미지/영상/음성 등 생성 또는 수집 자산 |
| Asset Set | 특정 콘텐츠 제작에 사용되는 Asset 묶음 |
| QA Record | 특정 대상에 대한 검증 실행 결과 |
| Publication Record | 플랫폼별 발행 및 예약 정보 |
| Analytics Snapshot | 특정 시점의 성과 관측값 |
| Learning Record | Analytics를 해석하여 도출한 학습과 다음 행동 |
| Decision Record | 사람이 내렸거나 시스템이 기록한 중요한 의사결정 |
| Audit Event | 상태 전이와 주요 시스템 행동의 불변 기록 |

### 4.3 데이터 소유권

```text
Git
→ canonical project knowledge / version-controlled definitions

Notion
→ operational queue / human interaction

Content Object
→ canonical content lifecycle record

Evidence Ledger
→ canonical claim/source traceability

Analytics
→ canonical performance record
```

동일한 의미의 데이터를 여러 시스템에서 서로 다른 원본으로 관리하지 않는다.

### 4.4 Content Object와 Supporting Object의 경계

#### Content Object에 직접 포함

- 콘텐츠 식별 정보
- 콘텐츠 계보 정보
- 현재 상태
- 콘텐츠 분류
- 제작 목적
- 핵심 질문과 Hook
- 주요 객체 참조
- 현재 사용 중인 산출물 버전
- QA 요약
- 발행 요약
- 성과 요약
- 학습 요약
- 비용 요약
- Provenance 요약
- Audit 요약

#### 독립 객체로 분리

- 원문 Source 전문
- Evidence 상세
- 전체 Research 결과
- Claim 상세 기록
- 대본 전체 버전 이력
- 장면 계획 전체 버전 이력
- 실제 이미지/영상/음성 파일
- Prompt 전문
- Model Configuration
- 전체 Analytics 시계열
- 상세 QA 실행 기록
- 불변 Audit Log
- Secret / Credential

### 4.5 Content ID와 버전의 관계

```text
Content ID
= 콘텐츠의 정체성

Artifact Version
= 대본/장면/Asset 등 산출물의 버전

Workflow Version
= 자동화 Workflow의 버전

Model / Prompt / Brand / QA Rule Version
= 해당 실행에 사용된 구성요소의 버전
```

대본이 `0.2.0 → 0.3.0`으로 변경되어도 같은 콘텐츠 계보라면 `content_id`는 변경하지 않는다.

### 4.6 Content Object와 State Machine의 관계

```text
State Machine
= 어떤 상태 전이가 허용되는가

Content Object
= 현재 어떤 상태에 있는가

Artifact Object
= 해당 상태에서 어떤 결과물이 생성되었는가
```

따라서 `current_state`는 Content Object에 저장하되, 상태 전이의 허용 규칙 자체는 State Machine 문서에서 관리한다.

### 4.7 Provenance 관계

AI Factory는 최종 결과에서 중요한 Claim과 Asset을 원천까지 역추적할 수 있어야 한다.

```text
Source
  ↓
Evidence
  ↓
Claim
  ↓
AI Interpretation
  ↓
Approved Expression
  ↓
Script
  ↓
Scene
  ↓
Asset / Final Video
```

이 구조는 의료·역사 콘텐츠의 사실성 검증과 사후 감사에 사용한다.

### 4.8 Progressive Completion

Content Object는 생성 시점부터 모든 필드를 완성할 필요가 없다.

다음 세 상태를 구분한다.

```text
missing = 아직 생성되지 않음
null    = 해당 없음
empty   = 생성되었지만 값이 없음
```

예:

```yaml
publication_summary:
  publication_refs: []
  published_platforms: []
  latest_publication_at: null
```

이는 발행 객체가 아직 생성되지 않았음을 의미한다.

### 4.9 재사용과 파생 콘텐츠

하나의 콘텐츠에서 Short, Long-form, Reel, Blog 등 여러 파생물이 생성될 수 있다.

```text
Root Content
├── YouTube Short
├── YouTube Long-form
├── Instagram Reel
└── Blog Article
```

이를 위해 Content Object는 다음 계보 정보를 가진다.

```text
parent_content_id
root_content_id
```

Asset 또한 여러 콘텐츠에서 재사용할 수 있으므로 Asset의 정체성과 콘텐츠별 사용 관계를 분리한다.

### 4.10 QA와 Audit

QA는 현재 상태 요약과 상세 실행 기록을 분리한다.

```text
Content Object
→ latest_qa_record_ref
→ overall_status
→ blocking_issues
→ required_human_review
```

상세 QA 기록은 별도 `QA Record`로 보존한다.

Audit 역시 Content Object에는 요약과 최신 이벤트 참조만 저장하고 상세 이벤트는 별도 `Audit Event`로 관리한다.

### 4.11 Analytics와 Learning

Analytics는 관측값이고 Learning은 해석 및 다음 행동이다.

```text
Analytics Snapshot
= 무엇이 발생했는가

Learning Record
= 무엇을 배웠는가

Decision Record
= 그래서 무엇을 바꿀 것인가
```

Analytics를 단순 조회값으로 덮어쓰지 않고 관측 시점을 보존한다.

### 4.12 비용 데이터

콘텐츠 단위 비용을 추적할 수 있어야 한다.

```text
Research
LLM
Image
Video
TTS
Render
Storage
```

Content Object에는 총 비용과 요약만 저장하고 상세 비용 기록은 별도 객체로 분리할 수 있도록 한다.

### 4.13 버전 및 Provenance 참조

Content Object는 다음 실행 구성요소를 참조할 수 있어야 한다.

```text
model_version
prompt_version
workflow_version
template_version
brand_version
qa_rule_version
config_version
```

이를 통해 동일 콘텐츠가 언제, 어떤 설정으로 생성·검증되었는지 추적한다.

### 4.14 Schema 계층

설계 문서와 실제 Schema를 분리한다.

```text
02_architecture/content-object-design.md
        ↓
10_schemas/content-object.json
        ↓
07_content/ actual content instances
```

하위 객체도 독립 Schema로 확장한다.

```text
content-object.json
research-package.json
claim.json
evidence.json
script.json
scene-plan.json
asset.json
asset-set.json
qa-record.json
publication-record.json
analytics-snapshot.json
learning-record.json
audit-event.json
```

초기 구현에서는 모든 Schema를 한 번에 만들지 않고, Content Object를 시작점으로 단계적으로 확장한다.

---

## 5. 설계 불변 원칙

1. `content_id`는 콘텐츠 계보 전체에서 안정적이어야 한다.
2. Content Object는 상세 데이터 저장소가 아니라 중심 Aggregate Root다.
3. 현재 참조와 과거 이력을 분리한다.
4. Claim과 Evidence는 재사용 가능한 독립 객체로 관리한다.
5. Script Segment는 Claim을 추적할 수 있어야 한다.
6. Scene은 Script Segment를 참조한다.
7. Asset은 콘텐츠와 독립적으로 식별한다.
8. Publication은 플랫폼별로 분리한다.
9. Analytics는 Snapshot으로 보존한다.
10. Learning은 Analytics와 분리한다.
11. QA 상세 기록은 삭제하지 않는다.
12. Workflow 실패는 콘텐츠의 의미 있는 상태와 분리한다.
13. 외부 입력은 실행 명령이 아닌 데이터로 취급한다.
14. Secret은 Content Object나 Git에 저장하지 않는다.

---

## 6. 다음 단계와 완료 기준

### 다음 단계

`02_architecture/content-object-design.md`의 설계를 기준으로 `10_schemas/content-object.json`을 작성한다.

### 완료 기준

- Content Object의 필드 구조가 설계 문서와 일치한다.
- 필드의 필수/선택 여부가 정의된다.
- Enum과 기본값이 필요한 항목이 명확하다.
- State Machine의 상태명과 충돌하지 않는다.
- 다른 객체의 상세 데이터를 중복 저장하지 않는다.
- Provenance와 Audit 참조를 유지한다.
- 실제 콘텐츠 인스턴스가 생성될 수 있는 수준의 Schema가 된다.


## 5. Canonical Content Object Contract

`Content Object`의 실제 중심 구조는 `content-object-design.md`를 기준으로 한다.

```text
Content Object
├── identity
├── lifecycle
├── classification
├── intent
├── references
├── current_outputs
├── quality_summary
├── publication_summary
├── performance_summary
├── learning_summary
├── cost_summary
├── provenance_summary
├── workflow
└── audit_summary
```

### 5.1 Lifecycle

`lifecycle.current_state`는 다음 Canonical Content State만 사용한다.

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

Workflow 실행 단계는 `workflow.workflow_stage`로 분리한다.

### 5.2 Schema hierarchy

```text
data-architecture.md
        ↓
content-object-design.md
        ↓
10_schemas/content-object.json
        ↓
runtime Content Object instances
```

`10_schemas`는 상위 문서에 없는 새로운 도메인 구조를 발명하지 않는다.

### 5.3 Null / Empty / Missing

- missing: 아직 생성되지 않았거나 계약상 제공하지 않는 값
- null: 필드 자체는 의미가 있으나 현재 값이 없음
- []: 배열 필드는 존재하지만 항목이 없음

이 세 상태를 임의로 동일시하지 않는다.
