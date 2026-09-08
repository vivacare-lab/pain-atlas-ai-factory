# 기묘한 통증도감 AI Factory

## Project Charter v0.1.0

> **Status:** Final Review Candidate
>
> **Version:** 0.1.0
>
> **Project:** 기묘한 통증도감 AI Factory
>
> **Repository:** `pain-atlas-ai-factory`
>
> **Document:** `01_project/project-charter.md`

.

# 1. Project Overview

**기묘한 통증도감 AI Factory**는
의학·통증·건강·의료 역사 콘텐츠를 지속적으로 생산하고 운영하기 위한 **AI 기반 콘텐츠 자동화 시스템**이다.

최종 목표는 단순한 영상 자동 생성이 아니라,

> **주제 발견 → 검증 → 콘텐츠 제작 → 품질관리 → 예약 발행 → 성과분석 → 학습**

의 전체 생명주기를 하나의 시스템으로 연결하는 것이다.

기본 운영 흐름은 다음과 같다.

```text
DAILY TRIGGER
    ↓
TREND DISCOVERY
    ↓
TOPIC ANALYSIS
    ↓
TOPIC RECOMMENDATION
    ↓
HUMAN CONFIRMATION
    ↓
RESEARCH
    ↓
FACT CHECK
    ↓
MEDICAL SAFETY CHECK
    ↓
CONTENT PLANNING
    ↓
SCRIPT
    ↓
BRAND CHECK
    ↓
SCENE / ASSET PLANNING
    ↓
IMAGE / VIDEO GENERATION
    ↓
VOICE GENERATION
    ↓
RENDER
    ↓
ASSET / VIDEO QA
    ↓
SCHEDULE
    ↓
YOUTUBE PUBLISH
    ↓
ANALYTICS
    ↓
LEARNING
    ↓
NEXT TOPIC RECOMMENDATION
```

---

# 2. Channel Identity

## 2.1 Channel Name

**기묘한 통증도감**

## 2.2 Core Theme

```text
Pain
+ Medicine
+ Medical History
+ Human History
+ Everyday Health
```

단순한 건강정보 채널이 아니라,

> **“인간은 통증을 어떻게 경험했고, 어떻게 이해했으며, 어떻게 없애려고 했는가?”**

를 중심으로 의학과 역사, 현대의 건강정보를 연결한다.

## 2.3 Channel Brand Statement

> **통증의 역사는 인간이 아픔을 이해해 온 역사입니다.**

## 2.4 Video Brand Line

> **통증을 알면, 아픔이 다르게 보입니다.**

Video Brand Line은 영상의 마지막 또는 적절한 전환 지점에서 사용하는 짧은 브랜드 장치이며, 모든 영상에서 기계적으로 반복할 필요는 없다.

---

# 3. Content Scope

주요 콘텐츠 영역은 다음과 같다.

### A. 통증의 역사

- 고대의 통증 치료
- 역사 속 진통제
- 마취의 역사
- 수술과 통증
- 전쟁과 외상 치료
- 전통 의학과 통증
- 의료기술의 발전

### B. 통증 지식

- 통증의 종류
- 통증 전달 과정
- 신경과 뇌
- 급성통증과 만성통증
- 통증의 원리
- 흔히 오해하는 통증

### C. 이상하고 흥미로운 통증

- 특이한 통증 증후군
- 설명하기 어려운 통증
- 역사적으로 기록된 특이한 통증
- 통증에 대한 인간의 기묘한 경험

### D. 생활 건강

- 일반적인 통증 관리
- 올바른 스트레칭
- 생활습관
- 통증 관련 자가관리
- 의료기관 방문이 필요한 신호

단, 생활 건강 콘텐츠는 의료 안전 기준을 적용한다.

### E. 의료 Myth / Fact

- 잘못 알려진 통증 상식
- 민간요법의 사실관계
- 의학적 오해
- 과학적으로 확인된 사실과 추정의 구분

---

# 4. Content Format

## 4.1 Shorts

기본 길이:

> **35–60초**

표준 목표:

> **약 45초**

