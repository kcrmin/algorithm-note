---
type: template
tags: ["Strings"]
aliases: ["Rolling Hash"]
category: string-algorithms
categories: [string-algorithms]
---

# String Hashing

```java
long MOD = 1_000_000_007L, BASE = 31;
long[] prefix, power;
void build(String text) {
    prefix = new long[text.length() + 1];
    power = new long[text.length() + 1];
    power[0] = 1;
    for (int i = 0; i < text.length(); i++) {
        long value = text.charAt(i) - 'a' + 1;
        prefix[i + 1] = (prefix[i] * BASE + value) % MOD;
        power[i + 1] = power[i] * BASE % MOD;
    }
}
long hash(int left, int right) {
    // 앞부분을 구간 길이만큼 밀어 자릿수를 맞춘 뒤 뺀다.
    long removed = prefix[left] * power[right - left] % MOD;
    return (prefix[right] - removed + MOD) % MOD; // 음수를 [0,MOD)로 보정한다.
}
```

소문자 a~z, 0-index [left,right) · 충돌 가능하므로 길이·원문도 확인 · 전처리·공간 O(n), Hash O(1).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
