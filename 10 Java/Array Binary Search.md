---
type: template
tags: ["Search"]
category: binary-search
categories: [binary-search]
---

# Array Binary Search

**오름차순 배열에서, 조건을 처음 만족하는 인덱스를 찾는다.**

![[attachments/array-binary-search-overview.svg]]

---

## Pseudocode

```text
left = 0
right = n

while (left < right) {
    mid = middle(left, right)

    if (condition(arr[mid])) {
        right = mid
    } else {
        left = mid + 1
    }
}

return left
```

## Java로 옮기기

| 의사코드 | 실제 코드 | 이유 |
| --- | --- | --- |
| `right = n` | `right = arr.length` | 조건에 맞는 원소가 없으면 n. mid는 n보다 작으므로 arr[n]은 읽지 않는다. |
| `middle(left, right)` | `left + (right - left) / 2` | 아래쪽 가운데. `[3,4]`에서 3을 골라야 오른쪽 끝을 줄일 수 있다. |
| `condition` · Lower Bound | `arr[mid] >= target` | 같은 값도 포함하는 **첫 위치**. |
| `condition` · Upper Bound | `arr[mid] > target` | 같은 값을 모두 지나친 **첫 위치**. |
| 참 → `right = mid` | `right = mid` | mid도 답일 수 있다. 남겨 두고 왼쪽을 찾는다. |
| 거짓 → `left = mid + 1` | `left = mid + 1` | mid까지 조건을 만족하지 못한다. 전부 버린다. |
| `return left` | `return left` | 두 끝이 모인 위치. 결과가 n이면 배열에 접근하지 않는다. |

## Java · Lower Bound

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
