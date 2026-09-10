# Content Object Design

> Version: 0.2.0\
> Status: Canonical Content Object Baseline\
> Parent: `02_architecture/data-architecture.md`\
> Related: `02_architecture/state-machine.md`,
> `02_architecture/internal-protocol.md`,
> `10_schemas/content-object.json`\
> Description: AI Factory에서 콘텐츠의 기획, 근거, 제작, 검증, 발행,
> 성과, 학습을 일관되게 추적하기 위한 핵심 데이터 모델

------------------------------------------------------------------------

## 0. 설계 방향

AI Factory에서 `Content Object`는 콘텐츠 생명주기의 중심 연결점으로
사용한다.

다만 모든 상세 데이터를 하나의 객체에 직접 포함하지 않고, 콘텐츠의
정체성, 현재 상태, 목적, 주요 참조, 요약 정보를 관리하는 **Aggregate
Root**로 정의한다.

실제 제작과 운영에 필요한 상세 데이터는 독립 객체로 분리한다.

``` text
Content Object
      │
      ├── Content Intent
      ├── Research Package
      ├── Claim Set
      ├── Script Version
      ├── Scene Plan
      ├── Asset Set
      ├── QA Record
      ├── Publication Record
      ├── Analytics Snapshot
      └── Learning Record
```

이 구조를 통해 콘텐츠의 전체 생명주기를 하나의 ID로 추적하면서도, 대본,
Asset, Claim, Evidence, QA, Analytics와 같은 데이터는 독립적으로 버전
관리하고 재사용할 수 있다.

이 문서에서는 이 구조를 **Content-Centric Lifecycle Model**이라고
부른다.

------------------------------------------------------------------------

# 1. Content Object의 정의

Content Object는 다음을 관리하는 최상위 객체다.

``` text
Content Object
= 하나의 콘텐츠 아이디어와 그 파생 제작물, 검증 결과, 발행 관계를
  동일한 콘텐츠 정체성 아래 연결하는 Aggregate Root
```

Content Object는 다음 질문에 답할 수 있어야 한다.

-   이 콘텐츠는 무엇인가?
-   왜 만들었는가?
-   누구를 위한 것인가?
-   현재 어느 상태인가?
-   어떤 근거를 사용하는가?
-   어떤 제작 산출물과 연결되어 있는가?
-   어떤 검증을 통과했는가?
-   어디에 발행되었는가?
-   어떤 성과를 냈는가?
-   무엇을 학습했는가?

Content Object는 모든 상세 데이터를 직접 보관하는 저장소가 아니라,
콘텐츠와 관련 객체를 연결하고 현재 상태를 요약하는 중심 객체다.

------------------------------------------------------------------------

# 2. 전체 구조

``` text
Content Object
│
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

실제 상세 데이터는 다음 독립 객체로 관리한다.

``` text
Content Object
│
├── Content Intent
├── Research Package
├── Evidence Records
├── Claim Records
├── Script Versions
├── Scene Plans
├── Asset Records
├── QA Records
├── Publication Records
├── Analytics Snapshots
├── Learning Records
└── Decision Records
```

------------------------------------------------------------------------

# 3. Identity

``` yaml
identity:
  content_id:
  canonical_key:
  parent_content_id:
  root_content_id:
  created_at:
  updated_at:
```

예:

``` yaml
identity:
  content_id: CNT-20260910-0001
  canonical_key: pain-history-first-treatment
  parent_content_id: null
  root_content_id: CNT-20260910-0001
  created_at: 2026-09-10T09:00:00+09:00
  updated_at: 2026-09-10T09:30:00+09:00
```

## 3.1 `content_id`

하나의 콘텐츠 정체성을 식별한다.

콘텐츠의 대본이나 Asset이 변경되어도 콘텐츠의 계보가 유지되는 한
`content_id`는 변경하지 않는다.

## 3.2 `canonical_key`

사람과 시스템이 이해하기 쉬운 안정적인 키다.

예:

``` text
pain-history-first-treatment
```

`content_id`는 시스템 식별자이고, `canonical_key`는 운영과 검색을 위한
의미 기반 키다.

## 3.3 `parent_content_id`

파생 콘텐츠의 원본 콘텐츠를 가리킨다.

예:

``` text
Long-form Content
      └── Short Content
```

## 3.4 `root_content_id`

콘텐츠 파생 계보 전체의 최상위 콘텐츠를 가리킨다.

이 구조를 통해 다음을 표현할 수 있다.

``` text
하나의 주제
      ├── YouTube Short
      ├── YouTube Long-form
      ├── Instagram Reel
      └── Blog Article
```

------------------------------------------------------------------------

# 4. Lifecycle

콘텐츠의 현재 상태와 상태별 진행 정보를 관리한다.

``` yaml
lifecycle:
  current_state:
  state_entered_at:
  state_version:
  blocked:
  blocked_reason:
  completion:
