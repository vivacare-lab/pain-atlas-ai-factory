# Decision Log
> Version: 0.1.0
> Status: Initial Decision Baseline
> Parent: `01_project/project-charter.md`
> Description: 주요 설계·운영 결정과 결정 근거를 기록

## 0. 이 문서를 왜 만드는가?

AI Factory를 구축하면서 내려진 중요한 설계·운영 결정을 기록하여, 시간이 지나도 **왜 이런 구조를 선택했는지** 추적할 수 있게 한다.

AI가 생성한 문서나 코드가 계속 수정되더라도 원래의 의사결정 배경을 보존하고, 같은 논의를 반복하지 않으며, 향후 구조를 변경할 때 기존 결정과의 충돌 여부를 판단하기 위한 문서다.

## 1. 이 문서에서 반드시 이해해야 하는 것

Decision Log는 '무엇을 만들었는가'를 기록하는 문서가 아니라 **'왜 그렇게 결정했는가'를 기록하는 문서**다.

특히 다음을 남긴다.

- 문제 또는 의사결정이 필요했던 이유
- 검토한 선택지
- 최종 결정
- 결정의 근거
- 포기한 대안과 그 이유
- 예상되는 trade-off
- 영향을 받는 문서/시스템
- 나중에 재검토해야 할 조건

모든 사소한 결정까지 기록하지 않는다. 구조, 비용, 안전, 운영, 자동화 수준, 데이터, 품질 기준처럼 이후 설계에 영향을 주는 결정이 대상이다.

## 2. AI Factory에서 이 문서가 담당하는 역할

Decision Log는 AI Factory의 **설계 기억(Design Memory)** 역할을 한다.

Git이 파일의 변경 이력을 보존한다면, Decision Log는 그 변경의 **의사결정 맥락**을 보존한다.

예를 들어 나중에 누군가 다음과 같이 질문할 수 있다.

> 왜 Notion을 원본 데이터베이스로 사용하지 않았는가?
> 왜 Shorts를 먼저 검증한 뒤 Long-form으로 확장하는가?
> 왜 의료/역사적 Claim에 Evidence Ledger를 요구하는가?
> 왜 모든 자동화 결과를 바로 발행하지 않고 Human Review를 두는가?

이 문서는 이러한 질문에 답한다.

## 3. 실제 시스템에서는 어떻게 사용되는가?

### 3.1 새로운 중요한 결정을 내릴 때

```text
Problem
  ↓
Options
  ↓
Evaluation
  ↓
Decision
  ↓
Record in Decision Log
  ↓
Update affected documents
```

### 3.2 기존 결정을 변경할 때

기존 결정을 삭제하지 않는다.

```text
Decision D-001
    ↓
Reconsideration Trigger
    ↓
New Evaluation
    ↓
Decision D-014
    ↓
D-001 = Superseded
```

즉, 결정의 역사를 남긴다.

### 3.3 AI가 새로운 구조를 제안할 때

AI의 제안은 곧바로 시스템 규칙이 되지 않는다.

```text
AI Proposal
   ↓
Human Evaluation
   ↓
Decision
   ↓
Decision Log
   ↓
Architecture / Workflow / Schema Update
```

## 4. 구현 세부사항

### 4.1 권장 Decision ID

```text
D-001
D-002
D-003
...
```

ID는 변경하지 않는다.

### 4.2 기본 기록 형식

```markdown
## D-XXX — 결정 제목

- Status: Accepted / Superseded / Rejected / Deprecated
- Date: YYYY-MM-DD
- Decision Owner: Human / System Owner
- Related Documents:
  - path/to/document.md

### Context
왜 이 결정을 내려야 했는가?

### Options Considered
1. Option A
2. Option B
3. Option C

### Decision
무엇을 선택했는가?

### Rationale
왜 선택했는가?

### Trade-offs
무엇을 얻고 무엇을 포기했는가?

### Consequences
이 결정으로 어떤 시스템/운영 변화가 발생하는가?

### Reconsider When
어떤 조건이 발생하면 이 결정을 다시 검토할 것인가?
```

## 5. 현재까지의 핵심 결정

### D-001 — Git을 Source of Truth로 사용

- Status: Accepted
- Date: 2026-09-09
- Decision Owner: Human / System Owner

#### Context
AI Factory의 설계 문서와 규칙이 여러 도구에 흩어지면 변경 이력과 원본 기준을 추적하기 어렵다.

#### Decision
Git을 프로젝트의 **설계·규칙·스키마·프롬프트·워크플로우 원본 및 버전 관리 계층**으로 사용한다.

Notion은 사람이 보는 운영·승인 중심의 관리 계층으로 사용한다.

#### Rationale
버전 관리, 변경 이력, diff, rollback, 협업에 유리하며 AI가 생성한 산출물도 구조적으로 관리할 수 있다.