YouTube의 플랫폼상 최대 길이에 맞추는 것이 아니라, 채널의 정보 밀도와 시청 유지율을 고려한 전략적 기본값으로 45초를 사용한다.

### 기본 구조

```text
HOOK
↓
QUESTION / PROBLEM
↓
CORE INFORMATION
↓
EXAMPLE / HISTORICAL CASE
↓
MODERN INTERPRETATION
↓
WARNING / LIMITATION
↓
BRAND LINE
```

모든 영상이 동일한 구조를 강제적으로 따르지는 않는다.

---

## 4.2 Long-form

초기 목표:

> **5–10분**

Long-form은 초기부터 독립적인 주제 생산라인으로 운영하기보다,

```text
Shorts
↓
Performance Analysis
↓
High-Potential Topic
↓
Long-form Expansion
```

방식으로 운영한다.

즉 Shorts를 **주제 검증 장치**로 활용한다.

---

# 5. Audience

주요 대상:

- 통증과 건강정보에 관심 있는 일반 시청자
- 의학 상식을 쉽고 흥미롭게 배우고 싶은 사람
- 의료 역사에 관심 있는 사람
- 의학적 호기심이 많은 시청자
- 짧고 강한 지식 콘텐츠를 선호하는 시청자

전문 의료인만을 대상으로 하지 않는다.

따라서 전문적인 의학적 정확성을 유지하되, 최종 표현은 일반 시청자가 이해할 수 있어야 한다.

---

# 6. Content Principles

콘텐츠 우선순위는 다음과 같다.

```text
FACTUALITY
    >
MEDICAL SAFETY
    >
RELIABILITY
    >
BRAND CONSISTENCY
    >
INTEREST
    >
VIRALITY
```

조회수보다 사실성과 안전성을 우선한다.

---

## 6.1 Topic Recommendation

AI Factory는 매일 수집된 트렌드와 기존 콘텐츠를 분석하여 **3–5개의 주제 후보**를 추천한다.

각 추천에는 최소한 다음 정보가 포함되어야 한다.

```text
Topic
Why Now
Channel Relevance
Content Angle
Expected Format
Risk Level
Source Summary
Duplicate Check
Recommendation Score
```

사용자는 다음 중 하나를 선택한다.

```text
APPROVE
REJECT
REQUEST_REVISION
DEFER
```

승인되지 않은 주제는 제작 파이프라인으로 자동 진입하지 않는다.

---

# 7. Topic Strategy

주제는 다음 세 가지 축으로 관리한다.

## 7.1 TREND

```text
Breaking
Seasonal
Rising
```

## 7.2 EVERGREEN

```text
Pain Knowledge
Pain History
Self-care
Medical Myth
```

## 7.3 SERIES

예:

```text
통증의 역사
이상한 통증
인류 최초의 치료법
의사가 설명하는 통증
```

초기 Shorts 편성의 예:

```text
1. Trend
2. Evergreen
3. Trend
4. Series
```

Long-form은:

```text
Shorts에서 검증된 주제
+
Evergreen / History 주제
```

를 중심으로 구성한다.

---

# 8. Medical Safety Principles

의료 관련 콘텐츠는 일반 정보 제공을 목적으로 하며 진단이나 개인별 치료를 대체하지 않는다.

다음 표현을 원칙적으로 금지한다.

```text
확실히 이 병입니다.
이 방법이면 반드시 낫습니다.
이것만 하면 치료됩니다.
무조건 효과가 있습니다.
병원에 갈 필요 없습니다.
```

대신 다음과 같은 표현을 사용한다.

```text
가능성이 있습니다.
일부 연구에서는 보고되었습니다.
상황에 따라 다릅니다.
증상이 지속되면 의료진의 평가가 필요합니다.
정확한 진단은 의료진의 평가가 필요합니다.
```

---

## 8.1 Medical Risk Level

### LEVEL 0 — General Information

예:

- 통증의 역사
- 해부학적 기본 지식
- 일반적인 통증 개념
- 역사적 의료 이야기

→ 기본 자동 검수 가능

### LEVEL 1 — Self-care