```

예:

``` yaml
lifecycle:
  current_state: SCRIPT_APPROVED
  state_entered_at: 2026-09-10T12:00:00+09:00
  state_version: 3
  blocked: false
  blocked_reason: null
  completion:
    topic: complete
    research: complete
    script: complete
    scenes: pending
    assets: pending
    qa: pending
    publication: pending
    learning: pending
```

자동화 실행 정보는 별도 영역으로 둔다.

``` yaml
workflow:
  workflow_id:
  workflow_version:
  active_run_id:
  retry_count:
  idempotency_key:
  last_error:
```

현재 상태와 자동화 실행 상태는 서로 다른 정보다.

``` text
current_state = 콘텐츠가 어느 단계에 있는가
active_run_id = 어떤 자동화 실행이 처리 중인가
retry_count = 몇 번 재시도했는가
```

이 둘을 분리하면 Workflow 장애나 재시도가 콘텐츠의 의미 있는 상태를 잘못
표현하지 않는다.

## 4.1 Canonical Content State

`lifecycle.current_state`는 **콘텐츠의 의미적 생명주기 상태(semantic
lifecycle state)** 를 나타낸다.

Foundation Freeze 기준으로 `current_state`에는 다음 12개 값만 사용한다.

``` text
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

이 값들은 `02_architecture/state-machine.md`에서 정의하는 **Canonical
Content State**와 동일해야 한다.

### Content State의 원칙

-   `current_state`는 콘텐츠가 의미적으로 어느 단계까지 도달했는지를
    표현한다.
-   상태 전이는 State Machine의 규칙을 따른다.
-   `current_state`는 자동화 실행의 세부 단계나 실행 결과를 표현하지
    않는다.
-   `FAILED`, `RETRYING`, `ERROR`, `CANCELLED`와 같은 실행/예외 상태는
    `current_state`의 값으로 사용하지 않는다.

## 4.2 Workflow Stage와의 분리

`workflow.workflow_stage`는 자동화가 **현재 어떤 실행 작업을 수행하고
있는지**를 나타낸다.

Workflow Stage의 전체 정의는 `02_architecture/service-flow.md`가
소유한다.

``` text
Content State
= 콘텐츠가 의미적으로 어디까지 완료되었는가

Workflow Stage
= 자동화가 지금 무엇을 실행하고 있는가
```

따라서 동일한 콘텐츠에 대해 다음과 같은 상태가 동시에 존재할 수 있다.

``` yaml
lifecycle:
  current_state: RESEARCHING

workflow:
  workflow_stage: RESEARCH
  retry_count: 2
```

Workflow가 실패하거나 재시도하더라도 `lifecycle.current_state`를 임의로
`FAILED` 등의 값으로 변경하지 않는다. 실행 실패, 재시도, 오류 원인은
Workflow 및 Audit 영역에서 기록한다.

------------------------------------------------------------------------

# 5. Classification

``` yaml
classification:
  category:
  format:
  language:
  series_id:
  topic_types:
  target_audience:
  channel_targets:
  sensitivity:
```

예:

``` yaml
classification:
  category: pain_history
  format: short
  language: ko
  series_id: SERIES-PAIN-HISTORY
  topic_types:
    - evergreen
    - series
  target_audience:
    - general_public
  channel_targets:
    - youtube_shorts
  sensitivity:
    medical: medium
    historical: medium
```

콘텐츠는 여러 분류 속성을 동시에 가질 수 있다.

``` text
evergreen + series + medical
```

따라서 `topic_types`는 단일 값이 아니라 배열로 관리한다.

------------------------------------------------------------------------

# 6. Intent

콘텐츠의 의도와 목적을 관리한다.

``` yaml
intent:
  title:
  working_title:
  hook:
  core_question:
  audience_problem:
  desired_response:
  objective:
  reason_selected:
  differentiation:
  recommendation_ref:
  priority:
```

예:

``` yaml
intent:
  title: 인류 최초의 통증은 무엇이었을까?
  working_title: 5000년 전에도 사람들은 아팠다
  hook: 인간은 언제부터 통증을 치료하려 했을까?
  core_question: 인류는 언제부터 통증을 치료하기 시작했는가?
  audience_problem: 통증의 역사를 단순한 의학 지식이 아니라 인간의 생존 문제로 이해하지 못한다.
  desired_response: 시청자가 통증을 역사적 관점에서 다시 생각하게 한다.
  objective: topic_validation
  reason_selected: 역사적 호기심과 통증이라는 보편적 경험을 결합할 수 있기 때문이다.
  differentiation: 통증의 역사를 인간 생존의 관점에서 설명한다.
  recommendation_ref: REC-20260910-003
  priority: high
```

콘텐츠 제작에서는 다음을 구분해야 한다.

``` text
왜 선택했는가?
무엇을 전달하려는가?
시청자가 어떤 반응을 하길 원하는가?
```

------------------------------------------------------------------------

# 7. References

Content Object는 상세 객체를 직접 포함하지 않고 참조한다.

``` yaml
references:
  research_package_refs:
    - RPK-20260910-001
  evidence_refs:
    - EVD-001
    - EVD-004
  claim_refs:
    - CLM-001
    - CLM-002
  decision_refs:
    - DEC-20260910-002
  experiment_refs:
    - EXP-20260910-001
```

