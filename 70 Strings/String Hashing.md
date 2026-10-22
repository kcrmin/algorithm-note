---
type: template
tags: ["Strings"]
aliases: ["Rolling Hash"]
---

# String Hashing

```java
// 소문자 a~z. 필드 long[] prefix, power; long MOD=1_000_000_007L, BASE=31; 준비.
// 같은 길이와 같은 Hash여도 충돌할 수 있다. 정확한 판정은 원문도 비교한다.
// build(text) 후 hash(left,right). 범위는 0-index·[left,right).
// prefix[i]는 앞 i글자의 Hash다.
void build(String text) {
    prefix = new long[text.length() + 1];
    power = new long[text.length() + 1];
    power[0] = 1; // BASE^0=1이어야 빈 앞부분과 길이 0 구간도 같은 식으로 처리된다.
    for (int i = 0; i < text.length(); i++) {
        long value = text.charAt(i) - 'a' + 1;
        // 앞부분을 한 자리 밀고 새 문자를 붙인다.
        prefix[i + 1] = (prefix[i] * BASE + value) % MOD;
        power[i + 1] = power[i] * BASE % MOD;
    }
}
long hash(int left, int right) {
    // 앞부분을 구간 길이만큼 밀어 자릿수를 맞춘다.
    long removed = prefix[left] * power[right - left] % MOD;
    // 뺄셈이 음수일 수 있어 MOD를 더해 [0,MOD)로 돌린다.
    return (prefix[right] - removed + MOD) % MOD;
}
```

**예시**

```text
a=1, b=2, c=3, BASE=31
abc = 1×31² + 2×31 + 3 = 1026
 a  = 1             bc = 1026 - 1×31² = 65
hash([l,r)) = prefix[r] - prefix[l] × BASE^(r-l)
                 전체     앞부분을 같은 자리로 밀어 제거
```

**주의** **길이도 비교**한다. Hash가 같아도 충돌할 수 있어 정확한 판정은 실제 문자까지 확인한다. 전처리·공간 O(N)·Hash O(1).

[[Prefix Sum]] · [[KMP]] · [[Hashing]]