#### Trade-offs
Git 사용에 익숙하지 않은 운영자는 접근성이 낮을 수 있으므로 Notion을 운영 인터페이스로 둔다.

#### Reconsider When
팀 규모와 운영 방식이 크게 변경되어 별도의 configuration management 또는 governance 시스템이 필요해질 경우.

### D-002 — 콘텐츠 제작은 상태 기반 Pipeline으로 운영

- Status: Accepted
- Date: 2026-09-09
- Decision Owner: Human / System Owner

#### Context
콘텐츠 제작 과정에서 일부 단계가 실패하거나 사람이 검토해야 하는 경우 단순한 순차 자동화만으로는 복구와 추적이 어렵다.

#### Decision
콘텐츠에 명시적인 상태(state)를 부여하고, 각 상태 사이의 transition 조건과 실패/복구 경로를 정의한다.

#### Rationale
현재 위치, 완료 조건, 실패 지점, 재시도 및 Human Review를 명확하게 관리할 수 있다.

#### Trade-offs
초기 설계와 데이터 구조가 단순한 workflow보다 복잡해진다.

#### Reconsider When
콘텐츠 규모가 매우 커져 별도의 distributed workflow engine이 필요해질 경우.

### D-003 — Evidence / Claim Provenance를 핵심 데이터로 관리

- Status: Accepted
- Date: 2026-09-09
- Decision Owner: Human / System Owner

#### Context
의료 및 역사 콘텐츠는 단순한 텍스트 생성보다 주장의 근거와 출처를 추적할 수 있어야 한다.

#### Decision
핵심 Claim마다 Source와 Evidence를 연결하고, 최종 Script까지 provenance를 추적할 수 있도록 한다.

```text
Claim → Source → Evidence → AI Interpretation → Final Script
```

#### Rationale
의료 안전, 역사적 사실검증, 수정 가능성, 감사 추적성을 확보하기 위해 필요하다.

#### Trade-offs
Research와 Fact Check에 추가적인 시간과 API 비용이 발생한다.

#### Reconsider When
콘텐츠 유형이 근거 추적이 거의 필요 없는 순수 오락형 콘텐츠로 확장되는 경우에는 risk tier에 따라 적용 수준을 차등화할 수 있다.

### D-004 — Human Review는 제거하지 않고 Risk-based로 운영

- Status: Accepted
- Date: 2026-09-09
- Decision Owner: Human / System Owner

#### Context
AI 자동화의 목표를 '사람이 전혀 개입하지 않는 시스템'으로 정의하면 의료·역사·저작권·합성 미디어 등 고위험 영역에서 오류가 그대로 실행될 수 있다.

#### Decision
일상적인 저위험 작업은 자동화하되, 다음과 같은 경우 Human Review를 요구한다.

- 의료 위험도가 높은 Claim
- 핵심 출처 간 충돌
- 낮은 confidence
- breaking event
- 저작권/라이선스 불명확성
- 실제 인물/장소를 사실적으로 합성한 고위험 Asset
- 반복적인 QA 실패
- 시스템이 정의한 기타 고위험 조건

#### Rationale
자동화율과 안전성을 동시에 확보하기 위해서다.

#### Trade-offs
완전 자동화보다 Human Time이 더 필요하다.

#### Reconsider When
QA 정확도와 risk control이 장기간 검증되고, 특정 저위험 영역에 대해 Human Review를 안전하게 축소할 근거가 확보될 경우.

### D-005 — Shorts를 Topic Validation 계층으로 사용

- Status: Accepted
- Date: 2026-09-09
- Decision Owner: Human / System Owner

#### Context
초기 채널에서는 모든 주제를 Long-form으로 제작하는 것보다 작은 비용으로 관심도를 검증할 필요가 있다.

#### Decision
Shorts를 주요 Topic Validation 수단으로 사용하고, 성과가 확인된 주제와 evergreen/history 후보를 Long-form 확장 대상으로 검토한다.

#### Rationale
제작 비용과 학습 주기를 줄이면서 실제 시청자 반응을 확보할 수 있다.

#### Trade-offs
Shorts 성과가 Long-form 성과를 완전히 예측하지는 못한다.

#### Reconsider When
Long-form의 독립적인 수요 예측 모델이나 충분한 채널 데이터가 확보될 경우.

## 6. 이 문서의 운영 원칙

1. 중요한 결정은 기록한다.
2. 결정과 구현을 구분한다.
3. AI의 제안과 확정된 시스템 규칙을 구분한다.
4. 기존 결정을 수정할 때 삭제하지 않고 supersede한다.
5. 결정에는 재검토 조건을 남긴다.
6. Decision Log가 Architecture 문서와 불일치하면 어느 쪽이 최신 결정인지 확인한다.
7. 비용, 안전, 품질, 자동화 수준에 영향을 주는 결정은 우선적으로 기록한다.