예:

- 스트레칭
- 운동
- 마사지
- 자세
- 생활습관
- 일반적인 자가관리

→ 강화된 의료 안전 검수

### LEVEL 2 — High Risk

예:

- 질환
- 약물
- 치료
- 시술
- 응급상황
- 특정 치료 효과
- 금기사항
- 의학적 판단이 필요한 내용

→ **엄격한 Evidence Gate + Human Review 또는 동등한 승인 절차**

---

## 8.2 Medical Claim Ledger

의학적 사실은 문장 단위의 Claim으로 관리한다.

최소 필드:

```text
claim_id
claim
source
source_type
evidence_level
publication_date
population
limitations
risk_level
approved_expression
```

즉,

> **“대본 전체가 그럴듯한가?”가 아니라 “각 의학적 주장 하나하나가 근거가 있는가?”**

를 검증한다.

---

# 9. Historical Fact-Checking Principles

역사 콘텐츠는 특히 **“최초”라는 표현을 엄격하게 관리**한다.

다음 표현은 자동으로 강화된 역사 검증 대상으로 분류한다.

```text
최초
가장 오래된
인류 최초
세계 최초
최초의 치료
최초의 진통제
최초로 발견
처음 사용
```

---

## 9.1 Historical Evidence Gate

```text
Historical Claim
↓
Source Identification
↓
Primary / Secondary Classification
↓
Date Verification
↓
Context Verification
↓
Interpretation Check
↓
Confidence Score
↓
Approved Wording
```

---

## 9.2 Historical Source Principle

우선순위:

```text
Primary Source
>
High-quality Scholarly Secondary Source
>
Reliable Reference
>
General Secondary Source
```

출처가 불분명한 블로그나 콘텐츠를 역사적 사실의 최종 근거로 사용하지 않는다.

“전해진다”, “~라고 알려져 있다”와 실제 사료에 기록된 사실을 구분한다.

인물·인용문·유물·연도·사건을 임의로 생성하지 않는다.

---

# 10. Visual Identity

## 10.1 General Style

기본 스타일:

> **3D Illustration / Stylized Medical Visualization**

Photorealism보다 다음 방향을 선호한다.

```text
Clean
Cinematic
Medical
Historical
Slightly Mysterious
Premium
```

---

## 10.2 Historical Scene

```text
Historically grounded environment
+
Stylized 3D visualization
+
Modern anatomical visualization where appropriate
```

---

## 10.3 Modern Medical Scene

```text
Clean
Clinical
Minimal
Modern 3D
Easy anatomical visualization
```

---

## 10.4 Visual Restrictions

다음은 원칙적으로 제한한다.

- 과도한 고어
- 불필요한 수술 장면
- 실제 환자처럼 보이는 합성 이미지
- 해부학적 오류
- 잘못된 의료기구
- 시대와 맞지 않는 역사적 소품
- 이미지 내부의 불필요한 텍스트
- 사실처럼 오인될 수 있는 가짜 역사적 자료

---

# 11. AI Factory Goal

AI Factory의 핵심 목표는 **무인에 가까운 콘텐츠 운영 구조**를 만드는 것이다.

최종적으로는:

```text
Daily Trigger
↓
Trend Discovery
↓
Topic Recommendation
↓
Human Approval
↓
Automated Production
↓
Automated QA
↓
Scheduled Publishing
↓
Analytics
↓
Learning
```

구조를 지향한다.

초기에는 사람의 개입을 적극적으로 유지하고, 시스템이 안정화될수록 저위험 영역의 자동화를 확대한다.

---

# 12. Production Volume

초기 목표:

> **주 4 Shorts + 주 2 Long-form**

이를 초기 안정화 기준으로 한다.

시스템이 안정화된 이후 단계적으로 확대한다.

### Phase A

```text
4 Shorts + 2 Long-form / week
```

### Phase B

```text
5–7 Shorts + 2 Long-form / week
```

### Phase C

```text
1 Short / day
+
2–3 Long-form / week
```

### Phase D

```text
Multi-channel
+
Multi-format
```

