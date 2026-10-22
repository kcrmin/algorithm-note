---
type: template
aliases: ["Longest Increasing Subsequence"]
category: dynamic-programming
categories: [dynamic-programming, binary-search]
---

# LIS

```java
int lis(int[] arr) {
    // state: tails[i]=길이 i+1의 최소 끝값
    int[] tails = new int[arr.length];
    int size = 0;
    for (int value : arr) {
        // find: value 이상인 첫 자리, 같은 값이면 교체
        int left = 0, right = size;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails[mid] >= value) right = mid;
            else left = mid + 1;
        }
        // replace: 끝값을 낮춰 다음 확장 여지 확보
        tails[left] = value;
        // extend: 새 자리에 닿았을 때만 길이 증가
        if (left == size) size++;
    }
    return size;
}
```

엄격히 증가하는 부분 수열의 길이 · tails 자체는 실제 선택한 수열이 아니다 · 시간 O(n log n), 공간 O(n).

**분류** [[00 Maps/Dynamic Programming|Dynamic Programming]]

**사용 원리** [[10 Java/Array Binary Search|Array Binary Search]]
