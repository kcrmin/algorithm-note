---
type: template
tags: ["Search"]
category: binary-search
categories: [binary-search]
---

# Array Binary Search

```java
// 못 찾은 답도 n으로 모이도록 끝 다음 칸을 둔다.
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    // mid도 답 후보라 포함. >로 바꾸면 upper bound.
    if (arr[mid] >= target) right = mid;
    else left = mid + 1; // mid까지는 모두 작으므로 버린다.
}
return left;
```

**조건** arr 오름차순. target 이상 첫 인덱스, 없으면 n. O(log n).

`upper - lower` = target 개수.

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - First True|Binary Search - First True]]