생산량 증가는 품질과 시스템 안정성을 확인한 후 진행한다.

---

# 13. Production Time

생산시간은 두 가지로 분리한다.

## 13.1 Human Time

사용자가 실제로 개입하는 시간.

초기 목표:

```text
Shorts: < 5–10 min / piece
Long-form: < 15–20 min / piece
```

## 13.2 Machine Runtime

AI Factory가 실제 작업을 수행하는 시간.

초기 목표:

```text
Shorts: 20–30 min
Long-form: 40–60 min
```

장기적으로 Shorts의 wall-clock runtime을 **10–15분 수준**까지 단축하는 것을 목표로 한다.

단, **“1시간 이내 완성”은 초기 절대 조건이 아니다.**

속도 개선의 핵심은 단순히 더 빠른 AI를 사용하는 것이 아니라:

```text
Parallel Processing
+
Template Reuse
+
Asset Reuse
+
Prompt / Model Optimization
+
Caching
+
Batch Processing
```

이다.

n8n은 가능한 작업을 병렬화하여 실행하는 Orchestrator 역할을 한다.

---

# 14. Core Architecture

전체 시스템은 다음 역할 분리를 따른다.

```text
Git
= Memory / Source of Truth / Version Control

Notion
= Project Management / Dashboard / Approval Center

Dify
= Knowledge / RAG / AI Reasoning

n8n
= Orchestration / Execution / Scheduling

OpenAI
= Reasoning / Structured Generation / QA

fal.ai
= Image / Video Generation

ElevenLabs
= Voice Generation

Creatomate
= Video Assembly / Rendering

YouTube API
= Publishing / Scheduling

Analytics
= Performance Measurement / Learning
```

핵심 철학:

> **Git은 기억하고, Notion은 관리하고 승인받고, Dify는 생각하고, n8n은 수집·조율·실행하고, YouTube API는 실제 발행한다.**

---

# 15. State Machine

기본 상태는 다음과 같다.

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

실패 시:

```text
GENERATE
→ QA
→ FAIL
→ REPAIR / REGENERATE
→ QA
→ PASS
```

각 단계는 가능한 한 독립적으로 재실행할 수 있어야 한다.

---

# 16. Human Intervention

초기에는 다음 영역에 사람의 승인을 요구한다.

```text
Topic Approval
High-risk Medical Content
Ambiguous Historical Claims
Copyright / License Ambiguity
Conflicting Evidence
Breaking News
High-risk Synthetic Media
Repeated QA Failure
```

시스템 안정화 이후에는 LEVEL 0 콘텐츠부터 자동화 범위를 확대한다.

---

# 17. Source Provenance & Evidence Ledger

모든 중요한 콘텐츠는 다음 연결관계를 유지해야 한다.

```text
Claim
↓
Source
↓
Evidence
↓
AI Interpretation
↓
Final Script
```

즉, 최종 대본에서 특정 주장이 어디에서 왔는지 역추적할 수 있어야 한다.

이 구조는 의료 검증뿐 아니라 역사 검증, 수정, 감사, 재사용에도 활용한다.

---

# 18. Copyright & Asset License Gate

외부 자료 및 생성 자산은 다음 정보를 관리한다.

```text
Asset
Source
License
Usage Permission
Attribution Required?
Approved
```

원칙:

> **인터넷에서 찾았다는 이유만으로 사용 가능한 자료라고 판단하지 않는다.**

이미지·영상·음원·폰트·외부 자료의 라이선스 상태를 가능한 한 자동 검증하고, 불명확하면 Human Review 대상으로 보낸다.

---

# 19. AI Disclosure Gate

AI로 생성되거나 의미 있게 변경된 콘텐츠 중 플랫폼 정책상 공개가 필요한 경우를 판별한다.

Content Object에 관련 상태를 기록한다.

```text
synthetic_media
disclosure_required
disclosure_status
```

YouTube 업로드 단계에서 필요한 메타데이터를 반영한다.

---

# 20. Semantic Duplicate Check

단순한 제목 중복 검사는 사용하지 않는다.

