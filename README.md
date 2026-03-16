# algo-docs

알고리즘/자료구조 학습 노트 저장소입니다.
README는 자주 바뀌는 상세 목차 대신, 오래 유지되는 구조 원칙만 관리합니다.

## 목적
- 개념을 빠르게 복습할 수 있는 개인 레퍼런스 유지
- Java 기준 구현 패턴 축적
- 문제 풀이 전에 핵심 체크리스트를 즉시 확인

## 폴더 규칙
- `notes/Foundations`: 자료구조, 수학, 복잡도 등 기초
- `notes/Topics`: 알고리즘 주제별 정리 (예: Graph, DP, Greedy, Tree, String)
- `notes/Techniques`: 공통 풀이 테크닉 (예: Two Pointers, Sliding Window, Parametric Search)
- `templates/`: 노트 작성 템플릿
- `inbox/`: 임시 메모, TODO, 초안
- `attachments/`: 이미지/첨부 자료

## 분류 원칙
- 폴더 깊이는 최대 2단계를 기본으로 유지
- 분류가 애매하면 우선 `_notes/Topics`에 배치
- 세부 재분류는 노트가 쌓일 때만 수행

## 문서 작성 규칙
모든 주제 문서는 아래 흐름을 기본으로 작성합니다.
- 한 줄 요약
- Core Concept (언제 쓰는지 / 핵심 아이디어)
- API & State Change (연산과 상태 변화)
- Patterns (자주 쓰는 코드 패턴)
- Pitfalls & Tips (실수/디버깅 포인트)

새 문서는 `_templates/Main Template.md`를 복사해 시작합니다.

## B형 대비 빠른 진입
- 최적화 전략: `_notes/Techniques/B_Type_Optimization_Playbook.md`
- 주제별 최적화: `_notes/Techniques/Topic_Optimization_Strategies.md`
- 자바 성능 팁: `_notes/Techniques/Java_Fast_IO_and_Memory.md`
- 60% 축: `Dijkstra`, `Priority_Queue`, `Bucket`, `Union_Find`
- 40% 축: `DFS`, `Segment_Tree`, `Trie`, `Game_Basics`, `Hash`
