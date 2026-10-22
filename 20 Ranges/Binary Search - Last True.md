---
type: template
tags: ["Ranges"]
category: binary-search
categories: [binary-search]
---

# Binary Search - Last True

```text
candidate   0  1  2  3  4  5
valid       T  T  T  T  F  F
                     ↑ last T = 3

T = 가능 · F = 불가능
단조성: T T T → F F F     F가 된 뒤 T로 돌아가지 않음
불가:   F T F T          조건이 번갈아 뒤집힘
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Range** | // 정답을 포함하는 후보 구간을 잡는다. |
| **Check** | // 가운데 후보가 조건을 만족하는지 확인한다. |
| **Shrink · T** | // 현재 후보도 답일 수 있다. 남겨 두고 오른쪽을 찾는다. |
| **Shrink · F** | // 현재 후보부터 전부 불가능하다. 버리고 왼쪽을 찾는다. |
| **Answer** | // 후보가 하나 남으면 그 위치가 마지막 T다. |

## Java

```java
long left = 0;
long right = max;

while (left < right) {
    long mid = left + (right - left + 1) / 2;

    if (valid(mid)) {
        left = mid;
    } else {
        right = mid - 1;
    }
}

return left;
```

`valid(x)`는 문제별 가능 여부 함수 · 정수 후보 `0 ≤ x ≤ max < Long.MAX_VALUE` · 모두 T인 경우도 가능 · 판정 `O(log(max + 1))`회

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `left = mid` + 위쪽 mid | T인 mid도 답 후보이므로 남긴다. `[3,4]`에서는 mid=4를 골라야 left가 전진한다. |
| `+ 1`을 빼면 | `[3,4]`에서 mid=3이고 T일 때 left=3이 반복된다. 구간이 줄지 않아 무한 반복한다. |
| `mid - 1` | F인 mid는 답이 아니다. mid부터 제외한다. |
| `valid(0)`이 T | 구간 안에 T가 있어야 한다. F면 답이 없으므로 이 코드를 실행하기 전에 처리한다. |
| `return left` | 답을 남겨 가며 좁혔고 종료 시 left와 right가 같다. |

## First True와 구분

| 패턴 | 찾는 답 | T일 때 | mid |
| --- | --- | --- | --- |
| `TTT → FFF` | **마지막 T · 최대값** | `left = mid` | 위쪽 |
| `FFF → TTT` | **첫 T · 최소값** | `right = mid` | 아래쪽 |

방향이 반대라면 [[20 Ranges/Binary Search - First True|Binary Search - First True]]의 별도 템플릿을 쓴다.

---

**분류** [[00 Maps/Binary Search|Binary Search]]