다음 요소를 기준으로 의미적 중복을 판단한다.

```text
Topic
+
Claim
+
Audience
+
Angle
+
Format
```

예:

```text
허리 통증
허리 아플 때
허리 통증 원인
허리가 아픈 이유
```

처럼 표현만 다른 유사 콘텐츠는 **Content Cannibalization** 위험을 검사한다.

---

# 21. Trend Source Registry

트렌드 수집원은 임의로 계속 추가하지 않고 Registry로 관리한다.

각 Source에는 최소한:

```text
source_id
source_name
source_type
collection_method
url / endpoint
update_frequency
reliability
license / terms
rate_limit
allowed_use
last_checked
status
```

를 기록한다.

트렌드 수집은 해당 서비스의 API, 이용약관, robots 정책, 저작권 및 개인정보 관련 요구사항을 준수한다.

---

# 22. Untrusted Source / Prompt Injection Defense

외부에서 수집한 트렌드 페이지와 문서는 **신뢰할 수 없는 데이터**로 취급한다.

외부 문서에 포함된 지시문을 AI의 실행 명령으로 해석하지 않는다.

원칙:

```text
External Content
≠
System Instruction
```

검색 결과나 문서 내부의 프롬프트 인젝션을 방지하기 위해 데이터와 명령을 구조적으로 분리한다.

---

# 23. Audit Trail

각 Content Object는 다음을 추적할 수 있어야 한다.

```text
왜 이 주제가 추천되었는가
누가 승인했는가
어떤 출처를 사용했는가
어떤 모델이 생성했는가
어떤 Prompt Version을 사용했는가
어떤 QA를 통과했는가
몇 번 재생성되었는가
언제 예약되었는가
언제 발행되었는가
어떤 성과를 냈는가
어떤 학습 결과가 다음 콘텐츠에 반영되었는가
```

---

# 24. Configuration / Model / Prompt Versioning

콘텐츠 결과만 저장하지 않고 생성 환경도 추적한다.

최소한 다음을 기록한다.

```text
model
model_version
prompt_version
workflow_version
template_version
brand_version
qa_rule_version
config_version
```

이를 통해 향후 모델 변경이나 Prompt Drift가 콘텐츠 품질에 미치는 영향을 분석할 수 있어야 한다.

---

# 25. Security

API Key, OAuth Token, Secret 및 개인정보 등 민감한 인증정보는 Git에 저장하지 않는다.

원칙:

```text
Git
≠
Secret Storage
```

대신:

```text
n8n Credentials
Environment Secrets
Secret Manager
```

등의 방식으로 관리한다.

---

# 26. Failure Recovery & Idempotency

모든 주요 Workflow는 실패 후 재실행 가능해야 한다.

특히:

```text
Upload
Schedule
Publish
Payment / API Call
Asset Generation
```

등에서 중복 실행을 방지한다.

각 작업에는 가능한 경우:

```text
idempotency_key
retry_count
last_error
workflow_run_id
```

등을 기록한다.

반복 실패 시 일반 retry loop를 계속 돌리지 않고 Error Queue / Dead-letter 영역으로 이동시킨다.

---

# 27. Test & Deployment Policy

초기에는 실제 공개 발행보다 테스트를 우선한다.

```text
Development
↓
Private / Unlisted Upload
↓
Integration Test
↓
QA
↓
Production
↓
Scheduled Public Publishing
```

YouTube API의 인증·업로드·예약발행 및 프로젝트 상태에 따른 제한사항을 구현 단계에서 별도로 검증한다.

---

# 28. Analytics Learning

성과 판단을 조회수 하나로 결정하지 않는다.

주요 지표:

```text
Views
Average View Duration
Average View Percentage
Audience Retention
Completion
Likes
Comments
Shares
Subscribers Gained
Subscriber Conversion
```

Shorts의 플랫폼상 조회수 집계 방식 변화도 고려하여 단일 조회수 중심의 평가를 피한다.

---

## 28.1 Content Performance Score

초기 개념:

```text
Topic Score
+
Hook Score
+
Retention Score
+
Completion Score
+
Engagement Score
+
Subscriber Conversion
```

