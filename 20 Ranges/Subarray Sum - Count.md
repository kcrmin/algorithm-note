---
type: template
tags: ["Ranges"]
aliases: ["Prefix Sum and HashMap - Count"]
---

# Subarray Sum - Count

```java
// 합이 target인 비어 있지 않은 연속 구간의 개수. 음수·0도 허용.
Map<Long, Long> count = new HashMap<>();
count.put(0L, 1L); // 읽기 전 합 0은 한 번 있었다. 처음부터인 구간도 센다.
// 구간 개수는 n(n+1)/2까지 커질 수 있어 합과 답을 long으로 둔다.
long sum = 0, answer = 0;
for (int num : arr) {
    sum += num;
    // 현재 합-이전 합=target이므로, 이전 합 sum-target의 횟수를 더한다.
    // 그 합이 두 번 있었다면 시작점이 다른 정답 두 개다.
    answer += count.getOrDefault(sum - target, 0L);
    // 현재 합은 나중에 저장해 길이 0인 구간을 제외한다.
    count.put(sum, count.getOrDefault(sum, 0L) + 1);
}
return answer;
```

**예시**

```text
arr=[1,-1,1], target=1, 시작 전 count={0:1}
현재 합     1     0     1
필요한 합   0    -1     0
이전 등장   1회   0회   2회  → 답 3
마지막의 0 두 번 = 시작 전 합0 + [1,-1]의 합0
```

**주의** 답은 n(n+1)/2까지 커져 answer도 long. 평균 시간·공간 O(n).

[[Subarray Sum - Existence]] · [[Prefix Sum]] · [[Hashing]]
