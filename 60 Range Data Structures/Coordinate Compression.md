---
type: template
tags: ["DataStructures"]
aliases: ["Coordinate_Compression"]
---

# Coordinate Compression

**언제** 값은 너무 커서 배열 인덱스로 못 쓰지만, 값의 대소 관계만 필요할 때.

```text
원본        [100, -5, 100, 20]
정렬·중복제거 [-5, 20, 100] → 번호 [0, 1, 2]
원본 순서로  [  2,  0,   2,  1]
               같은 값 → 같은 번호
```

**코드** 반환 배열은 원본과 같은 순서이며, 번호는 0부터다. `java.util.*` 사용.

```java
int[] compress(int[] arr) {
    int[] sorted = arr.clone();
    Arrays.sort(sorted);
    Map<Integer, Integer> rank = new HashMap<>();
    for (int value : sorted) {
        if (!rank.containsKey(value)) {
            rank.put(value, rank.size()); // ①
        }
    }
    int[] result = new int[arr.length];
    for (int i = 0; i < arr.length; i++) {
        result[i] = rank.get(arr[i]); // ②
    }
    return result;
}
```

| 암기 포인트 | 이유 |
|---|---|
| 정렬한 값을 순회 | 작은 값부터 번호를 붙여야 대소 관계가 유지된다. |
| ① 처음 본 값만 | 중복에도 번호를 늘리면 같은 값의 순위가 달라진다. |
| ② 원본을 다시 읽기 | 정렬 순서가 아닌 입력 인덱스에 맞춰 답을 돌려준다. |

**주의** 순서는 보존하지만 거리 `20-(-5)=25`는 보존하지 않는다. O(N log N) 시간·O(N) 공간.

[[Sorting]] · [[Hashing]] · [[Fenwick Tree]] · [[Segment Tree]]