참조 대상은 독립적으로 버전 관리하고 재사용할 수 있다.

------------------------------------------------------------------------

# 8. Current Outputs

현재 콘텐츠에 연결된 제작 산출물을 요약한다.

``` yaml
current_outputs:
  script_ref:
  script_version:
  scene_plan_ref:
  asset_set_ref:
  render_ref:
  thumbnail_ref:
  subtitle_ref:
```

예:

``` yaml
current_outputs:
  script_ref: SCR-20260910-001
  script_version: 0.3.0
  scene_plan_ref: SCP-20260910-001
  asset_set_ref: ASTSET-20260910-001
  render_ref: RND-20260910-001
  thumbnail_ref: THM-20260910-001
  subtitle_ref: SUB-20260910-001
```

대본, 장면, Asset은 여러 버전을 가질 수 있으므로 Content Object에는 현재
사용 중인 버전만 참조한다.

전체 버전 이력은 각 독립 객체에서 관리한다.

------------------------------------------------------------------------

# 9. Research Package

Research는 Content Object의 하위 필드가 아니라 독립 객체로 관리한다.

``` yaml
research_package:
  research_id:
  version:
  status:
  methodology:
  source_refs:
  evidence_refs:
  unresolved_questions:
  confidence:
  completed_at:
```

예:

``` yaml
research_package:
  research_id: RPK-20260910-001
  version: 1.0.0
  status: completed
  methodology:
    - primary_source_review
    - secondary_source_cross_check
  source_refs:
    - SRC-001
    - SRC-002
  evidence_refs:
    - EVD-001
    - EVD-004
  unresolved_questions:
    - 정확한 사용 시점은 추가 검증이 필요하다.
  confidence: high
  completed_at: 2026-09-10T10:30:00+09:00
```

------------------------------------------------------------------------

# 10. Claim과 Evidence

## 10.1 Claim

``` yaml
claim:
  claim_id:
  statement:
  claim_type:
  risk_level:
  confidence:
  evidence_refs:
  verification_status:
  allowed_usage:
```

예:

``` yaml
claim:
  claim_id: CLM-001
  statement: 고대 사회에서 특정 식물성 물질이 통증 완화에 사용되었다.
  claim_type: historical_medical
  risk_level: medium
  confidence: high
  evidence_refs:
    - EVD-001
    - EVD-004
  verification_status: verified
  allowed_usage:
    - educational_summary
```

## 10.2 Evidence

``` yaml
evidence:
  evidence_id:
  source_ref:
  excerpt:
  interpretation:
  reliability:
  verified_by:
  verified_at:
```

## 10.3 연결 원칙

``` text
Content Object
      ↓
Claim
      ↓
Evidence
      ↓
Source
```

대본과 장면은 Claim을 참조한다.

``` text
Script Segment
      ↓
Claim Ref
      ↓
Evidence Ref
```

Claim과 Evidence는 여러 콘텐츠에서 재사용될 수 있으므로 콘텐츠에
종속되지 않는 독립 객체로 관리한다.

------------------------------------------------------------------------

# 11. Script Version

대본은 독립적인 버전 객체다.

``` yaml
script:
  script_id:
  content_id:
  version:
  status:
  format:
  duration_target:
  segments:
  ending:
  brand_line:
  created_by:
  approved_by:
  created_at:
```

예:

``` yaml
script:
  script_id: SCR-20260910-001
  content_id: CNT-20260910-0001
  version: 0.3.0
  status: approved
  format: short
  duration_target: 45
  segments:
    - segment_id: SEG-001
      sequence: 1
      text: 인간은 언제부터 통증을 없애려 했을까?
      function: hook
      claim_refs: []
    - segment_id: SEG-002
      sequence: 2
      text: 고대 사회에서는 특정 식물성 물질이 통증 완화에 사용되었다.
      function: evidence
      claim_refs:
        - CLM-001
  ending: 통증을 알면, 아픔이 다르게 보입니다.
  brand_line: 통증의 역사를 이해하면 현재의 치료도 다르게 보입니다.
  created_by: AI
  approved_by: human
  created_at: 2026-09-10T11:00:00+09:00
```

대본은 세그먼트를 기본 단위로 사용한다.

이를 통해 다음을 지원할 수 있다.

-   문장별 Claim 연결
-   문장별 QA
-   문장별 수정 이력
-   장면과 대사의 정확한 연결
-   여러 대본 포맷으로의 변환

------------------------------------------------------------------------

# 12. Scene Plan

장면 계획은 대본과 Asset 사이의 변환 계층이다.

``` yaml
scene_plan:
  scene_plan_id:
  script_ref:
  version:
  scenes:
    - scene_id:
      sequence:
      duration:
      narration_segment_refs:
      visual_intent:
      claim_refs:
      asset_refs:
      transition:
      qa_status:
```

예:

``` yaml
scene_plan:
  scene_plan_id: SCP-20260910-001
  script_ref: SCR-20260910-001
  version: 0.2.0
  scenes:
    - scene_id: SC-001
      sequence: 1
      duration: 4
      narration_segment_refs:
        - SEG-001
      visual_intent: prehistoric human experiencing injury
      claim_refs: []
      asset_refs:
        - AST-001
      transition: hard_cut
      qa_status: pass
```

Scene은 대본 문장을 직접 복사하기보다 `narration_segment_refs`를
참조한다.

이를 통해 대본과 장면 사이의 텍스트 중복과 불일치를 줄인다.

------------------------------------------------------------------------

# 13. Asset Record와 Asset Set

Asset은 개별 파일이고, Asset Set은 특정 콘텐츠 제작에 사용된 Asset의
묶음이다.

## 13.1 Asset Record

``` yaml
asset:
  asset_id:
  asset_type:
  provider:
  generation_method:
  prompt_ref:
  source_ref:
  license:
  file_location:
  checksum:
  dimensions:
  duration:
  qa_status:
```

## 13.2 Asset Set

``` yaml
asset_set:
  asset_set_id:
  content_id:
  version:
  asset_refs:
  created_at:
```

하나의 Asset은 여러 콘텐츠에서 재사용될 수 있다.

``` text
Asset AST-001
      ├── Content A
      ├── Content B
      └── Content C
```

따라서 Asset 자체와 콘텐츠별 사용 묶음을 분리한다.

------------------------------------------------------------------------

# 14. QA Record

QA는 현재 상태의 요약과 상세 실행 기록을 분리한다.

## 14.1 Content Object의 QA 요약

``` yaml
quality_summary:
  overall_status:
  blocking_issues:
  required_human_review:
  latest_qa_record_ref:
  last_checked_at:
```

## 14.2 상세 QA Record

``` yaml
qa_record:
  qa_id:
  content_id:
  target_type:
  target_ref:
  qa_type:
  rule_version:
  status:
  reviewer:
  confidence:
  issues:
  checked_at:
```

예:

``` yaml
qa_record:
  qa_id: QA-20260910-001
  content_id: CNT-20260910-0001
  target_type: script
  target_ref: SCR-20260910-001
  qa_type: fact_check
  rule_version: QA-RULE-1.1.0
  status: pass
  reviewer: AI
  confidence: 0.94
  issues: []
  checked_at: 2026-09-10T11:30:00+09:00
```

QA 유형은 콘텐츠 유형과 규칙의 확장을 고려하여 일반화한다.

``` text
qa_type = fact_check
qa_type = medical_safety
qa_type = historical_accuracy
qa_type = brand_compliance
qa_type = asset_license
qa_type = video_render
qa_type = subtitle
```

------------------------------------------------------------------------

# 15. Publication Record

발행은 플랫폼별 독립 객체로 관리한다.

``` yaml
publication_record:
  publication_id:
  content_id:
  platform:
  status:
  scheduled_at:
  published_at:
  platform_content_id:
  title:
  description:
  tags:
  thumbnail_ref:
  synthetic_media_disclosure:
  publication_version:
```

예:

``` yaml
publication_record:
  publication_id: PUB-20260910-YT-001
  content_id: CNT-20260910-0001
  platform: youtube_shorts
  status: published
  scheduled_at: 2026-09-11T18:00:00+09:00
  published_at: 2026-09-11T18:00:00+09:00
  platform_content_id: YT-ABC123
  title: 5000년 전에도 사람들은 아팠다
  description: 인류는 언제부터 통증을 치료하려 했을까요?
  tags:
    - 통증
    - 의학의 역사
    - 역사
  thumbnail_ref: THM-20260910-001
  synthetic_media_disclosure: not_required
  publication_version: 1.0.0
```

하나의 콘텐츠가 여러 플랫폼에 발행될 수 있으므로 Publication은 플랫폼별
독립 레코드로 관리한다.

------------------------------------------------------------------------

# 16. Analytics Snapshot

Analytics는 시점별 Snapshot으로 저장한다.

``` yaml
analytics_snapshot:
  analytics_id:
  publication_ref:
  captured_at:
  observation_window:
  views:
  average_view_duration:
  average_view_percentage:
  retention:
  engagement:
  subscribers_gained:
  traffic_sources:
  platform_metric_version:
```

예:

``` yaml
analytics_snapshot:
  analytics_id: ANL-20260912-YT-001
  publication_ref: PUB-20260910-YT-001
  captured_at: 2026-09-12T09:00:00+09:00
  observation_window:
    start: 2026-09-11T18:00:00+09:00
    end: 2026-09-12T09:00:00+09:00
  views: 125000
  average_view_duration: 31.4
  average_view_percentage: 69.7
  retention:
    first_3_seconds: 0.82
    completion_rate: 0.61
  engagement:
    likes: 4200
    comments: 180
    shares: 310
  subscribers_gained: 540
  traffic_sources:
    shorts_feed: 0.88
    search: 0.07
    external: 0.05
  platform_metric_version: youtube-analytics-2026-01
```

