---
type: template
aliases: ["Counting Sort", "Bucket"]
---

# Frequency Array and Buckets

```java
int[] sort(int[] arr, int maxValue) {
    int[] count = new int[maxValue + 1];
    for (int value : arr) count[value]++;  // ①
    int[] result = new int[arr.length];
    int index = 0;
    for (int value = 0; value <= maxValue; value++) {
        for (int repeat = 0; repeat < count[value]; repeat++) {
            result[index++] = value;      // ②
        }
    }
    return result;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 값 → 인덱스 | 비교하거나 탐색하지 않고 해당 값의 빈도에 바로 접근한다. |
| ② 작은 값부터 복원 | value가 증가하는 순서이므로 출력은 자동으로 정렬된다. |
| 중첩 반복이어도 O(n+K) | 안쪽 반복의 총횟수는 모든 빈도의 합 n이다. |

**예시**

```text
입력          [3,1,3,2]
값 value       0  1  2  3
횟수 count     0  1  1  2
작은 값부터 횟수만큼 출력 → [1,2,3,3]
```

**적용** 값이 작은 정수 범위에 모여 있으면, 값을 배열 인덱스로 삼아 횟수를 세거나 정렬한다.

**준비** 0 ≤ arr[i] ≤ maxValue. 정렬된 새 배열을 반환한다.

**주의** K=maxValue+1이 크면 메모리 낭비가 크다. O(n+K) 시간·공간.

[[Hashing]] · [[Coordinate Compression]] · [[Sorting]]
