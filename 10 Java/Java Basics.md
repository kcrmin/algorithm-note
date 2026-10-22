---
type: template
tags: ["Java"]
---

# Java Basics

```java
// 배열은 길이 필드, 문자열·컬렉션은 길이를 묻는 메서드를 쓴다.
int n = arr.length;
int m = s.length();
int size = list.size();
// ==는 같은 객체인지 비교한다. 글자 내용이 같은지는 equals로 본다.
boolean same = s.equals(t);
// int 계산 뒤에 변환하면 이미 넘칠 수 있어, 첫 피연산자부터 long으로 바꾼다.
long sum = (long) a + b;
long product = (long) a * b;
```

**예시**

```text
길이: 배열 length / 문자열 length() / 컬렉션 size()
"abc"와 새로 만든 "abc" → 내용 비교는 equals
20억 + 20억 → long으로 먼저 변환 → 40억
            → int로 먼저 계산   → 이미 넘침
```

**주의** 마지막 인덱스는 길이−1. char 비교는 `'a' == ch`로 한다.

[[Fast Input and Output]] · [[Sorting]] · [[Hashing]] · [[Stack and Queue]]