성과 데이터는 향후:

```text
Trend Recommendation
Topic Selection
Hook Design
Content Format
Long-form Candidate
```

에 다시 반영한다.

---

# 29. Cost & Unit Economics

콘텐츠별 비용을 추적한다.

```text
Research Cost
LLM Cost
Image Cost
Video Cost
TTS Cost
Render Cost
Storage Cost
```

이를 통해:

```text
Cost / Short
Cost / Long-form
Cost / 1,000 Views
Cost / Subscriber
```

를 계산한다.

자동화 확대 여부는 단순 제작속도뿐 아니라 **콘텐츠당 경제성**을 기준으로 판단한다.

---

# 30. Publishing Queue Health

가능하다면 일정량의 준비된 콘텐츠를 확보한다.

권장 장기 목표:

> **7–14일의 Ready / Scheduled 콘텐츠 버퍼**

단, 초기에는 실제 생산능력과 운영 안정성을 확인한 후 적용한다.

버퍼가 부족하거나 과도하게 쌓이는 경우 이를 운영 지표로 기록한다.

---

# 31. Content Object

모든 콘텐츠의 기본 데이터 모델은 다음 구조를 사용한다.

```json
{
  "content_id": "",
  "topic": "",
  "topic_source": {
    "type": "",
    "source_urls": [],
    "collected_at": ""
  },
  "recommendation": {
    "reason": "",
    "channel_relevance_score": 0,
    "trend_score": 0,
    "risk_level": "",
    "duplicate_check": "",
    "status": "PENDING_CONFIRMATION"
  },
  "confirmation": {
    "status": "PENDING",
    "confirmed_by": "",
    "confirmed_at": "",
    "note": ""
  },
  "format": "shorts",
  "target_duration_sec": 45,
  "state": "RECEIVED",
  "version": "1.0",

  "research": {},
  "claims": [],
  "script": {},
  "scenes": [],
  "assets": [],
  "voice": {},
  "render": {},
  "qa": {},

  "publish": {
    "status": "",
    "scheduled_at": "",
    "published_at": ""
  },

  "analytics": {}
}
```

향후 구현 단계에서 다음 필드를 추가한다.

```text
provenance
license
synthetic_media
model_version
prompt_version
workflow_version
idempotency
cost
audit_log
```

---

# 32. QA Gates

모든 콘텐츠는 단계별 QA를 통과해야 한다.

```text
Trend Source QA
↓
Topic Relevance QA
↓
Research QA
↓
Historical Fact QA
↓
Medical Safety QA
↓
Brand QA
↓
Asset QA
↓
Video QA
↓
Schedule QA
↓
Publishing QA
```

QA 결과는 단순 PASS/FAIL만 저장하지 않는다.

```text
PASS
FAIL
WARNING
HUMAN_REVIEW_REQUIRED
```

등의 상태를 사용할 수 있도록 설계한다.

---

# 33. Human Review Trigger

다음 조건에서는 자동 승인하지 않는다.

```text
Medical Risk Level 2
Historical "First" Claim
Conflicting Sources
Low Evidence Confidence
Breaking News
Copyright Ambiguity
Synthetic Media Risk
Repeated QA Failure
Unusual Risk Score
```

---

# 34. Self-Correction

AI Factory는 오류 발생 시 전체 파이프라인을 처음부터 다시 실행하지 않는다.

예:

```text
Script QA FAIL
→ Script Repair
→ Script QA
→ PASS
```

또는

```text
Image QA FAIL
→ Image Regeneration
→ Asset QA
→ PASS
```

방식으로 최소 단위의 재작업을 수행한다.

---

# 35. Decision Management

주요 설계 결정은 별도 기록한다.

```text
Decision
Reason
Alternatives
Trade-offs
Date
Version
```

파일:

```text
05_decisions/decision-log.md
```

채택하지 않은 중요한 아이디어 역시 기록한다.

```text
05_decisions/rejected-ideas.md
```

이를 통해 동일한 논의를 반복하지 않는다.

---

# 36. Experiment Management