Analytics는 시간이 지나면서 변하므로 현재 값만 덮어쓰지 않고 관측 시점을
보존한다.

------------------------------------------------------------------------

# 17. Learning Record

Learning은 Analytics를 해석한 결과다.

``` yaml
learning_record:
  learning_id:
  content_id:
  analytics_refs:
  baseline_ref:
  result:
  successful_elements:
  weak_elements:
  hypotheses:
  recommendations:
  confidence:
  created_at:
```

예:

``` yaml
learning_record:
  learning_id: LRN-20260915-001
  content_id: CNT-20260910-0001
  analytics_refs:
    - ANL-20260912-YT-001
  baseline_ref: BASELINE-SHORT-2026-Q3
  result: above_baseline
  successful_elements:
    - historical curiosity hook
    - fast opening
    - clear visual contrast
  weak_elements:
    - ending retention
  hypotheses:
    - stronger first 2 seconds may improve completion
  recommendations:
    - test question-based opening
    - shorten final brand line
  confidence: medium
  created_at: 2026-09-15T10:00:00+09:00
```

Learning은 다음 제작에 사용할 수 있는 의사결정 입력이어야 한다.

``` text
관측값       → Analytics
해석         → Learning
실행할 결정  → Decision
```

------------------------------------------------------------------------

# 18. Cost Summary

비용은 Content Object에 요약값만 저장하고, 상세 비용은 별도 기록으로
관리한다.

``` yaml
cost_summary:
  currency:
  total:
  by_category:
    research:
    llm:
    image:
    video:
    tts:
    render:
    storage:
  cost_record_ref:
```

비용은 Workflow 실행, API 호출, 재시도에 따라 여러 건 발생할 수 있으므로
상세 비용 기록과 분리한다.

------------------------------------------------------------------------

# 19. Provenance Summary

``` yaml
provenance_summary:
  source_refs:
  model_refs:
  prompt_refs:
  workflow_refs:
  template_refs:
  brand_version:
  qa_rule_versions:
  generated_by:
```

상세 Prompt와 모델 설정은 별도 객체로 관리한다.

``` text
Content Object
      ↓
Prompt Record
      ↓
Model Run
      ↓
Generated Artifact
```

Prompt와 Model Configuration은 여러 번 변경될 수 있고 민감한 설정을
포함할 수 있으므로 Content Object에 전문을 직접 넣지 않는다.

------------------------------------------------------------------------

# 20. Audit Summary

Audit은 Content Object에 전체 이벤트를 직접 넣기보다 요약과 최신 참조를
저장한다.

``` yaml
audit_summary:
  created_by:
  last_modified_by:
  last_action:
  event_count:
  latest_event_ref:
  immutable_log_ref:
```

상세 이벤트:

``` yaml
audit_event:
  event_id:
  content_id:
  timestamp:
  actor:
  action:
  from_state:
  to_state:
  target_ref:
  reason:
  run_id:
```

감사 로그는 삭제나 수정이 제한되어야 하므로 별도 불변 로그로 관리한다.

------------------------------------------------------------------------

# 21. Content Object 권장 최종 구조

``` yaml
identity:
  content_id:
  canonical_key:
  parent_content_id:
  root_content_id:
  created_at:
  updated_at:

lifecycle:
  current_state:
  state_entered_at:
  state_version:
  blocked:
  blocked_reason:
  completion:

classification:
  category:
  format:
  language:
  series_id:
  topic_types:
  target_audience:
  channel_targets:
  sensitivity:

intent:
  title:
  working_title:
  hook:
  core_question:
  audience_problem:
  desired_response:
  objective:
  reason_selected:
  differentiation:
  recommendation_ref:
  priority:

references:
  research_package_refs:
  evidence_refs:
  claim_refs:
  decision_refs:
  experiment_refs:

current_outputs:
  script_ref:
  script_version:
  scene_plan_ref:
  asset_set_ref:
  render_ref:
  thumbnail_ref:
  subtitle_ref:

quality_summary:
  overall_status:
  blocking_issues:
  required_human_review:
  latest_qa_record_ref:
  last_checked_at:

publication_summary:
  publication_refs:
  published_platforms:
  latest_publication_at:

performance_summary:
  analytics_refs:
  latest_snapshot_ref:
  baseline_comparison:
  last_measured_at:

learning_summary:
  learning_refs:
  latest_learning_ref:
  open_hypotheses:
  next_action_refs:

cost_summary:
  currency:
  total:
  by_category:
  cost_record_ref:

provenance_summary:
  source_refs:
  model_refs:
  prompt_refs:
  workflow_refs:
  template_refs:
  brand_version:
  qa_rule_versions:

workflow:
  workflow_id:
  workflow_version:
  active_run_id:
  retry_count:
  idempotency_key:
  last_error:

audit_summary:
  created_by:
  last_modified_by:
  last_action:
  event_count:
  latest_event_ref:
  immutable_log_ref:
```

------------------------------------------------------------------------

# 22. 전체 관계

