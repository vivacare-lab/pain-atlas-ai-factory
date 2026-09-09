# Option Register
> Version: 0.1.0
> Status: Initial Option Register Baseline
> Parent: `01_project/project-charter.md`
> Description: 주요 선택지와 검토·선택 근거를 기록

## 0. 이 문서를 왜 만드는가?

AI Factory를 설계하고 운영하면서 하나의 문제에 여러 가지 선택지가 생긴다. 이 문서는 중요한 선택지를 기록하고, 현재 선택된 방향과 보류된 대안을 비교할 수 있게 하기 위해 만든다.

단순한 아이디어 목록이 아니라, **결정 전의 선택지와 판단 근거를 보존하는 설계 기록**이다.

## 1. 이 문서에서 반드시 이해해야 하는 것

- 모든 아이디어를 즉시 채택할 필요는 없다.
- 선택하지 않은 대안도 미래에는 유효할 수 있다.
- 현재의 최선과 절대적인 정답은 다르다.
- 옵션을 비교할 때 비용, 품질, 안정성, 자동화 수준, 운영 복잡도, 확장성을 함께 본다.
- 최종 결정 자체는 `decision-log.md`에 남기고, 이 문서는 그 결정에 이르기까지 검토한 선택지를 관리한다.

## 2. AI Factory에서 이 문서가 담당하는 역할

Option Register는 **의사결정 전 단계의 지식 저장소**다.

역할은 다음과 같다.

1. 중요한 선택지를 잃어버리지 않는다.
2. 선택 기준을 명확하게 한다.
3. 현재 선택과 대안을 비교한다.
4. 나중에 기술이나 비용 조건이 바뀌었을 때 재검토할 후보를 남긴다.
5. 동일한 논의를 반복하는 것을 방지한다.

## 3. 실제 시스템에서는 어떻게 사용되는가?

일반적인 흐름은 다음과 같다.

```text
문제/요구사항 발생
→ 선택지 수집
→ 평가 기준 정의
→ 옵션 비교
→ 임시 선택
→ Decision Log에 결정 기록
→ Option Register에는 대안과 재검토 조건 보존
```

예:

```text
영상 생성 방식
├── A. 이미지 + 편집 템플릿
├── B. AI video generation 중심
└── C. 혼합 방식
        ↓
초기에는 A 선택
        ↓
B/C는 보류 옵션으로 유지
        ↓
영상 품질·비용·제작시간 데이터가 쌓이면 재평가
```

## 4. 구현 세부사항

### 4.1 Option Record

각 중요한 옵션은 다음 정보를 갖는다.

| Field | Description |
|---|---|
| Option ID | 고유 식별자 |
| Topic | 어떤 문제에 대한 선택지인지 |
| Option | 선택지 이름 |
| Description | 선택지의 핵심 내용 |
| Advantages | 장점 |
| Disadvantages | 단점 |
| Cost | 예상 비용 |
| Complexity | 구현/운영 복잡도 |
| Quality Impact | 품질에 미치는 영향 |
| Automation Impact | 자동화 수준에 미치는 영향 |
| Scalability | 확장성 |
| Current Status | 현재 상태 |
| Decision ID | 채택된 경우 연결된 결정 |
| Revisit Trigger | 재검토 조건 |

### 4.2 Status

- `candidate` — 검토 후보
- `evaluating` — 평가 중
- `selected` — 현재 선택
- `deferred` — 보류
- `rejected` — 현재 조건에서는 선택하지 않음
- `superseded` — 다른 선택으로 대체됨

### 4.3 기록 원칙

- 모든 사소한 선택을 기록하지 않는다.
- 향후 시스템 구조, 비용, 품질, 자동화 수준에 영향을 줄 수 있는 선택을 기록한다.
- 감정이나 선호보다 판단 기준을 우선한다.
- 당시의 정보와 제약조건을 함께 기록한다.
- 시간이 지나 결정이 바뀌더라도 과거의 판단을 임의로 수정하지 않는다.

## 5. Current Option Register

### OPT-001 — System of Record

**Topic:** 프로젝트 원본 및 버전 관리

**Options:**

- Git 중심
- Notion 중심
- Google Drive 중심
- Git + Notion 분리 운영

**Current:** `selected`

**Selection:** Git을 원본/버전 관리의 Source of Truth로 사용하고 Notion을 운영/승인 Dashboard로 사용한다.

**Reason:** 문서 변경 이력과 구조적 버전 관리가 필요하며, 운영 데이터와 원본 문서를 분리하는 것이 적합하다.

**Decision:** D-001

---

### OPT-002 — Workflow Orchestration

**Topic:** 자동화 실행 엔진

**Options:**

- n8n
- 직접 개발한 backend worker
- 서버리스 함수 중심
- 플랫폼별 자동화 도구 조합

**Current:** `selected`

**Selection:** 초기 단계에서는 n8n을 orchestration layer로 사용한다.

**Reason:** 시각적 workflow 구성, 외부 API 연결, scheduling, retry/error handling을 빠르게 구성하기에 적합하다.

**Decision:** D-001

---

### OPT-003 — Knowledge / RAG Layer

**Topic:** AI 지식 관리

**Options:**

- Dify Knowledge/RAG
- 직접 구축한 vector database
- 단순 파일 검색
- LLM context injection 중심

**Current:** `selected`

**Selection:** 초기 단계에서는 Dify를 Knowledge/RAG 및 AI workflow layer로 활용한다.

**Reason:** 의료·역사 자료를 지속적으로 축적하고 검색/참조하는 구조가 필요하다.

**Decision:** D-001

---

### OPT-004 — Content Production Strategy

**Topic:** Shorts와 Long-form의 관계

**Options:**

- Shorts와 Long-form을 독립적으로 운영
- Long-form을 먼저 만들고 Shorts로 분해
- Shorts를 topic validation으로 사용하고 검증된 주제를 Long-form으로 확장

**Current:** `selected`

**Selection:** Shorts를 주요 topic validation 채널로 사용하고 성과가 검증된 주제를 Long-form 후보로 확장한다.

**Decision:** D-005

---

### OPT-005 — Human Review Model

**Topic:** 인간 검수의 범위

**Options:**

- 모든 단계에서 수동 승인
- 완전 자동화
- Risk-based Human Review

**Current:** `selected`

**Selection:** 위험도가 높은 콘텐츠와 예외 상황을 중심으로 Human Review를 적용한다.

**Reason:** 완전 수동은 자동화 목표와 충돌하고, 완전 자동은 의료·역사·저작권·AI disclosure 영역에서 위험하다.

**Decision:** D-005

---

## 6. Revisit Policy

다음과 같은 조건이 발생하면 기존 옵션을 재평가한다.

- 비용이 예상보다 크게 증가한다.
- 특정 서비스의 장애가 반복된다.
- 품질 기준을 충족하지 못한다.
- 콘텐츠 생산량이 현재 구조의 한계를 초과한다.
- 새로운 기술이 운영 효율을 크게 개선한다.
- 법률/플랫폼 정책이 변경된다.
- Human Review 시간이 병목이 된다.
- 분석 결과 현재 전략의 가정이 틀렸음을 보여준다.

재평가 결과가 실제 운영 정책을 변경하면 반드시 새로운 Decision Log 항목을 생성한다.

## 7. Related Documents

- `05_decisions/decision-log.md`
- `05_decisions/rejected-ideas.md`
- `01_project/roadmap.md`
- `02_architecture/system-architecture.md`
- `02_architecture/state-machine.md`
