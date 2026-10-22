---
type: template
tags: ["Search"]
---

# Array Binary Search

```java
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] >= target) right = mid;  // ①
    else left = mid + 1;
}
return left;
```

| 암기 포인트 | 이유 |
|---|---|
| `>= target` → right=mid | mid도 첫 위치일 수 있어 남긴다. |
| `right = n` | 못 찾은 경우까지 n 한 곳으로 모은다. mid는 n을 검사하지 않는다. |
| `>=`를 `>`로 바꾸기 | 같은 값까지 지나면 upper bound다. |

**예시**

```text
index   0  1  2  3
arr     1  3  3  5    target=3
           ↑     ↑
         lower  upper   → 같은 값 개수 = 3−1 = 2
```

**적용** 오름차순 배열에서 target 이상인 첫 위치를 찾는다. 없으면 배열 길이 n.

**준비** `arr`, `target`을 받는 int 메서드 내부.

**주의** 정렬이 먼저다. 조회 O(log n).

[[Binary Search - First True]] · [[Binary Search - Last True]] · [[LIS]] · [[Sorting]]
