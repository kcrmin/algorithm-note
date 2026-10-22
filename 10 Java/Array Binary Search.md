---
type: template
tags: ["Search"]
---

# Array Binary Search

```java
// arr는 오름차순. target 이상인 첫 인덱스, 없으면 n을 반환.
// 끝 다음 위치 n도 답으로 남겨 두면, 못 찾은 경우 n으로 모인다.
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    // mid도 첫 위치일 수 있어 포함한다. upper bound는 >로 바꾼다.
    if (arr[mid] >= target) right = mid;
    else left = mid + 1; // mid까지는 모두 target보다 작아 버린다.
}
return left;
```

**예시**

```text
index   0  1  2  3
arr     1  3  3  5    target=3
           ↑     ↑
         lower  upper   → 같은 값 개수 = 3−1 = 2
```

**주의** 정렬이 먼저다. 조회 O(log n).

[[Binary Search - First True]] · [[Binary Search - Last True]] · [[LIS]] · [[Sorting]]
