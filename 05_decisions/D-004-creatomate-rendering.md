# D-004 — Video Rendering

## Date
2026-09-07

## Problem

자동 영상 렌더링을 어떤 서비스로 처리할 것인가?

## Candidates

1. Creatomate
2. Shotstack
3. 자체 FFmpeg

## Evaluation

### Creatomate
Pros:
- Template based
- API rendering
- Webhook

Cons:
- 외부 서비스 의존

### Shotstack
Pros:
- API 기반
- 영상 편집 기능

Cons:
- 실제 프로젝트 템플릿 적합성 확인 필요

### FFmpeg
Pros:
- 완전한 제어
- 비용 절감 가능

Cons:
- 개발 부담
- 유지보수 증가

## Decision

Creatomate를 MVP 1순위로 테스트.

## Why

현재 목표가 영상 편집 엔진 자체를 개발하는 것이 아니라
콘텐츠 자동 생산 시스템을 빠르게 검증하는 것이기 때문.

## Reconsider If

- 렌더 비용이 과도함
- 원하는 템플릿 구현 불가
- API 안정성 문제가 반복됨

## Status

ACTIVE