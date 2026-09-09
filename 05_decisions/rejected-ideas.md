# Rejected Ideas

> Version: 0.1.0
> Status: Initial Rejected Ideas Baseline
> Parent: `01_project/project-charter.md`
> Description: 현재 채택하지 않은 아이디어와 그 이유를 기록

## 0. 이 문서를 왜 만드는가?

AI Factory를 설계하다 보면 많은 아이디어가 나오지만, 모든 아이디어를 구현하면 시스템은 빠르게 복잡해진다. 이 문서는 현재 채택하지 않은 아이디어와 그 이유를 기록하여 **같은 논의를 반복하거나 이미 검토한 아이디어를 무분별하게 다시 도입하는 것을 방지**하기 위해 만든다.

`Rejected`는 영구적으로 틀렸다는 뜻이 아니다. 현재의 목표, 비용, 기술 수준, 운영 조건에서 채택하지 않았다는 뜻이다.

## 1. 이 문서에서 반드시 이해해야 하는 것

- 아이디어를 거절하는 것도 설계 결정이다.
- "나쁜 아이디어"와 "지금은 필요하지 않은 아이디어"를 구분한다.
- 미래에 조건이 바뀌면 재검토할 수 있다.
- 거절 이유가 없으면 같은 아이디어가 반복해서 등장한다.
- 이 문서는 아이디어를 묻어버리는 곳이 아니라 **재검토 가능한 상태로 보존하는 곳**이다.

## 2. AI Factory에서 이 문서가 담당하는 역할

Rejected Ideas는 **설계의 경계선(boundary)** 을 기록한다.

무엇을 만들지뿐 아니라 무엇을 만들지 않을지도 명확히 해야 시스템이 불필요하게 커지는 것을 막을 수 있다.

특히 초기 단계에서는 다음 원칙을 적용한다.

> 현재의 핵심 생산 흐름을 개선하지 않는 기능은 우선 구현하지 않는다.

## 3. 실제 시스템에서는 어떻게 사용되는가?

아이디어가 발생하면 다음과 같이 처리한다.

```text
Idea 발생
→ 필요성 평가
→ 현재 목표와 비교
→ 즉시 필요하지 않으면 Rejected Ideas에 기록
→ 재검토 조건이 명확하면 조건 기록
→ 조건 충족 시 Option Register로 이동
→ 재평가
```

즉,

```text
Rejected ≠ 삭제
Rejected = 현재 범위 밖
```

## 4. 구현 세부사항

### 4.1 Rejected Idea Record

| Field            | Description                  |
| ---------------- | ---------------------------- |
| Idea ID          | 고유 식별자                  |
| Idea             | 아이디어                     |
| Category         | 어떤 영역의 아이디어인지     |
| Status           | rejected/deferred/superseded |
| Reason           | 현재 채택하지 않은 이유      |
| Risk             | 도입 시 예상되는 문제        |
| Revisit Trigger  | 재검토 조건                  |
| Related Decision | 관련 결정                    |
| Date             | 기록일                       |

### 4.2 기록 원칙

- 비판보다 판단 근거를 기록한다.
- "별로라서" 같은 주관적 이유는 사용하지 않는다.
- 현재의 목표와 제약조건을 명시한다.
- 재검토 가능성이 있는 아이디어는 조건을 반드시 기록한다.
- 이미 다른 결정으로 대체된 경우 `superseded`로 표시한다.

### 4.3 Status

- `deferred` — 보류
- `rejected` — 현재 조건에서는 선택하지 않음
- `superseded` — 다른 선택으로 대체됨

## 5. Current Rejected / Deferred Ideas

### REJ-001 — 완전 무인 콘텐츠 제작

**Status:** `rejected`

**Idea:** Trend discovery부터 YouTube publishing까지 모든 단계를 사람의 승인 없이 완전 자동으로 실행한다.

**Reason:** 의료·역사적 사실·저작권·AI disclosure 등 고위험 영역에서 인간의 판단이 필요한 경우가 있다. 초기 시스템에서는 완전 무인보다 Risk-based Human Review가 적합하다.

