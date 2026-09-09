# 기묘한 통증도감 AI Factory Experiments

> Version: 0.1.0
> Status: Initial Experiment Framework
> Parent: `01_project/project-charter.md`
> Description: 불확실한 설계·운영 가설을 작은 실험으로 검증하고 결과를 다음 의사결정에 연결

## 0. 이 문서를 왜 만드는가?

AI Factory의 모든 설계가 처음부터 정답일 수는 없다. 이 문서는 **아직 확신할 수 없는 가설을 실제 데이터와 제한된 범위의 실험으로 검증**하기 위한 공통 프레임워크를 정의한다.

실험의 목적은 '무언가를 해보는 것'이 아니라, 다음 의사결정에 사용할 수 있는 근거를 만드는 것이다.

## 1. 이 문서에서 반드시 이해해야 하는 것

- 실험은 기능 개발과 다르다. **불확실성을 줄이기 위한 활동**이다.
- 실험 전에 가설과 성공 기준을 정의한다.
- 한 번에 너무 많은 변수를 바꾸지 않는다.
- 실패한 실험도 유효한 지식이다.
- 실험 결과는 `decision-log.md`, `option-register.md`, `rejected-ideas.md`의 후속 결정으로 연결한다.
- 채널 성과 실험에서는 조회수 하나만 보지 않고 유지율, 완주율, 참여, 구독 전환, 제작 비용 등을 함께 본다.

## 2. AI Factory에서 이 문서가 담당하는 역할

Experiments는 **Decision → Experiment → Data → Learning → Decision**의 순환을 담당한다.

주요 실험 영역은 다음과 같다.

1. **Topic** — 주제 유형·각도·제목 전략
2. **Script** — Hook, 길이, 정보 밀도, 서술 방식
3. **Visual** — 이미지 스타일, 장면 수, 반복 사용 가능 자산
4. **Voice** — TTS 음성, 속도, 감정, 발화 밀도
5. **Production** — 생성·QA·렌더링 시간과 비용
6. **Publishing** — 업로드 시간, 빈도, 예약 전략
7. **Automation** — 자동화율, 실패율, Human Review 빈도

## 3. 실제 시스템에서는 어떻게 사용되는가?

### 3.1 실험 수명주기

```text
IDEA / UNCERTAINTY
→ HYPOTHESIS
→ EXPERIMENT DESIGN
→ BASELINE
→ TEST
→ MEASURE
→ ANALYZE
→ DECISION
→ LEARNING
```

### 3.2 실험 객체

각 실험은 최소한 다음 정보를 가진다.

```yaml
experiment_id: EXP-001
title: Hook 문장 유형 비교
question: 어떤 Hook이 초기 이탈을 줄이는가?
hypothesis: 질문형 Hook이 설명형 Hook보다 초기 유지율이 높을 것이다.
variable: hook_type
baseline: 설명형
test: 질문형
success_metric: average_view_percentage
safety_constraints: 의료적 과장 표현 금지
status: planned
result: null
decision: null
```

### 3.3 실험 결과의 연결

결과가 유의미하면 Decision Log에 반영하고, 새로운 표준이 되면 Brand/Workflow/QA 문서의 규칙으로 승격한다. 결과가 부정적이면 Rejected Ideas 또는 실험 기록에 남겨 같은 실험을 반복하지 않는다.

## 4. 구현 세부사항

### 4.1 실험 원칙

- **One primary question**: 하나의 실험은 핵심 질문 하나를 우선한다.
- **Defined baseline**: 비교 기준을 명시한다.
- **Controlled variables**: 가능한 한 다른 조건을 고정한다.
- **Minimum viable experiment**: 큰 자동화를 만들기 전에 작은 범위로 검증한다.
- **Reproducibility**: 모델, 프롬프트, 브랜드 버전, 템플릿, 데이터 기간을 기록한다.
- **Stop rule**: 비용·시간·품질 저하가 일정 수준을 넘으면 중단한다.

### 4.2 주요 지표

| 영역 | 예시 지표 | 목적 |
|---|---|---|
| Content | 평균 시청 지속시간 | 실제 시청량 평가 |
| Content | 평균 시청 비율 | 영상 길이를 고려한 유지력 평가 |
| Shorts | 완주율/유지율 | Hook 및 전개 평가 |
| Engagement | 좋아요·댓글·공유 | 반응 평가 |
| Growth | 구독 전환 | 채널 성장 기여도 |
| Production | 제작 시간 | Human Time 절감 |
| Cost | 콘텐츠당 비용 | 단위경제성 평가 |
| Reliability | 실패율/재시도율 | 자동화 안정성 평가 |

### 4.3 실험과 안전

의료·역사 콘텐츠는 조회수 상승만을 이유로 안전 기준을 낮추는 실험을 허용하지 않는다. 의료 위험이 높은 주장, 역사적 '최초' 주장, 출처 충돌, 저작권·라이선스 불명확성은 실험 대상이 아니라 **Gate/검토 대상**으로 분리한다.

### 4.4 실험 결과의 판정

```text
PASS
→ 표준화 후보

INCONCLUSIVE
→ 추가 데이터 필요

FAIL
→ 현재 가설 폐기 또는 수정

BLOCKED
→ 안전/비용/기술 제약으로 실행 중단
```

실험 결과가 곧바로 시스템 규칙이 되는 것은 아니다. 반복 검증과 비용·안전·운영 가능성을 확인한 뒤 공식 Decision으로 승격한다.

## 5. 실험 기록 템플릿

```markdown
# EXP-XXX — Experiment Title

## Purpose
무엇을 알아내기 위한 실험인가?

## Question
핵심 질문은 무엇인가?

## Hypothesis
어떤 결과를 예상하는가?

## Baseline
현재 기준은 무엇인가?

## Variables
무엇을 바꾸고 무엇을 고정하는가?

## Success Criteria
어떤 결과면 채택할 것인가?

## Safety / Quality Constraints
어떤 조건은 절대 희생하지 않는가?

## Result
실험 결과는 무엇인가?

## Decision
채택 / 수정 / 보류 / 폐기

## Follow-up
다음 실험 또는 문서 변경은 무엇인가?
```

## 6. 버전 및 변경 관리

이 문서의 `Version`은 **실험 프레임워크 자체가 변경될 때** 올린다. 개별 실험의 생성·수정은 `experiment_id`와 실험별 기록으로 추적한다.

예:

```text
experiments.md v0.1.0
 ├── EXP-001
 ├── EXP-002
 └── EXP-003
```

문서의 구조·판정 기준·핵심 정책이 변경되면 문서 버전을 갱신하고, 단순히 실험 하나를 추가했다고 문서 버전을 매번 올리지는 않는다.

## 7. Related Documents

- `01_project/roadmap.md`
- `05_decisions/decision-log.md`
- `05_decisions/option-register.md`
- `05_decisions/rejected-ideas.md`
- `03_brand/content-format.md`
- `08_qa/qa-protocol.md`
