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
// build: 앞부분 해시·BASE 거듭제곱 전처리
void build(String text) {
    prefix = new long[text.length() + 1];
    power = new long[text.length() + 1];
    power[0] = 1;
    for (int i = 0; i < text.length(); i++) {
        long value = text.charAt(i) - 'a' + 1;
        // append: 한 자리 밀고 새 문자 붙이기
        prefix[i + 1] = (prefix[i] * BASE + value) % MOD;
        power[i + 1] = power[i] * BASE % MOD;
    }
}
// hash: [left,right) 부분 해시
long hash(int left, int right) {
    // align: 뺄 앞부분의 자릿수 맞추기
    long removed = prefix[left] * power[right - left] % MOD;
    // remove: 앞부분 제거 후 음수 보정
    return (prefix[right] - removed + MOD) % MOD;
}
```

소문자 a~z, 0-index [left,right) · 충돌 가능하므로 길이·원문도 확인 · 전처리·공간 O(n), Hash O(1).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
