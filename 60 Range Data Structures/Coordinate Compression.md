---
type: template
tags: ["DataStructures"]
aliases: ["Coordinate_Compression"]
---

# Coordinate Compression

```java
// java.util.* 사용. 값의 대소 관계만 보존하는 0부터의 번호를 반환한다.
int[] compress(int[] arr) {
    int[] sorted = arr.clone();
    Arrays.sort(sorted); // 작은 값부터 번호를 붙여야 대소 관계가 유지된다.
    Map<Integer, Integer> rank = new HashMap<>();
    for (int value : sorted) {
        if (!rank.containsKey(value)) {
            // 중복에는 새 번호를 주지 않아 같은 값이 같은 순위다.
            rank.put(value, rank.size());
        }
    }
    int[] result = new int[arr.length];
    for (int i = 0; i < arr.length; i++) {
        // 원본 순서로 읽어야 입력 인덱스에 맞는 결과가 된다.
        result[i] = rank.get(arr[i]);
    }
    return result;
}
```

**예시**

```text
원본        [100, -5, 100, 20]
정렬·중복제거 [-5, 20, 100] → 번호 [0, 1, 2]
원본 순서로  [  2,  0,   2,  1]
               같은 값 → 같은 번호
```

**주의** 순서는 보존하지만 거리 `20-(-5)=25`는 보존하지 않는다. O(N log N) 시간·O(N) 공간.

[[Sorting]] · [[Hashing]] · [[Fenwick Tree]] · [[Segment Tree]]
