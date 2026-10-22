---
type: template
tags: ["Search"]
category: binary-search
categories: [binary-search]
---

# Array Binary Search

```text
index       0  1  2  3  4     5 = n
value       1  3  3  3  8     (배열 밖)
target = 3     ↑        ↑
             lower    upper
             첫 ≥3    첫 >3

3의 개수 = upper - lower = 4 - 1 = 3
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Range** | // 배열 끝 다음 위치까지 답 후보에 넣는다. 못 찾으면 이 위치가 답이다. |
| **Check** | // 가운데 값이 target 이상인지 확인한다. |
| **Shrink** | // 이상이면 가운데를 남기고 왼쪽, 미만이면 가운데까지 버리고 오른쪽으로 간다. |
| **Answer** | // 후보가 하나 남으면 첫 위치를 반환한다. |

## Java

```java
int left = 0;
int right = arr.length;

while (left < right) {
    int mid = left + (right - left) / 2;

    if (arr[mid] >= target) {
        right = mid;
    } else {
        left = mid + 1;
    }
}

return left;
```

위 코드는 **Lower Bound** · `arr` 오름차순 · `O(log n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 오름차순 배열 | target 이상이라는 조건이 F→T로만 바뀌어야 한다. |
| `right = n` | target 이상인 원소가 없으면 n을 돌려준다. 루프 안의 mid는 n보다 작으므로 배열 접근은 안전하다. |
| 반환값으로 조회하기 전 | 결과가 n이면 해당 원소가 없다. 빈 배열도 0=n을 반환한다. |
| 값이 정확히 존재하는가 | 결과가 n보다 작고 그 위치의 값이 target인지 함께 확인한다. |

## Upper Bound · 첫 > target

| 찾는 위치 | 위 코드에서 사용할 조건 | 나머지 이동 / 반환 |
| --- | --- | --- |
| Lower Bound · 첫 이상 | `arr[mid] >= target` | 동일 |
| Upper Bound · 첫 초과 | `arr[mid] > target` | 동일 |

중복을 건너뛰려면 **`>=`를 `>`로 바꾼다.** Upper Bound도 첫 T를 찾으므로 mid 방향과 반환은 그대로다.

---

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - First True|Binary Search - First True]]
