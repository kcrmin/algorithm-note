---
type: template
aliases: ["Longest Increasing Subsequence"]
---

# LIS

```java
// 엄격히 증가하는 부분 수열의 길이. 같은 값으로는 길이를 늘리지 않는다.
int[] tails = new int[arr.length]; // tails[i] = 길이 i+1인 수열의 가능한 최소 끝값.
int size = 0;
for (int value : arr) {
    int left = 0, right = size;
    while (left < right) {
        int mid = left + (right - left) / 2;
        // 같은 값도 교체하도록 value 이상인 첫 자리를 찾는다.
        if (tails[mid] >= value) right = mid;
        else left = mid + 1;
    }
    tails[left] = value; // 같은 길이라면 끝값이 작을수록 나중에 붙일 값이 많다.
    // 기존 끝값보다 커서 새 길이를 만들 수 있을 때만 연장한다.
    if (left == size) size++;
}
// 답은 size. tails 자체가 실제로 선택한 수열은 아니다.
```

**예시**

```text
입력          2      5        3         4
최소 끝값    [2] → [2,5] → [2,3] → [2,3,4]
                             ↑ 교체      ↑ 연장
길이          1      2        2         3
```

**주의** tails 자체가 실제 선택한 수열은 아니다. 길이 답은 size. O(n log n) 시간, O(n) 공간.

[[Array Binary Search]] · [[Binary Search - First True]] · [[DP]]