``` text
                         CONTENT OBJECT
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
      Intent              Lifecycle            Classification
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
             Research Package       Decision
                    │                   │
             ┌──────┴──────┐            │
             │             │            │
          Evidence       Claims        │
             │             │            │
             └──────┬──────┘            │
                    ▼                   │
              Script Version            │
                    │                   │
                    ▼                   │
                Scene Plan              │
                    │                   │
                    ▼                   │
                 Asset Set              │
                    │                   │
                    ▼                   │
                QA Records              │
                    │                   │
                    ▼                   │
            Publication Records         │
                    │                   │
                    ▼                   │
            Analytics Snapshots         │
                    │                   │
                    ▼                   │
             Learning Records ──────────┘
                    │
                    ▼
              Next Decision
```

------------------------------------------------------------------------

# 23. 핵심 추적 경로

## 23.1 근거 추적

``` text
Script Segment
      ↓
Claim
      ↓
Evidence
      ↓
Source
```

## 23.2 제작 추적

``` text
Content Intent
      ↓
Script Version
      ↓
Scene Plan
      ↓
Asset Set
      ↓
Render
```

## 23.3 품질 추적

``` text
Artifact
      ↓
QA Record
      ↓
Blocking Issue
      ↓
Human Review
      ↓
Approval
```

## 23.4 성과 학습 추적

``` text
Publication
      ↓
Analytics Snapshot
      ↓
Learning Record
      ↓
Decision
      ↓
Next Content
```

------------------------------------------------------------------------

# 24. 상태와 객체의 관계

`02_architecture/state-machine.md`는 **Canonical Content State**가 어떤
상태로 이동할 수 있는지와 그 전이 규칙을 정의한다.

`Content Object.lifecycle.current_state`는 그 State Machine에 따라 현재
콘텐츠 상태를 저장한다.

`Content Object.workflow.workflow_stage`는 별도의 실행 계층으로서,
`02_architecture/service-flow.md`에 정의된 Workflow Stage를 저장한다.

즉:

``` text
State Machine
= 어떤 Content State 전이가 허용되는가

Content Object.lifecycle.current_state
= 현재 어떤 Canonical Content State인가

Content Object.workflow.workflow_stage
= 자동화가 현재 어떤 Workflow Stage를 실행 중인가

Artifact Object
= 해당 상태/실행에서 어떤 결과물이 생성되었는가

Audit Event
= 왜 그 상태 또는 실행 정보가 변경되었는가
```

``` text
State Machine
= 어떤 전이가 허용되는가

Content Object
= 현재 어떤 상태에 있는가

Artifact Object
= 해당 상태에서 어떤 결과물이 생성되었는가

Audit Event
= 왜 그 상태로 이동했는가
```

예:

``` text
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

상태는 현재 작업의 완료 여부를 표현하고, 산출물은 별도 객체로 참조한다.

QA 통과 여부는 상태 전이 조건으로 사용하며, Workflow 실행 실패와
재시도는 콘텐츠 상태와 별도로 기록한다.

------------------------------------------------------------------------

# 25. Content Object와 외부 객체의 경계

## Content Object에 직접 포함

-   콘텐츠 식별 정보
-   콘텐츠 계보 정보
-   현재 상태
-   콘텐츠 분류
-   제작 목적
-   핵심 질문과 Hook
-   주요 객체 참조
-   현재 사용 중인 산출물 버전
-   QA 요약
-   발행 요약
-   성과 요약
-   학습 요약
-   비용 요약
-   Provenance 요약
-   Audit 요약

## 외부 객체로 분리

-   원문 Source 전문
-   Evidence 전문
-   전체 Research 결과
-   Claim 상세 기록
-   대본 전체 버전 이력
-   장면 계획 전체 버전 이력
-   실제 이미지, 영상, 음성 파일
-   Prompt 전문
-   Model Configuration
-   전체 Analytics 시계열
-   상세 QA 실행 기록
-   불변 Audit Log
-   Secret과 API Credential

------------------------------------------------------------------------

# 26. 파일 시스템 구조

Git Repository에서는 다음과 같이 구성한다.

``` text
07_content/
└── CNT-20260910-0001/
    ├── content-object.yaml
    ├── intent.yaml
    ├── references.yaml
    ├── research/
    │   └── RPK-20260910-001.yaml
    ├── claims/
    │   ├── CLM-001.yaml
    │   └── CLM-002.yaml
    ├── scripts/
    │   ├── SCR-20260910-001-v0.1.0.yaml
    │   ├── SCR-20260910-001-v0.2.0.yaml
    │   └── SCR-20260910-001-v0.3.0.yaml
    ├── scenes/
    │   └── SCP-20260910-001-v0.2.0.yaml
    ├── assets/
    │   └── asset-set.yaml
    ├── qa/
    │   ├── QA-20260910-001.yaml
    │   └── QA-20260910-002.yaml
    ├── publication/
    │   └── PUB-20260910-YT-001.yaml
    ├── analytics/
    │   ├── ANL-20260912-YT-001.yaml
    │   └── ANL-20260915-YT-001.yaml
    ├── learning/
    │   └── LRN-20260915-001.yaml
    └── audit/
        └── audit-index.yaml
