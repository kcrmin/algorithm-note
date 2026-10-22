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
    Arrays.sort(sorted);
    Map<Integer, Integer> rank = new HashMap<>();
    for (int value : sorted) {
        // 중복에 새 번호를 주지 않아 같은 값은 같은 순위다.
        if (!rank.containsKey(value)) rank.put(value, rank.size());
    }
    int[] result = new int[arr.length];
    for (int i = 0; i < arr.length; i++) result[i] = rank.get(arr[i]);
    return result;
}
```

0부터의 순위 반환 · 대소 관계만 보존하며 거리는 보존하지 않는다 · 시간 O(n log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]
