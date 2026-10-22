---
type: template
tags: ["Ranges"]
aliases: ["Binary_Search", "Binary Search"]
category: binary-search
categories: [binary-search]
---

# Binary Search - First True

```text
candidate   0  1  2  3  4  5
valid       F  F  F  T  T  T
                     ↑ first T = 3

F = 불가능 · T = 가능
단조성: F F F → T T T     T가 된 뒤 F로 돌아가지 않음
불가:   F T F T          조건이 번갈아 뒤집힘
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Range** | // 정답을 포함하는 후보 구간을 잡는다. |
| **Check** | // 가운데 후보가 조건을 만족하는지 확인한다. |
| **Shrink · T** | // 현재 후보도 답일 수 있다. 남겨 두고 왼쪽을 찾는다. |
| **Shrink · F** | // 현재 후보까지 전부 불가능하다. 버리고 오른쪽을 찾는다. |
| **Answer** | // 후보가 하나 남으면 그 위치가 첫 T다. |

## Java

```java
long left = 0;
long right = max;

while (left < right) {
    long mid = left + (right - left) / 2;

    if (valid(mid)) {
        right = mid;
    } else {
        left = mid + 1;
    }
}

return left;
```

`valid(x)`는 문제별 가능 여부 함수 · 정수 후보 `0 ≤ x ≤ max < Long.MAX_VALUE` · 모두 T인 경우도 가능 · 판정 `O(log(max + 1))`회

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `right = mid` + 아래쪽 mid | T인 mid도 답 후보이므로 남긴다. `[3,4]`에서 mid=3을 골라야 right가 줄어든다. |
| `mid + 1` | F인 mid는 답이 아니다. mid까지 제외한다. |
| `valid(max)`가 T | 구간 안에 T가 있어야 한다. F면 답이 없으므로 이 코드를 실행하기 전에 처리한다. |
| `return left` | 답을 남겨 가며 좁혔고 종료 시 left와 right가 같다. 별도의 answer가 필요 없다. |

## Last True와 구분

| 패턴 | 찾는 답 | T일 때 | mid |
| --- | --- | --- | --- |
| `FFF → TTT` | **첫 T · 최소값** | `right = mid` | 아래쪽 |
| `TTT → FFF` | **마지막 T · 최대값** | `left = mid` | 위쪽 |

방향이 반대라면 [[20 Ranges/Binary Search - Last True|Binary Search - Last True]]의 별도 템플릿을 쓴다.

---

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - Last True|Binary Search - Last True]]