```

이 구조는 다음을 명확히 한다.

-   어떤 파일이 어떤 객체인가
-   어떤 버전인가
-   어떤 콘텐츠에 속하는가
-   어떤 객체를 참조하는가
-   어떤 파일이 현재 활성 버전인가

------------------------------------------------------------------------

# 27. JSON Schema 방향

`10_schemas/content-object.json`은 Content Object의 중심 구조만
검증한다.

권장 구조:

``` json
{
  "$id": "https://ai-factory.local/schemas/content-object.json",
  "type": "object",
  "required": [
    "identity",
    "lifecycle",
    "classification",
    "intent",
    "references",
    "current_outputs"
  ],
  "properties": {
    "identity": {
      "$ref": "#/$defs/identity"
    },
    "lifecycle": {
      "$ref": "#/$defs/lifecycle"
    },
    "classification": {
      "$ref": "#/$defs/classification"
    },
    "intent": {
      "$ref": "#/$defs/intent"
    },
    "references": {
      "$ref": "#/$defs/references"
    },
    "current_outputs": {
      "$ref": "#/$defs/currentOutputs"
    },
    "quality_summary": {
      "$ref": "#/$defs/qualitySummary"
    },
    "publication_summary": {
      "$ref": "#/$defs/publicationSummary"
    },
    "performance_summary": {
      "$ref": "#/$defs/performanceSummary"
    },
    "learning_summary": {
      "$ref": "#/$defs/learningSummary"
    },
    "cost_summary": {
      "$ref": "#/$defs/costSummary"
    },
    "provenance_summary": {
      "$ref": "#/$defs/provenanceSummary"
    },
    "workflow": {
      "$ref": "#/$defs/workflow"
    },
    "audit_summary": {
      "$ref": "#/$defs/auditSummary"
    }
  }
}
```

각 하위 객체는 별도 Schema로 관리한다.

``` text
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

객체별 Schema를 분리하면 각 객체의 변경 범위를 제한하고, Workflow별
검증과 재사용 가능한 객체의 독립 검증을 지원할 수 있다.

------------------------------------------------------------------------

# 28. Progressive Completion

초기 구현에서는 모든 필드를 채우지 않는다.

다만 필드가 비어 있는 것과 필드가 아직 생성되지 않은 것을 구분해야 한다.

``` text
missing = 아직 생성되지 않음
null    = 해당 없음
empty   = 생성되었지만 값이 없음
```

예:

``` yaml
publication_summary:
  publication_refs: []
  published_platforms: []
  latest_publication_at: null
```

이는 다음을 의미한다.

``` text
발행 객체가 아직 없음
```

반면 다음은 다르다.

``` yaml
publication_summary:
  publication_refs:
    - PUB-20260910-YT-001
  published_platforms:
    - youtube_shorts
  latest_publication_at: 2026-09-11T18:00:00+09:00
```

권장 완료 단계:

``` text
Topic Approved
→ identity + lifecycle + classification + intent

Research Completed
→ research refs + claim refs + evidence refs

Script Approved
→ current_outputs.script_ref

Scene Planned
→ current_outputs.scene_plan_ref

Assets Ready
→ current_outputs.asset_set_ref

QA Passed
→ quality_summary

Published
→ publication_summary

Analytics Ready
→ performance_summary

Learning Completed
→ learning_summary
```

------------------------------------------------------------------------

# 29. 불변 원칙

### Rule 1 --- Content ID는 콘텐츠 계보 전체에서 안정적이어야 한다.

단순한 대본 수정 때문에 Content ID를 변경하지 않는다.

### Rule 2 --- 산출물 버전과 콘텐츠 버전을 구분한다.

대본 버전이 변경되어도 Content Object의 정체성은 유지한다.

### Rule 3 --- 현재 참조와 과거 이력을 분리한다.

Content Object는 현재 활성 버전을 가리키고, 전체 이력은 독립 객체에
보관한다.

### Rule 4 --- Claim과 Evidence는 재사용 가능해야 한다.

콘텐츠에 종속되지 않는 근거는 독립 객체로 관리한다.

### Rule 5 --- Script Segment는 Claim을 참조할 수 있어야 한다.

대본 전체가 아니라 문장 단위의 근거 추적을 지원한다.

### Rule 6 --- Scene은 Script Segment를 참조한다.

대본과 장면의 텍스트 중복을 최소화한다.

### Rule 7 --- Asset은 콘텐츠와 독립적으로 식별한다.

동일 Asset을 여러 콘텐츠에서 재사용할 수 있어야 한다.

### Rule 8 --- Publication은 플랫폼별로 분리한다.

하나의 콘텐츠가 여러 플랫폼에 발행될 수 있음을 기본값으로 한다.

### Rule 9 --- Analytics는 Snapshot으로 저장한다.

현재 값만 덮어쓰지 않고 관측 시점을 보존한다.

### Rule 10 --- Learning은 Analytics와 분리한다.

