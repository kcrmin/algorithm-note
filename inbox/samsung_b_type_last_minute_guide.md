# 삼성 SW 역량 테스트 B형 막판 대비 가이드

이 문서는 **시간이 얼마 안 남은 상태**에서 삼성 SW 역량 테스트 **B형**을 준비하는 사람을 위한 실전 가이드다.

핵심 목표는 세 가지다.

1. 문제를 보자마자 **자료구조를 10초 안에 잡는 것**
2. 구현 전에 **노트에 설계부터 고정하는 것**
3. 시험장에서 **버그를 줄이고 점수를 가져가는 순서**를 몸에 익히는 것

---

## 0. 먼저 결론

시간이 부족하면 이것만 기억해도 된다.

```text
id가 있으면 -> 배열 먼저
그룹이 있으면 -> bucket
삭제/이동이 많으면 -> linked list 또는 lazy deletion
최대/최소/상위 k면 -> priority queue
문제 연산 수가 크면 -> O(N) 금지
```

그리고 삼성 B형에서 제일 중요한 문장 하나:

> 이 시험은 어려운 알고리즘 시험이라기보다, **대용량 연산을 버티는 자료구조 설계 시험**에 가깝다.

---

## 1. 문제를 처음 봤을 때 10초 안에 구조 잡는 방법

문제를 읽으면서 아래 6개를 순서대로 체크한다.

### 1-1. 연산 수부터 본다

문제에 보통 이런 게 있다.

- `Q <= 100000`
- `호출 횟수 <= 200000`
- `ID 범위 <= 100000`

이걸 보면 바로 생각해야 한다.

- `매 연산 O(N)`이면 위험
- 목표는 `O(1)` 또는 `O(logN)`

즉, 문제를 읽자마자

```text
매번 전체 순회하면 죽는다
```

를 먼저 떠올려야 한다.

---

### 1-2. 객체에 고유 ID가 있는지 본다

예:

- 병사 ID
- 학생 ID
- 사용자 ID
- 파일 ID
- 게시글 ID

이게 보이면 거의 자동으로:

```text
배열[id]
```

을 떠올려야 한다.

예를 들면:

- `teamOf[id]`
- `scoreOf[id]`
- `alive[id]`
- `version[id]`
- `head[id]`

왜냐하면 ID 기반 문제에서 배열 접근은 가장 빠르기 때문이다.

---

### 1-3. 그룹 분류 기준이 있는지 본다

예:

- 팀별
- 점수별
- 카테고리별
- 타입별
- 등급별

이게 보이면:

```text
bucket[group]
bucket[group1][group2]
```

를 생각한다.

예:

- `bucket[team][score]`
- `bucket[type]`
- `bucket[level][status]`

즉 **비슷한 성질끼리 통에 나눠 담는 구조**를 먼저 본다.

---

### 1-4. 삭제/이동이 많은지 본다

연산에 이런 게 있으면 중요하다.

- remove
- fire
- erase
- update해서 다른 그룹으로 이동
- 상태 변경

그러면 질문은 하나다.

```text
이걸 빨리 빼고 빨리 옮길 수 있나?
```

여기서 후보가 나온다.

- linked list
- doubly linked list
- lazy deletion
- 버전 관리

삼성 B형은 **“진짜로 삭제하지 않고 죽은 걸로 표시하는 방식”**도 자주 잘 먹힌다.

---

### 1-5. 최대/최소/상위값 질의가 있는지 본다

예:

- 최고 점수 찾기
- 가장 큰 ID
- 최소 비용
- top K

그러면:

- priority queue
- ordered bucket
- score를 거꾸로 순회

중 하나가 보통 답이다.

주의할 점은,

```text
최대값 질의가 있다고 무조건 heap은 아님
```

예를 들어 점수 범위가 1~5처럼 작으면 힙보다 **bucket + 역순 탐색**이 더 낫다.

---

### 1-6. 값 범위가 작은지 본다

예:

- score: 1 ~ 5
- team: 1 ~ 5
- 등급: 1 ~ 10

이런 건 엄청 중요한 힌트다.

값 범위가 작으면:

- 배열 가능
- bucket 가능
- 정렬 대신 인덱스 접근 가능

즉,

```text
값 범위가 작다 = 분류해서 관리하라는 신호
```

로 보는 게 좋다.

---

## 2. 10초 판단 공식

문제 읽고 아래 공식으로 바로 분해한다.

```text
1) id 있음?         -> 배열
2) 그룹 있음?       -> bucket
3) 삭제/이동 많음?  -> linked list / lazy deletion
4) 최대/최소 질의?  -> heap 또는 점수 bucket 역순 탐색
5) 범위 작음?       -> 배열로 압축
```

