---
type: template
tags: ["Search"]
category: binary-search
categories: [binary-search]
---

# Array Binary Search

```java
int left = 0, right = arr.length; // range: 없을 때의 답 n도 후보에 포함
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] >= target) right = mid; // keep: mid도 첫 위치 후보
    else left = mid + 1; // drop: mid까지 target보다 작음
}
return left; // answer: 첫 >= target; >로 바꾸면 upper bound
```

**조건** arr 오름차순. target 이상 첫 인덱스, 없으면 n. O(log n).

`upper - lower` = target 개수.

**분류** [[00 Maps/Binary Search|Binary Search]]

**비교** [[20 Ranges/Binary Search - First True|Binary Search - First True]]
