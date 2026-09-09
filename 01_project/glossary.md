# 기묘한 통증도감 AI Factory Glossary
> Version: 0.1.0
> Status: Initial Glossary Baseline
> Parent: `01_project/project-charter.md`
> Description: 프로젝트 전체에서 사용하는 핵심 용어의 공통 정의


## A

### AI Factory
주제 발견부터 제작, QA, 예약 발행, 분석, 학습까지 콘텐츠 생명주기를 자동화하는 전체 시스템.

### Asset
영상 제작에 사용되는 이미지·영상·음성·음악·효과음·자막 등 개별 제작 자산.

### Asset QA
생성 또는 수집된 자산의 시각·기술·브랜드·라이선스 적합성을 검사하는 단계.

### Audit Trail
추천·승인·연구·생성·QA·발행·오류·재시도·분석 결과를 추적할 수 있는 기록.

## C

### Claim
콘텐츠에서 사실로 주장하거나 설명하는 개별 명제. 의료·역사 콘텐츠는 가능한 한 Claim 단위로 근거를 연결한다.

### Content Object
하나의 콘텐츠를 제작·검수·발행·분석하기 위한 구조화된 데이터 단위.

### Content Cannibalization
유사한 주제·관점의 콘텐츠가 서로의 노출과 관심을 잠식하는 현상.

### Confidence Score
특정 주장이나 판단에 대한 근거의 신뢰 수준을 수치 또는 등급으로 표현한 값.

## D

### Dify
지식 연결, RAG, AI Workflow 및 reasoning을 담당하는 시스템 구성요소.

### Dead-letter / Error Queue
자동 재시도로 해결되지 않은 작업을 별도로 보관하여 사람의 확인을 받는 영역.

## E

### Evidence Gate
특정 Claim이 최종 콘텐츠에 포함되기 전에 출처와 근거 수준을 확인하는 검증 관문.

### Evidence Ledger
Claim, Source, Evidence, 해석, 최종 표현 사이의 연결을 기록하는 구조.

## F

### Fact Check
콘텐츠의 사실관계를 출처와 비교하여 검증하는 과정.

## H

### Human Review
자동 시스템이 단독 판단하지 않고 사람이 최종 확인하는 검토 단계.

## I

### Idempotency
같은 작업이 여러 번 실행되어도 중복 생성·업로드·발행이 발생하지 않도록 보장하는 특성.

## L

### Long-form
기묘한 통증도감의 5–10분 내외 장편 콘텐츠. 초기에는 성과가 검증된 Shorts의 확장을 우선한다.

### License Gate
외부 또는 생성 자산의 사용권, 이용조건, Attribution 필요 여부를 확인하는 관문.

## M

### Medical Claim Ledger
의학적 Claim별 출처·근거 수준·대상·한계·위험도·허용 표현을 기록하는 구조.

### Medical Risk Level
- **Level 0:** 일반 건강/역사 정보
- **Level 1:** 자가관리·운동·스트레칭 등
- **Level 2:** 질환·약물·시술·응급·치료 효과 등 고위험 내용

### Machine Runtime
AI Factory가 작업을 수행하는 실제 경과 시간.

## N

### n8n
Trigger, 분기, 병렬 실행, Retry, 외부 서비스 연결, Scheduling을 담당하는 Workflow Orchestrator.

### Notion
Dashboard, 승인, 운영 현황 등 Human-facing Project Management를 담당하는 시스템.

## P

### Prompt Version
특정 생성 결과에 사용된 Prompt의 버전. 재현성과 품질 변화 추적을 위해 기록한다.

### Provenance
Claim 또는 Asset이 어떤 출처와 생성 과정을 거쳐 최종 결과가 되었는지 추적할 수 있는 계보 정보.

## Q

### QA Gate
다음 단계로 이동하기 전에 정해진 품질 조건을 검사하는 관문.

## R

### Retry
일시적 오류 또는 재생성이 가능한 실패에 대해 동일 작업을 다시 수행하는 과정.

## S

### Semantic Duplicate Check
제목이 아니라 Topic, Claim, Audience, Angle, Format의 의미적 유사성을 비교하는 중복 검사.

### Shorts
기본 35–60초, 목표 약 45초로 설계하는 단편 영상 포맷.

### Source Provenance
정보가 어느 출처에서 수집되고 어떤 해석을 거쳐 콘텐츠에 사용되었는지 나타내는 정보.

### Synthetic Media
AI로 생성 또는 의미 있게 변경된 이미지·영상·음성 등의 미디어.

## T

### Trend
현재 관심도 또는 상승 가능성이 있는 주제. Breaking, Seasonal, Rising 등으로 분류할 수 있다.

### Trend Source Registry
트렌드 수집에 사용하는 외부 Source, 수집 방법, 신뢰도, 이용조건, Rate Limit 등을 관리하는 목록.

### Topic Recommendation
Trend와 채널 전략을 분석하여 제작 후보로 제안하는 구조화된 추천.

## V

### Video QA
최종 렌더링 영상의 콘텐츠·시청성·기술·브랜드 품질을 확인하는 단계.

### Voice QA
TTS 음성의 발음·속도·자연스러움·대본 일치·타이밍을 검사하는 단계.

## 상태 용어

### DRAFT
아직 확정되지 않은 콘텐츠 또는 설계.

### PENDING_CONFIRMATION
사용자의 승인 대기 상태.

### APPROVED
사용자가 제작을 승인한 상태.

### IN_PRODUCTION
제작 파이프라인이 진행 중인 상태.

### READY
QA를 통과하여 발행 가능한 상태.

### SCHEDULED
예약 발행 정보가 확정된 상태.

### PUBLISHED
실제 발행이 완료된 상태.

### FAILED
자동 처리에 실패하여 복구 또는 Human Review가 필요한 상태.

### CANCELLED
제작 또는 발행을 중단한 상태.

## 프로젝트 핵심 원칙

```text
Traceability
Safety
Reliability
Automation
Human Override
Learning
```
