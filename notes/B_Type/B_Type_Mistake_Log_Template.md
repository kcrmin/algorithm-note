---
type: concept
tags:
  - b-type
  - b-type-mistake-log-template
language: java
status: in-review
source:
  - "[[B_Type_Master_Index]]"
---

# B형 Mistake Log Template

> **한 줄 요약:** 틀린 이유를 유형화해 같은 실수를 반복하지 않기 위한 템플릿.

## 기록 템플릿

- 문제: 
- 날짜: 
- 체감 난이도(1-5): 
- 첫 통과/실패 원인: 

## 실수 유형 (복수 선택)

- 모델링 오류
- 경계값/인덱스 오류
- 자료형/overflow
- 시간초과(중복 탐색, 자료구조 선택)
- 메모리초과(상태 과다, 객체 생성 과다)

## 수정 포인트

- 바꾼 자료구조:
- 추가한 pruning/stale 조건:
- 다음 문제에서 강제할 체크리스트:

## 재발 방지 규칙

- 같은 유형 3회 누적 시 전용 미니 노트 생성
- 다음 1주일 동안 해당 유형 문제를 2문제 추가 풀이
