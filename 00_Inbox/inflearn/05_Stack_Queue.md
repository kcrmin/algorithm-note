---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 5 - Stack Queue

## 1. Stack 기초 (LIFO)

### 기본 조작
| 코드                                      |   요약 / 결과   | 예시                |
|:----------------------------------------|:-----------:|:------------------|
| `Stack<Integer> stack = new Stack<>();` |     선언      | —                 |
| `stack.push(10);` `stack.push(20);`     |     삽입      | `[20, 10]`        |
| `stack.peek();`                         |   맨 위 조회    | `20` → `[20, 10]` |
| `stack.pop();`                          | 맨 위 제거 + 반환 | `20` → `[10]`     |
| `stack.isEmpty();`                      |   비었는지 확인   | false             |
| `stack.size();`                         |     크기      | 1                 |

## 2. Queue 기초

### 기본 조작
| 코드                                       |   요약 / 결과   | 예시                |
|:-----------------------------------------|:-----------:|:------------------|
| `Queue<Integer> q = new LinkedList<>();` |     선언      |                   |
| `q.offer(10);` `q.offer(20);`            |     삽입      | `[10, 20]`        |
| `q.peek();`                              |   맨 앞 조회    | `10` → `[10, 20]` |
| `q.poll();`                              | 맨 앞 제거 + 반환 | `10` → `[20]`     |
| `q.isEmpty();`                           |   비었는지 확인   | false             |
| `q.size();`                              |     크기      | 1                 |
> `add()` / `remove()` 대신 `offer()` / `poll()` 권장

## 2. Deque 기초

### 기본 조작
| 코드                                        |   요약 / 결과   | 예시 |
|:------------------------------------------|:-----------:|:---|
| `Deque<Integer> dq = new ArrayDeque<>();` |     선언      | —  |
| `dq.addFirst(x);`                         | Stack push  | —  |
| `dq.removeFirst();`                       |  Stack pop  | —  |
| `dq.addLast(x);`                          | Queue offer | —  |
| `dq.removeFirst();`                       | Queue poll  | —  |
| `peekFirst();`                            |     조회      | —  |
| `peekLast();`                             |     조회      | —  |


## 3. 기본 코드
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        
    }
}
```


## 4. 주의 포인트
|  코드 / 개념   | 요약                                         |
|:----------:|:-------------------------------------------|
|   Stack    | 느림, 대신 ` ArrayDeque` 사용                    |
|   Queue    | add/remove는 예외 던짐, 대신 offer/poll 사용 (null) |
| ArrayDeque | null 저장 불가                                 |
|  Queue 구현  | `LinkedList` / `ArrayDeque`로 구현            |


## **핵심 요약**
- Stack → 후입선출(LIFO): push, pop, peek 
- Queue → 선입선출(FIFO): offer, poll, peek 
- Deque → 양방향 (Stack+Queue 통합형)
- ArrayDeque 하나로 모두 처리 가능, 속도 가장 빠름
  - Deque 인터페이스 + ArrayDeque 구현체 → 가장 빠르고 안전
  - Stack 사용: push, pop, peek
  - Queue 사용: offer, poll, peek

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section05_stack_queue)**
| 분류  | 문제 이름                                                                              | 포인트 메모                            |
|:---:|:-----------------------------------------------------------------------------------|:----------------------------------|
| 응용  | [05-03 크레인 인형뽑기(카카오)](../../src/inflearn/section05_stack_queue/INF_S05_P03.java)   | 좋은 문제                             |
| 응용  | [05-04 후위식 연산(postfix)](../../src/inflearn/section05_stack_queue/INF_S05_P04.java) | 좋은 문제, switch case 활용, char → int |
| 응용  | [05-05 쇠막대기](../../src/inflearn/section05_stack_queue/INF_S05_P05.java)            | 문제 잘 이해하기                         |
| 고난도 | [05-08 응급실](../../src/inflearn/section05_stack_queue/INF_S05_P08.java)             | 좋은 문제                             |