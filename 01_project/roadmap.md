# 기묘한 통증도감 AI Factory Roadmap
> Version: 0.1.0  
> Status: Initial Roadmap  
> Based on: `project-charter.md v0.1.0`  
> Description: 개발/구축 순서

## 1. Roadmap Principles

- 품질과 안전성을 생산량보다 우선한다.
- 자동화는 단계적으로 확대한다.
- 각 Phase는 실제 운영 검증을 통과한 뒤 다음 단계로 이동한다.
- 사람의 승인 지점을 제거하는 것이 아니라 위험도가 낮은 영역부터 자동화한다.
- Shorts를 주제 검증 장치로 활용하고 성과가 검증된 주제를 Long-form으로 확장한다.

## 2. Phase Overview

| Phase | 목표 | 핵심 결과 |
|---|---|---|
| 0 | Foundation | 프로젝트 기준과 문서 체계 확립 |
| 1 | Trend Discovery | 트렌드 수집 및 주제 추천 |
| 2 | Human Confirmation | 승인/거절/수정/보류 운영 |
| 3 | AI Content Core | 조사·Claim·대본 자동화 |
| 4 | Validation | 의학·역사·근거 검증 |
| 5 | Visual | 장면·이미지·영상 자산 자동화 |
| 6 | Voice | 음성 생성 및 검수 |
| 7 | Rendering | 영상 자동 조립·렌더링 |
| 8 | Video QA | 최종 영상 품질관리 |
| 9 | Publishing | 예약 발행 자동화 |
| 10 | Learning | 분석·비용·성과 기반 학습 |

## 3. Phase 0 — Foundation

프로젝트 Charter, Brand Bible, Glossary, Architecture, Data Model, Decision Log를 확정한다.

**완료 기준:** 역할·상태·데이터 흐름이 정의되고 후속 문서가 Charter와 충돌하지 않는다.

## 4. Phase 1 — Trend Discovery

Trend Source Registry를 기반으로 매일 데이터를 수집하고 정규화하여 3–5개의 주제 후보를 추천한다.

**완료 기준:** `Trend → Analysis → 3–5 Recommendations`가 반복 실행된다.

## 5. Phase 2 — Human Confirmation

Notion 또는 지정 승인 인터페이스에서 `APPROVE / REJECT / REQUEST_REVISION / DEFER`를 수행한다. 승인된 주제만 제작에 진입한다.

## 6. Phase 3 — AI Content Core

Research, Source Provenance, Claim Extraction, Medical/History Evidence Ledger, Script Generation을 Content Object로 연결한다.

## 7. Phase 4 — Validation

Medical Risk Level 0/1/2, Medical Claim Ledger, Historical Evidence Gate, Human Review Trigger를 구현한다. `최초` 계열 표현은 강화 검증한다.

## 8. Phase 5 — Visual

Scene Object → Prompt → Image/Video → Asset Metadata → Asset QA 흐름을 구현한다. Scene 생성은 가능한 경우 병렬화한다.

## 9. Phase 6 — Voice

Voice Script → TTS → Voice QA를 구현하고 발음·속도·자연스러움·대본 일치·타이밍을 검수한다.

## 10. Phase 7 — Rendering

고정 Creatomate Template, 자막, 음성, Scene Asset, Brand Element를 조립하여 자동 렌더링한다.

## 11. Phase 8 — Video QA

콘텐츠·의료·역사·브랜드·기술 품질을 최종 검수한다. FAIL 시 전체 파이프라인이 아니라 필요한 단계만 재작업한다.

## 12. Phase 9 — Publishing

YouTube API를 이용한 Upload → Metadata → Privacy → Schedule → Verification을 구현한다. 초기에는 Private/Unlisted 테스트 후 Public 예약발행으로 전환한다.

## 13. Phase 10 — Learning

Retention, Completion, Engagement, Subscriber Conversion, Topic Performance, Cost를 분석하여 Topic Recommendation, Hook, Format, Long-form 후보와 비용 최적화에 반영한다.

## 14. Production Scaling

### Phase A
`4 Shorts + 2 Long-form / week`

### Phase B
`5–7 Shorts + 2 Long-form / week`

### Phase C
`1 Short / day + 2–3 Long-form / week`

### Phase D
`Multi-channel / Multi-format`

## 15. Time Optimization

초기 목표:
- Shorts Human Time: `< 5–10 min`
- Long-form Human Time: `< 15–20 min`
- Shorts Machine Runtime: `20–30 min`
- Long-form Machine Runtime: `40–60 min`

장기적으로 병렬 실행, 템플릿/Asset 재사용, Prompt/Model 최적화, Caching, Batch Processing을 통해 Wall-clock Time을 단축한다. `1시간 이내 완성`은 초기 절대조건이 아니라 장기 최적화 목표다.

## 16. Gate Criteria

각 Phase는 다음을 만족해야 다음 단계로 이동한다.

```text
Functional
+ Reliable
+ Traceable
+ Safe
+ Recoverable
```

고위험 자동화에는 Human Override가 남아 있어야 한다.