**Revisit Trigger:** 장기간의 운영 데이터에서 고위험 오류율이 충분히 낮아지고, 각 Gate의 자동 검증 신뢰도가 입증될 경우 단계별 자동 승인 범위를 재검토한다.

---

### REJ-002 — 모든 콘텐츠를 처음부터 Long-form으로 제작

**Status:** `rejected`

**Idea:** Shorts를 별도의 검증 과정으로 사용하지 않고 Long-form을 중심으로 제작한다.

**Reason:** 초기 채널에서는 어떤 주제와 Hook이 실제 시청자에게 반응을 얻는지 빠르게 검증하는 것이 중요하다. Shorts가 상대적으로 빠른 실험 단위가 될 수 있다.

**Revisit Trigger:** 채널의 Long-form 데이터가 충분히 축적되어 별도의 validation 전략이 더 효과적이라는 근거가 확보될 경우.

---

### REJ-003 — 처음부터 모든 생성형 AI 서비스를 병렬 도입

**Status:** `deferred`

**Idea:** 이미지·영상·음성·LLM 분야에서 여러 공급자를 동시에 연결하여 자동으로 최적 모델을 선택한다.

**Reason:** 초기 단계에서는 provider abstraction 자체가 운영 복잡도를 증가시키며, 품질·비용 데이터를 비교할 기준도 충분하지 않다.

**Revisit Trigger:** 콘텐츠 생산량이 증가하고 단일 provider 의존성 또는 비용이 실질적인 병목으로 확인될 경우.

---

### REJ-004 — 모든 문서를 Notion만으로 관리

**Status:** `rejected`

**Idea:** Git을 사용하지 않고 Notion을 전체 문서와 설정의 단일 원본으로 사용한다.

**Reason:** 구조화된 버전 관리, 변경 이력, AI/개발 workflow와의 연계 측면에서 Git을 Source of Truth로 유지하는 편이 적합하다. Notion은 운영 Dashboard와 인간의 검토/승인 인터페이스에 집중한다.

**Related Decision:** D-001

---

### REJ-005 — 조회수만으로 자동 최적화

**Status:** `rejected`

**Idea:** Analytics가 조회수만을 기준으로 다음 콘텐츠의 주제와 제작 방향을 자동 결정한다.

**Reason:** 조회수는 단독으로 콘텐츠 품질과 채널 가치를 충분히 설명하지 않는다. Retention, completion, engagement, subscriber conversion, 비용 등의 지표를 함께 봐야 한다.

**Revisit Trigger:** 없음. 단일 조회수 최적화 방식은 핵심 운영 원칙으로 사용하지 않는다. 다만 조회수의 역할 자체는 다른 지표와 함께 지속적으로 재평가한다.

---

### REJ-006 — 인터넷에서 찾은 이미지를 자동 수집하여 영상에 사용

**Status:** `rejected`

**Idea:** 검색된 이미지나 영상을 자동으로 다운로드하여 Asset으로 사용한다.

**Reason:** 검색 가능 여부와 상업적 이용 가능 여부는 다르다. 저작권 및 라이선스 검증이 되지 않은 Asset을 자동 사용하면 법적·운영적 위험이 크다.

**Revisit Trigger:** 명확한 라이선스 정보와 사용 허가를 자동 검증할 수 있는 Asset source registry가 구축될 경우 제한적으로 재검토한다.

## 6. 운영 원칙

이 문서는 프로젝트가 커질수록 중요해진다.

다음 질문을 먼저 확인한다.

> "이 기능이 없으면 현재의 핵심 생산 Pipeline이 막히는가?"

답이 아니면 즉시 구현하지 않고 다음 중 하나로 분류한다.

- Option Register로 보내 추가 평가
- Rejected Ideas에 기록
- Roadmap의 후순위 항목으로 이동

이 원칙은 **기능을 많이 만드는 것보다 안정적인 핵심 Pipeline을 먼저 완성한다**는 방향을 유지하기 위한 것이다.

## 7. Related Documents

- `05_decisions/decision-log.md`
- `05_decisions/option-register.md`
- `01_project/roadmap.md`
- `02_architecture/system-architecture.md`
- `02_architecture/service-flow.md`
