---
type: template
aliases: ["Counting Sort", "Bucket"]
---

# Frequency Array and Buckets

```java
// 0≤arr[i]≤maxValue. 값 범위가 작을 때 사용하며, 정렬된 새 배열을 반환한다.
int[] sort(int[] arr, int maxValue) {
    int[] count = new int[maxValue + 1]; // count[value] = 해당 값의 등장 횟수.
    // 값 자체를 인덱스로 쓰므로 비교·탐색이 필요 없다.
    for (int value : arr) count[value]++;
    int[] result = new int[arr.length];
    int index = 0;
    // 작은 값부터 복원하면 자동으로 정렬된다.
    for (int value = 0; value <= maxValue; value++) {
        // 안쪽 반복의 총합은 n. 전체 O(n+값 범위).
        for (int repeat = 0; repeat < count[value]; repeat++) {
            result[index++] = value;
        }
    }
    return result;
}
```

**예시**

```text
입력          [3,1,3,2]
값 value       0  1  2  3
횟수 count     0  1  1  2
작은 값부터 횟수만큼 출력 → [1,2,3,3]
```

**주의** K=maxValue+1이 크면 메모리 낭비가 크다. O(n+K) 시간·공간.

[[Hashing]] · [[Coordinate Compression]] · [[Sorting]]
