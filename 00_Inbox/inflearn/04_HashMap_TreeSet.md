---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 4 - HashMap TreeSet

## 1. HashMap 기초

### 기본 조작
| 코드                                                |     요약 / 결과     | 예시           |
|:--------------------------------------------------|:---------------:|:-------------|
| `HashMap<String, Integer> map = new HashMap<>();` |       선언        | —            |
| `map.put("A", 1);`                                |      추가/갱신      | {"A": 1}     |
| `map.get("A");`                                   | 값 조회 (없으면 null) | 1            |
| `map.getOrDefault("A", 0);`                       |     기본값 반환      | "Z" → 0      |
| `map.containsKey("A");`                           |     키 존재 여부     | true / false |
| `map.remove("A");`                                |      키 제거       | —            |
| `map.size();`                                     |      원소 개수      | —            |

### 자주 쓰는 패턴
| 코드                                                     |  요약 / 결과  | 예시                           |
|:-------------------------------------------------------|:---------:|:-----------------------------|
| `map.put(x, map.getOrDefault(x, 0) + 1);`              |   개수 누적   | 문자/숫자 빈도                     |
| `for (String key : map.keySet()); `                    |   키 순회    | —                            |
| `for (Map.Entry<String, Integer> e : map.entrySet());` | 키/값 동시 순회 | `e.getKey()`, `e.getValue()` |
| `Collections.max(map.values())`                        |  최빈값 찾기   |


## 2. HashSet 기초
| 코드                                        |    요약 / 결과    | 예시             |
|:------------------------------------------|:-------------:|:---------------|
| `HashSet<Integer> set = new HashSet<>();` |      선언       | —              |
| `set.add(3);`                             |      추가       | `[3]`          |
| `set.add(3);`                             |     중복 무시     | `[3]`          |
| `set.remove(3);`                          |      삭제       | `[]`           |
| `set.contains(3);`                        |     포함 여부     | `true / false` |
| `set.size();`                             |     원소 개수     | —              |
| `set.clear();`                            |     전체 삭제     | —              |
| `for (int x : set)`                       |      순회       | 전체 원소 탐색       |
| `new HashSet<>(list)`                     | List → Set 변환 | 중복 제거          |


## 3. TreeSet 기초
| 코드                                                                 |    요약 / 결과    | 예시             |
|:-------------------------------------------------------------------|:-------------:|:---------------|
| `TreeSet<Integer> ts = new TreeSet<>();`                           |  선언 (자동 정렬)   | —              |
| `TreeSet<Integer> ts = new TreeSet<>(Collections.reverseOrder());` | 선언 (역순 자동 정렬) | —              |
| `ts.add(10);` `ts.add(20);` `ts.add(30);`                          |    오름차순 저장    | `[10, 20, 30]` |
| `ts.first();`                                                      |    가장 작은 값    | `10`           |
| `ts.last();`                                                       |    가장 큰 값     | `30`           |
| `ts.lower(20);`                                                    |  20보다 작은 최대값  | `10`           |
| `ts.higher(20);`                                                   |  20보다 큰 최소값   | `30`           |
| `ts.floor(20);`                                                    |  20 이하 중 최대   | `20`           |
| `ts.ceiling(20);`                                                  |  20 이상 중 최소   | `20`           |
| `ts.remove(x);`                                                    |      삭제       | —              |
> `higher(x)`는 초과, `ceiling()`은 이상 개념


## 4. 주의 포인트
|    코드 / 개념    | 요약                                                            |
|:-------------:|:--------------------------------------------------------------|
| HashMap 순서 없음 | 필요 시 `LinkedHashMap` 사용                                       |
|    HashMap    | 중복 X, 정렬 X                                                    |
|    HashSet    | 중복 X, 정렬 X                                                    |
|    TreeSet    | 중복 X, 정렬 O                                                    |
|   객체형 비교 시    | `equals` & `hashCode` 오버라이드 / `Comparable`의 `toCompare` 오버라이드 |


## **핵심 요약**
- HashMap: `put`, `getOrDefault`, `entrySet` 순회
- HashSet: 중복 제거 / 방문 체크 / 빠른 탐색
- TreeSet: 정렬된 값 탐색 (`first`, `last`, `floor`, `ceiling`)
- 탐색 속도: Hash O(1), Tree O(logN)

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section04_hashmap_treeset)**
| 분류 | 문제 이름                                                                             | 포인트 메모     |
|:--:|:----------------------------------------------------------------------------------|:-----------|
| 기초 | [04-03 매출액의 종류](../../src/inflearn/section04_hashmap_treeset/INF_S04_P03.java)    | 투포인터 + 해시맵 |
| 기초 | [04-04 모든 아나그램 찾기](../../src/inflearn/section04_hashmap_treeset/INF_S04_P04.java) | 투포인터 + 해시맵 |
| 응용 | [04-05 K번째 큰 수](../../src/inflearn/section04_hashmap_treeset/INF_S04_P05.java)    | 트리셋        |