---
type: template
tags: ["Strings"]
aliases: ["Rolling Hash"]
---

# String Hashing

```java
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
    long removed = prefix[left] * power[right - left] % MOD; // ①
    return (prefix[right] - removed + MOD) % MOD; // ②
}
```

| 암기 포인트 | 이유 |
|---|---|
| 이전 값 × BASE + 문자 | 앞부분을 한 자리 밀고 새 문자를 끝에 붙인다. |
| ① 길이만큼 거듭제곱 | 제거할 앞부분의 자릿수를 전체 Hash와 맞춘다. |
| ② MOD 한 번 더하기 | 두 나머지의 차는 음수일 수 있다. [0,MOD)로 돌린다. |

**예시**

```text
a=1, b=2, c=3, BASE=31
abc = 1×31² + 2×31 + 3 = 1026
 a  = 1             bc = 1026 - 1×31² = 65
hash([l,r)) = prefix[r] - prefix[l] × BASE^(r-l)
                 전체     앞부분을 같은 자리로 밀어 제거
```

**적용** 고정된 문자열의 여러 부분 문자열을 빠르게 비교할 때. Hash는 일치 후보를 거르는 값이다.

**준비** 소문자 a~z. 필드 `long[] prefix, power;`와 `long MOD=1_000_000_007L, BASE=31;`. **0-index·[left,right)**.

**주의** **길이도 비교**한다. Hash가 같아도 충돌할 수 있어 정확한 판정은 실제 문자까지 확인한다. 전처리·공간 O(N)·Hash O(1).

[[Prefix Sum]] · [[KMP]] · [[Hashing]]
