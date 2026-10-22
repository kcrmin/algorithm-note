---
type: template
aliases: ["Longest Increasing Subsequence"]
---

# LIS

**언제** 원래 순서를 지키며 고른 엄격히 증가하는 부분 수열의 최대 길이를 구한다.

```text
입력          2      5        3         4
최소 끝값    [2] → [2,5] → [2,3] → [2,3,4]
                             ↑ 교체      ↑ 연장
길이          1      2        2         3
```

**코드** `tails[i]` = 길이 i+1인 증가 수열의 가능한 최소 끝값.

```java
int[] tails = new int[arr.length];
int size = 0;
for (int value : arr) {
    int left = 0, right = size;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (tails[mid] >= value) right = mid; // ①
        else left = mid + 1;
    }
    tails[left] = value;                     // ②
    if (left == size) size++;
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① value 이상인 첫 자리 | 같은 값은 교체해야 중복으로 길이가 늘지 않는다. |
| ② 더 작은 끝값으로 교체 | 길이가 같으면 끝값이 작을수록 뒤에 붙일 수가 많다. |
| left == size면 연장 | 기존 끝값보다 커서 길이 하나를 새로 만들 수 있다. |

**주의** tails 자체가 실제 선택한 수열은 아니다. 길이 답은 size. O(n log n) 시간, O(n) 공간.

[[Array Binary Search]] · [[Binary Search - First True]] · [[DP]]