관측값, 해석, 실행 결정을 구분한다.

### Rule 11 --- QA 상세 기록은 삭제하지 않는다.

실패와 수정 이력도 품질 데이터다.

### Rule 12 --- Workflow 실패는 콘텐츠 상태와 분리한다.

자동화 실행 실패가 콘텐츠의 의미 있는 상태를 임의로 변경하지 않도록
한다.

### Rule 13 --- 외부 입력은 데이터로 취급한다.

Trend, Research, Source의 텍스트는 실행 명령이 아니라 검증 대상
데이터다.

### Rule 14 --- Secret은 Content Object에 저장하지 않는다.

API Key, Credential, Token은 별도 Secret Manager에서 관리한다.

------------------------------------------------------------------------

# 30. 사람이 이해해야 하는 핵심 구조

모든 필드를 외울 필요는 없다.

다음 구조만 이해하면 된다.

``` text
CONTENT
 │
 ├── 무엇을 만들 것인가?       → Intent
 ├── 왜 만드는가?              → Objective / Strategy
 ├── 무엇을 근거로 하는가?     → Claim / Evidence
 ├── 어떻게 말하는가?          → Script
 ├── 어떻게 보여주는가?        → Scene Plan / Asset Set
 ├── 검증되었는가?             → QA Record
 ├── 어디에 발행되었는가?      → Publication Record
 ├── 어떤 성과를 냈는가?       → Analytics Snapshot
 └── 무엇을 배웠는가?           → Learning Record
```

가장 중요한 추적 구조는 다음과 같다.

``` text
Script Segment
      ↓
Claim
      ↓
Evidence
      ↓
Source
```

``` text
Script Segment
      ↓
Scene
      ↓
Asset
```

``` text
Publication
      ↓
Analytics
      ↓
Learning
      ↓
Decision
```

------------------------------------------------------------------------

# 31. 최종 설계 결론

AI Factory의 Content Object는 다음과 같이 정의한다.

> **Content Object는 하나의 콘텐츠 아이디어와 그 파생 제작물, 검증 결과,
> 발행 관계, 성과 학습을 동일한 콘텐츠 정체성 아래 연결하는 Aggregate
> Root다.**

이 정의를 채택하는 이유는 다음과 같다.

-   콘텐츠의 전체 생명주기를 하나의 ID로 추적할 수 있다.
-   콘텐츠와 제작 산출물의 수명주기를 분리할 수 있다.
-   대본, 장면, Asset, Claim, Evidence를 독립적으로 버전 관리할 수 있다.
-   재사용 가능한 데이터와 콘텐츠 전용 데이터를 구분할 수 있다.
-   하나의 콘텐츠에서 여러 플랫폼과 포맷으로 파생되는 구조를 표현할 수
    있다.
-   Analytics 시계열과 Learning 결과를 분리해 보존할 수 있다.
-   QA와 Audit을 확장 가능한 구조로 관리할 수 있다.
-   Git과 DB 양쪽에서 동일한 모델을 사용할 수 있다.
-   n8n과 Dify가 특정 단계부터 부분적으로 재실행될 수 있다.

따라서 Content Object에는 정체성, 현재 상태, 목적, 주요 참조, 현재
산출물, 요약 정보를 저장하고, 상세 데이터는 독립 객체로 관리한다.

``` text
Content Object
= 정체성 + 현재 상태 + 목적 + 참조 + 요약

Supporting Objects
= 조사 + 근거 + 대본 + 장면 + Asset + QA + 발행 + 분석 + 학습
```

### v0.2.0 Foundation Freeze 변경 범위

v0.2.0은 Content Object의 Aggregate Root 구조나 Supporting Object 구조를
변경하지 않는다.

이번 버전에서 확정하는 핵심 계약은 다음과 같다.

1.  `lifecycle.current_state`는 12개의 Canonical Content State만
    사용한다.
2.  Workflow Execution Stage는 `workflow.workflow_stage`로 분리한다.
3.  Workflow 실패/재시도/오류는 Content State가 아니라
    Workflow/Audit에서 관리한다.
4.  State Machine은 Canonical Content State의 전이 규칙을 정의한다.
5.  Workflow 실행 순서는 `service-flow.md`가 정의한다.
6.  `content-object.json`은 이 계약을 Schema 수준에서 검증한다.

이 구조를 기준으로 다음 Schema를 구현한다.

``` text
1. content-object.json
2. claim.json
3. evidence.json
4. script.json
5. scene-plan.json
6. asset.json
7. qa-record.json
8. publication-record.json
9. analytics-snapshot.json
10. learning-record.json
```

가장 먼저 검증해야 할 핵심 경로는 다음이다.

``` text
Claim
 → Evidence
 → Script Segment
 → Scene
 → Asset
 → QA
 → Publication
 → Analytics
 → Learning
```

이 경로가 안정적으로 작동하면 AI Factory는 단순한 콘텐츠 생성 자동화가
아니라, 근거를 추적하고 결과를 학습하며 다음 의사결정으로 연결하는
콘텐츠 운영 시스템이 된다.