이 5개를 연산별로 연결하면 대충 구조가 나온다.

예시:

```text
병사관리
- id 있음 -> 배열
- team, score 있음 -> bucket[team][score]
- fire, update 많음 -> lazy deletion 또는 linked list
- bestSoldier 있음 -> score 5~1 역순 탐색
```

구조 요약:

```text
alive[id]
teamOf[id]
scoreOf[id]
bucket[team][score]
```

---

## 3. 시험장에서 노트는 어떻게 적어야 하는가

문제를 읽고 바로 코드 치면 망할 확률이 높다.

먼저 **종이에 설계 노트**를 적는다.

### 3-1. 노트 첫 줄: 객체 정의

예:

```text
객체: soldier
속성: id, team, score, alive
```

또는

```text
객체: user
속성: id, group, value, status
```

이걸 적어야 머리가 정리된다.

---

### 3-2. 두 번째 줄: 연산 목록

예:

```text
init
hire(id, team, score)
fire(id)
updateSoldier(id, score)
updateTeam(team, delta)
bestSoldier(team)
```

연산은 반드시 **전부 적는다**.

누락되면 설계가 틀어진다.

---

### 3-3. 세 번째 줄: 연산별 목표 시간복잡도

예:

```text
hire: O(1)
fire: O(1)
updateSoldier: O(1)
updateTeam: O(1) ~ O(scoreRange)
bestSoldier: O(scoreRange + bucket scan)
```

이걸 적는 이유는,

구현하다가 `O(N)`짜리 풀이로 미끄러지는 걸 막기 위해서다.

---

### 3-4. 네 번째 줄: 자료구조 선언 초안

예:

```text
alive[id]
teamOf[id]
scoreOf[id]
bucket[team][score]
```

혹은

```text
nameToId : HashMap
parent[id]
children[id]
```

이 단계에서 “무엇을 어디에 저장할지”를 끝낸다.

---

### 3-5. 다섯 번째 줄: 각 연산의 처리 방식 한 줄 요약

예:

```text
hire: 해당 bucket에 추가
fire: alive=false
updateSoldier: old 죽이고 새 score bucket에 재삽입
updateTeam: 5개 bucket을 새 위치로 merge
best: score 높은 순으로 보며 살아있는 최신 id 최대 찾기
```

이 한 줄 메모가 구현 중 길을 잃지 않게 해준다.

---

## 4. 시험장에서 코드 짜는 순서

항상 아래 순서를 추천한다.

### 4-1. 구조체 / 클래스

예:

- Node
- LinkedList
- UserSolution 필드

---

### 4-2. init 먼저

초기화가 정확해야 나머지가 덜 꼬인다.

체크:

- 배열 초기화
- bucket 생성
- 카운터/버전 초기화

---

### 4-3. 가장 쉬운 연산부터

보통:

- add / hire
- insert

이 먼저다.

이걸 먼저 통과시켜야 이후 연산이 편하다.

---

### 4-4. delete / fire

이때 제일 먼저 판단한다.

```text
진짜 삭제할지, 죽은 표시만 할지
```

시간이 부족하면 대개 `alive=false`가 훨씬 안정적이다.

---

### 4-5. update 계열

업데이트는 보통 이 중 하나다.

- 같은 객체의 속성 수정
- 기존 기록 무효화 후 새로 삽입
- 그룹 전체 shift

여기서 시간복잡도를 가장 주의한다.

---

### 4-6. query / best / count 마지막

조회 함수는 **앞에서 설계한 구조가 맞는지 검증하는 단계**다.

즉 query를 짤 때 막히면,

```text
구조 설계가 잘못되었을 가능성
```

이 높다.

---

## 5. 시간이 얼마 안 남았을 때 공부 우선순위

정말 급하면 아래 순서대로 본다.

### 5-1. 1순위: 배열 기반 ID 관리

반드시 익혀야 한다.

핵심 패턴:

```text
value[id]
status[id]
team[id]
version[id]
```

이건 B형에서 거의 기본 체력이다.

---

### 5-2. 2순위: bucket 설계

반드시 연습해야 한다.

예:

```text
bucket[team][score]
bucket[type]
bucket[level]
```

포인트는 두 가지다.

1. 어떤 축으로 분류할지
2. bucket 안에 무엇을 저장할지

---

### 5-3. 3순위: linked list / doubly linked list 감각

필수 개념:

- 앞뒤 연결
- O(1) 삽입
- O(1) 삭제
- 리스트 붙이기

주의:

실전에서는 **직접 연결리스트를 구현하지 않고** lazy deletion으로 우회하는 경우도 많다.

즉,

```text
linked list를 알아야 하지만, 항상 정석 삭제를 구현할 필요는 없다
```

---

### 5-4. 4순위: priority queue + stale skip

상위값, 랭킹, 추천, 작업 관리 문제에서 중요하다.

핵심 패턴:

```text
pq에 여러 번 넣고
꺼낼 때 현재 상태와 다르면 버린다
```

이 stale skip은 삼성 스타일 문제에서 자주 유효하다.

---

### 5-5. 5순위: HashMap

문자열/경로/이름이 등장하면 중요하다.

예:

- 파일명 → id
- 폴더명 → 노드
- 사용자명 → 번호

---

## 6. 시간이 없을 때 버려야 하는 것

우선순위 낮은 것들:

- 세그먼트 트리
- 플로이드
- 고급 DP
- 복잡한 그래프 최단경로 세트

물론 PS 전반에서는 중요하지만, **삼성 B형 막판 대비용으로는 효율이 낮다.**

---

## 7. 삼성 B형에서 자주 나오는 구현 패턴

### 7-1. lazy deletion

진짜로 삭제하지 않고:

```text
alive[id] = false
```

만 한다.

장점:

- 구현이 단순함
- 버그가 적음
- update를 재삽입으로 처리 가능

단점:

- bucket이나 pq 안에 찌꺼기가 쌓임
- query 시 현재 유효한지 검증 필요

검증에 보통 쓰는 것:

- alive[id]
- version[id]
- currentTeam[id]
- currentScore[id]

---

### 7-2. versioning

동일 id에 대해 갱신이 많으면:

```text
version[id]++
```

하고, 노드에 version을 저장한다.

query에서 꺼냈을 때 현재 version과 다르면 폐기한다.

이건 heap, linked structure, 로그형 문제에서 특히 강력하다.

---

### 7-3. bucket merge

팀 전체 점수 +1 같은 연산에서, 개별 원소를 하나씩 옮기면 느리다.

가능하면:

```text
리스트 통째로 이동/붙이기
```

를 생각한다.

---

### 7-4. 값 범위가 작으면 정렬하지 않기

점수 1~5면 정렬 대신:

```text
5부터 1까지 검사
```

이게 훨씬 낫다.

---

## 8. 문제를 보자마자 떠올려야 하는 질문 리스트

문제마다 아래를 자기에게 물어본다.

1. 객체는 무엇인가?
2. 고유 ID가 있는가?
3. 그룹은 무엇인가?
4. 삭제가 잦은가?
5. update가 잦은가?
6. query는 무엇을 요구하는가?
7. 값 범위가 작은가?
8. 전부 순회하면 시간초과인가?
9. stale data를 허용할 수 있는가?
10. 정답 검증 시 최신 상태 확인이 가능한가?

이 10개 질문이 구조 설계의 뼈대다.

---

## 9. 시험장용 템플릿 사고법

예를 들어 아무 문제를 봤다고 하자.

그럼 노트에 아래 템플릿을 바로 쓴다.

```text
[객체]
- ?

[속성]
- id:
- group:
- value:
- status:

[연산]
- init
- add
- remove
- update
- query

[제약]
- Q:
- ID range:
- value range:

[자료구조]
- array[id]
- bucket[group]
- heap ?
- hash ?
- alive/version ?

[연산 처리]
- add:
- remove:
- update:
- query:
```

이 템플릿을 습관화하면 구조를 놓치지 않는다.

---

## 10. 디버깅할 때 보는 순서

시간이 부족한 시험에서는 디버깅 순서가 중요하다.

### 10-1. init이 맞는가

많은 버그가 여기서 시작한다.

체크:

- 배열 초기값
- 버킷 새로 생성됐는지
- 이전 테스트케이스 찌꺼기 제거됐는지

---

### 10-2. add가 상태를 전부 갱신하는가

예:

- alive
- teamOf
- scoreOf
- version
- bucket 삽입

이 중 하나만 빼먹어도 query가 무너진다.

---

### 10-3. remove가 “현재 상태”를 무효화했는가

실제 삭제를 안 한다면:

- alive=false
- version 증가

같은 무효화 장치가 있어야 한다.

---

### 10-4. update가 old/new를 둘 다 처리했는가

가장 흔한 실수는:

- 새 상태는 넣었는데 old 상태를 무효화 안 함
- old를 무효화했는데 새 상태 기록을 안 함

