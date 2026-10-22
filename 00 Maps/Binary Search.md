---
type: map
level: family
category: binary-search
categories: [binary-search]
branch: sequences
---

# Binary Search

[[00 Maps/Sequences|Sequences]]

```text
후보가 커질수록 조건이 한 방향으로만 바뀜

F F F │ T T T       T T T │ F F F
        ↑ first T       ↑ last T
        최소 가능값     최대 가능값

F T F T처럼 되돌아가면 이 탐색을 쓸 수 없음
```

## First True · FFF → TTT

| 찾는 것 | 함께 기억할 선택 |
| --- | --- |
| 조건을 만족하는 최소 후보 | T이면 `right = mid` + 아래쪽 mid |
| 시작 전 확인 | 오른쪽 끝이 T여야 답이 존재 |

[[20 Ranges/Binary Search - First True|Binary Search - First True]]

## Last True · TTT → FFF

| 찾는 것 | 함께 기억할 선택 |
| --- | --- |
| 조건을 만족하는 최대 후보 | T이면 `left = mid` + 위쪽 mid |
| 시작 전 확인 | 왼쪽 끝이 T여야 답이 존재 |

[[20 Ranges/Binary Search - Last True|Binary Search - Last True]]

## Array Bounds

| 정렬 배열에서 찾는 위치 | 조건 |
| --- | --- |
| 첫 이상 · Lower Bound | `arr[mid] >= target` |
| 첫 초과 · Upper Bound | `arr[mid] > target` |

둘 다 **첫 T**를 찾는다. 해당 원소가 없으면 배열 길이 n을 반환한다.

[[10 Java/Array Binary Search|Array Binary Search]]

**응용** [[40 Optimization/LIS|LIS]] — 길이별 마지막 값 중 처음 바꿀 위치를 찾는다.