AI 모델, Prompt, Hook, Thumbnail, 영상 길이, 시각 스타일, 업로드 시간 등은 실험 대상으로 관리한다.

실험에는 최소한:

```text
Hypothesis
Variable
Control
Experiment
Metric
Result
Decision
```

을 기록한다.

---

# 37. Roadmap

### Phase 0 — Foundation

- Git Repository
- Project Charter
- Brand Bible
- Data Model
- Decision Log
- Basic Documentation

### Phase 1 — Trend Discovery

- Trend Source Registry
- Trend Collection
- Topic Analysis
- Recommendation Engine

### Phase 2 — Human Confirmation

- Approval Workflow
- Notion Dashboard
- Approve / Reject / Revise / Defer

### Phase 3 — AI Content Core

- Research
- Claim Extraction
- Script Generation
- Content Object

### Phase 4 — Validation

- Medical QA
- Historical QA
- Evidence Ledger
- Safety Gate

### Phase 5 — Visual

- Scene Planning
- Prompt Generation
- Image / Video Generation
- Asset QA

### Phase 6 — Voice

- Voice Script
- TTS
- Voice QA

### Phase 7 — Rendering

- Creatomate
- Subtitle
- Template
- Automated Assembly

### Phase 8 — Video QA

- Duration
- Audio
- Subtitle
- Scene
- Brand
- Technical QA

### Phase 9 — Publishing

- YouTube API
- Upload
- Scheduling
- Disclosure
- Publishing Verification

### Phase 10 — Learning

- Analytics
- Performance Scoring
- Topic Learning
- Format Learning
- Cost Optimization
- Automated Recommendation Improvement

---

# 38. Repository Structure

```text
pain-atlas-ai-factory/
│
├── README.md
├── CHANGELOG.md
│
├── 01_project/
│   ├── project-charter.md
│   ├── roadmap.md
│   └── glossary.md
│
├── 02_architecture/
│   ├── system-architecture.md
│   ├── service-flow.md
│   ├── internal-protocol.md
│   ├── state-machine.md
│   └── data-architecture.md
│
├── 03_brand/
│   ├── brand-bible.md
│   ├── visual-style.md
│   └── content-format.md
│
├── 04_knowledge/
│   ├── medical/
│   ├── history/
│   ├── research/
│   ├── safety/
│   └── trends/
│
├── 05_decisions/
│   ├── decision-log.md
│   ├── option-register.md
│   └── rejected-ideas.md
│
├── 06_experiments/
│   └── experiments.md
│
├── 07_content/
│   ├── trends/
│   ├── recommendations/
│   ├── topics/
│   ├── scripts/
│   ├── claims/
│   ├── scenes/
│   └── prompts/
│
├── 08_qa/
│   ├── qa-protocol.md
│   ├── trend-qa.md
│   ├── medical-qa.md
│   ├── historical-qa.md
│   └── video-qa.md
│
├── 09_workflows/
│   ├── n8n/
│   │   ├── daily-trend-discovery/
│   │   ├── topic-recommendation/
│   │   ├── confirmation-handler/
│   │   ├── content-production/
│   │   └── scheduled-publishing/
│   │
│   └── dify/
│
├── 10_schemas/
│   ├── trend-object.json
│   ├── topic-recommendation.json
│   ├── content-object.json
│   ├── scene-object.json
│   ├── schedule-object.json
│   └── qa-result.json
│
└── 11_integrations/
    ├── trend-sources.md
    ├── openai.md
    ├── fal.md
    ├── elevenlabs.md
    ├── creatomate.md
    └── youtube.md
```

---

# 39. Non-Goals

본 프로젝트는 초기 단계에서 다음을 목표로 하지 않는다.

- 완전한 무인 운영을 즉시 달성하는 것
- 모든 의료 콘텐츠를 자동 승인하는 것
- 조회수만을 극대화하는 것
- 모든 트렌드를 수집하는 것
- 모든 AI 서비스를 동시에 사용하는 것
- 최초부터 Multi-channel을 운영하는 것
- 인간 검토를 완전히 제거하는 것
- 품질보다 생산량을 우선하는 것