---

### 10-5. query가 최신 상태 검증을 하는가

특히 lazy deletion 방식이면 반드시 확인한다.

예:

```text
if (!alive[id]) continue
if (scoreOf[id] != expectedScore) continue
if (teamOf[id] != expectedTeam) continue
if (version mismatch) continue
```

---

## 11. 시험에서 절대 하면 안 되는 것

### 11-1. 구조 안 정하고 바로 구현

가장 위험하다.

---

### 11-2. ArrayList.remove 같은 선형 삭제에 기대기

연산 수가 큰 문제에서는 바로 터진다.

---

### 11-3. update 때 전체 순회

특히 그룹 전체 업데이트에서 매우 위험하다.

---

### 11-4. “일단 돌아가게만” 짜고 최적화 나중에 하기

B형은 대개 그 전략이 통하지 않는다.

처음부터 구조를 맞춰야 한다.

---

### 11-5. query를 먼저 짜기

query부터 막히면 구현 순서가 꼬이기 쉽다.

---

## 12. 막판 3일용 공부 플랜

### Day 1

- ID 기반 배열 관리 패턴 복습
- bucket 설계 문제 1개
- lazy deletion / versioning 패턴 정리

### Day 2

- linked list 또는 bucket merge 문제 1개
- priority queue + stale skip 문제 1개
- 노트 설계 연습 3세트

### Day 3

- 실전처럼 1문제 제한시간 두고 풀기
- 구현 후 반드시 “상태 배열/버킷/무효화” 체크리스트 검토
- 새로운 개념 공부 금지, 이미 본 패턴만 복습

---

## 13. 시험 직전 체크리스트

문제 시작 전에 머릿속에 넣어둘 것:

```text
[공식]
id -> array
group -> bucket
삭제 -> alive/version 또는 linked list
최대/최소 -> heap 또는 역순 bucket
범위 작음 -> 배열 압축
```

그리고 매 문제마다:

```text
객체
속성
연산
제약
자료구조
시간복잡도
```

6줄을 먼저 적는다.

---

## 14. 자주 쓰는 노트 예시

### 예시 A: 병사관리 스타일

```text
[객체]
soldier

[속성]
id, team, score, alive

[연산]
hire
fire
updateSoldier
updateTeam
bestSoldier

[자료구조]
alive[id]
teamOf[id]
scoreOf[id]
bucket[team][score]

[처리]
hire: bucket에 삽입
fire: alive=false
updateSoldier: old 무효화 후 새 score 삽입
updateTeam: bucket 이동/merge
best: 5->1 순회하며 최신 상태 최대 id
```

### 예시 B: 랭킹/추천 스타일

```text
[객체]
item

[속성]
id, score, alive, version

[연산]
add
remove
updateScore
getTop

[자료구조]
scoreOf[id]
alive[id]
version[id]
maxHeap

[처리]
add: heap push
remove: alive=false or version++
update: 새 score와 version으로 다시 push
getTop: stale skip 반복
```

---

## 15. 마인드셋

막판에는 “더 많은 알고리즘”보다 “더 적은 실수”가 훨씬 중요하다.

즉 목표는:

- 새로운 희귀 알고리즘 공부
n아님
- **삼성 B형에서 자주 나오는 구조를 빠르게 인식하는 능력**

이다.

시험장에서는 이렇게 생각하면 된다.

```text
이 문제는 어떤 자료구조 문제인가?
이 연산을 O(1) 또는 O(logN)으로 만들려면 상태를 어디에 저장해야 하나?
삭제를 진짜로 할 필요가 있나, 아니면 무효화만 해도 되나?
```

이 세 질문만 정확히 해도 절반 이상은 구조가 잡힌다.

---

## 16. 마지막 한 장 요약

```text
[문제 읽는 순서]
1. Q 크기 확인
2. id 있는지 확인
3. 그룹 기준 확인
4. 삭제/이동 많은지 확인
5. 최대/최소 질의 확인
6. 값 범위 작은지 확인

[구조 공식]
id -> array
group -> bucket
삭제/업데이트 -> alive/version 또는 linked list
최대/최소 -> heap 또는 역순 bucket

[노트]
객체
속성
연산
자료구조
연산별 처리
시간복잡도

[구현 순서]
구조 -> init -> add -> remove -> update -> query

[디버깅 순서]
init -> add -> remove -> update -> query
```

---

이 문서는 막판용으로 만들었기 때문에, 범위를 넓게 다루기보다 **시험장에서 바로 쓸 수 있는 판단 루틴**에 집중했다.
