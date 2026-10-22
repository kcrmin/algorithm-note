---
type: template
aliases: ["Counting Sort", "Bucket"]
category: core-collections
categories: [core-collections]
---

# Frequency Array and Buckets

```java
int[] sort(int[] arr, int maxValue) {
    int[] count = new int[maxValue + 1];
    // count: 값별 등장 횟수 집계
    for (int value : arr) count[value]++;
    int[] result = new int[arr.length];
    int index = 0;
    // restore: 작은 값부터 등장 횟수만큼 복원
    for (int value = 0; value <= maxValue; value++) {
        for (int repeat = 0; repeat < count[value]; repeat++) result[index++] = value;
    }
    return result;
}
```

0≤arr[i]≤maxValue, 작은 값 범위에 사용 · K=maxValue+1, 시간·공간 O(n+K).

**분류** [[00 Maps/Core Collections|Core Collections]]