---

# 40. Success Criteria

초기 시스템은 다음을 충족해야 한다.

## Topic Pipeline

```text
Daily Trend Collection
→ Relevance Analysis
→ 3–5 Recommendations
→ Human Confirmation
```

## Content Pipeline

```text
Approved Topic
→ Research
→ Claims
→ Script
→ Fact Check
→ Medical Safety
→ Scene
→ Assets
→ Voice
→ Render
→ QA
→ Schedule
→ Publish
```

## Quality

- 주요 의학적 주장에 근거가 존재한다.
- 역사적 주장에 출처가 존재한다.
- “최초” 등 고위험 표현이 별도 검증된다.
- 의료 위험도가 높은 콘텐츠는 Human Review를 거친다.
- 저작권 및 자산 라이선스 상태를 확인한다.
- 생성 콘텐츠의 필요한 AI disclosure를 처리한다.
- 최종 영상의 기술적 QA를 수행한다.

## Operation

초기 목표:

> **주 4 Shorts + 주 2 Long-form**

사용자의 실제 개입시간을:

```text
Shorts < 5–10 min / piece
Long-form < 15–20 min / piece
```

수준으로 줄이는 것을 목표로 한다.

장기적으로 자동화율과 생산량을 단계적으로 확대한다.

---

# 41. Core Design Philosophy

이 프로젝트의 핵심은 단순히 AI에게 콘텐츠를 만들게 하는 것이 아니다.

> **AI가 무엇을 만들지 발견하고, 왜 만들어야 하는지 판단하고, 근거를 확보하고, 안전성을 검증하고, 제작하고, 발행하고, 결과를 학습하는 시스템을 만드는 것**

이다.

따라서 모든 자동화는 다음 원칙을 따른다.

```text
Automation
without
Traceability
= Dangerous

Automation
without
Validation
= Unreliable

Automation
without
Human Override
= Fragile
```

최종적으로 지향하는 구조는:

```text
AUTOMATION
+
EVIDENCE
+
SAFETY
+
TRACEABILITY
+
HUMAN OVERRIDE
+
LEARNING
```

이다.

---

# 42. Current Architecture Principle

최종 역할 분리는 다음을 기본 원칙으로 한다.

```text
Git
→ 기억한다.

Notion
→ 관리한다.
→ 승인받는다.

Dify
→ 지식을 연결한다.
→ 추론한다.

n8n
→ 수집한다.
→ 조율한다.
→ 실행한다.
→ 예약한다.

OpenAI
→ 분석한다.
→ 생성한다.
→ 검증한다.

fal.ai
→ 시각 자산을 생성한다.

ElevenLabs
→ 음성을 생성한다.

Creatomate
→ 영상을 조립하고 렌더링한다.

YouTube API
→ 영상을 발행하고 예약한다.

Analytics
→ 결과를 측정하고 다음 의사결정에 반영한다.
```

---

# 43. Document Status

본 문서는 AI Factory의 상위 설계 기준 문서이다.

다음 문서들은 본 Charter가 확정된 후 이를 기준으로 작성한다.

```text
01_project/
├── roadmap.md - 작성 완료
└── glossary.md - 작성 완료

02_architecture/
├── system-architecture.md - 작성 완료
├── service-flow.md - 작성 완료
├── internal-protocol.md - ②작성 예정
├── state-machine.md - ③작성 예쩡
└── data-architecture.md - ①작성 예정
```

이후:

```text
03_brand - ⑤
04_knowledge - ⑥
05_decisions
06_experiments
07_content
08_qa - ⑦
09_workflows - ⑧
10_schemas - ④`/*.json` 작성 예정
11_integrations
```

순으로 구체화한다.

---

# 44. Version

```text
Document: project-charter.md
Version: 0.1.0
Status: Final Review Candidate
```

다음 버전에서는 프로젝트의 실제 구현 결과에 따라:

```text
v0.2.x
→ Architecture Refinement

v0.3.x
→ Implementation Refinement

v1.0.0
→ Production Baseline
```

으로 승격한다.
