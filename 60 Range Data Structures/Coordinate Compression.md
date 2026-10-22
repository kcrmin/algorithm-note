---
type: template
tags: ["DataStructures"]
aliases: ["Coordinate_Compression"]
category: range-queries
categories: [range-queries]
---

# Coordinate Compression

```java
int[] compress(int[] arr) {
    int[] sorted = arr.clone();
    // sort: 값의 대소 순서 만들기
    Arrays.sort(sorted);
    Map<Integer, Integer> rank = new HashMap<>();
    // rank: 중복을 제외하고 0부터 번호 부여
    for (int value : sorted) {
        if (!rank.containsKey(value)) rank.put(value, rank.size());
    }
    int[] result = new int[arr.length];
    // restore: 원본 순서로 순위 반환
    for (int i = 0; i < arr.length; i++) result[i] = rank.get(arr[i]);
    return result;
}
```

0부터의 순위 반환 · 대소 관계만 보존하며 거리는 보존하지 않는다 · 시간 O(n log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]
