---
type: template
aliases: ["Longest Increasing Subsequence"]
category: dynamic-programming
categories: [dynamic-programming, binary-search]
---

# LIS

```java
int lis(int[] arr) {
    int[] tails = new int[arr.length]; // tails[i]: 길이 i+1의 최소 끝값.
    int size = 0;
    for (int value : arr) {
        int left = 0, right = size;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails[mid] >= value) right = mid; // 같은 값은 길이를 늘리지 않는다.
            else left = mid + 1;
        }
        tails[left] = value; // 끝값이 작을수록 다음 값을 붙이기 쉽다.
        if (left == size) size++;
    }
    return size;
}
```

엄격히 증가하는 부분 수열의 길이 · tails 자체는 실제 선택한 수열이 아니다 · 시간 O(n log n), 공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[10 Java/Array Binary Search|Array Binary Search]]
