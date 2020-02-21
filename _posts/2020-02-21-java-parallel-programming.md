---
layout: post
title: 자바 병렬 프로그래밍
date: 2020-02-21 19:49:17 +0900
categories: java
---

# Java Parallel Programming

## 병렬 테스크 (Runnable, Callable)
Runnable은 리턴값이 없다. 결과를 계산하기 위해서는 Callable을 사용해야한다. Callable은 결과를 반환하는 Future 객체를 리턴하고, 개발자는 이 객체를 통해 값을 돌려받을수 있다.

Future의 get메서드는 결과가 나오거나 타임아웃이 발생할 때까지 블럭된다. 만약 call 메서드에서 예외가 발생하면, get메서드는 ExecutionException을 일으킨다. cancel 메서드는 태스크가 실행중인 경우는 인터럽트를 발생시키고, 실행중인 상태가 아닌 경우는 캔슬되지 않는다. 

서브테스크가 복수인 경우는 Callable을 Collection으로 만들어 executor.invokeAll로 실행할 수 있다. 이때 결과값은 복수개의 Future 객체(List<Future>)를 통해 전달받을 수 있다.

executor의 invokeAny 메서드는 invokeAll과 흡사하지만 테스크중 하나가 완료되면 나머지 테스크를 취소하고, 그 즉시 모든 결과를 반환한다.

## Thread-safety

### Visibility
모던 컴파일러, 가상머신, 프로세서에서 발생하는 최적화에 의해 특정 변수가 모든 스레드에서 동일하지 않거나, 연산 결과를 예측하지 못하는 경우가 많다. 온보드 메모리 캐시나 프로세스의 레지스터에 저장되어 있는 값을 바라보고 있을수도 있고, 명령어가 재정렬되어 명령의 순서가 보장되지 않을수도 있다. 기본적으로 이러한 최적화들은 동시 메모리 접근이 없다고 가정하기 때문이다. 불변값 (Immutable)을 사용하거나, static 초기화, volatile 선언, lock의 해제이후에 값의 가시성이 보장된다.

### Race Condition
동시에 같은 로직을 여러 스레드에서 접근하는 경우, atomic 하지 않은 연산은 안전한 결과를 보장하지 않는다. 레이스 컨디션을 회피하기 위한 전략은 여러가지가 있다.

1. Confinement
테스크사이에 값을 공유할 필요가 없을때, 각각의 연산결과를 저장하는 변수를 만들고 이를 취합해 연산결과를 만들어낸다.

2. Immutability
불변하는 객체를 사용한다.

3. Locking
임계영역을 통해 동시 접근을 막는다.

4. Partitioning
데이터를 나눠서 각기 다른 조각에 접근할 수 있게 한다.

## Parallel Stream

스트림의 병렬 라이브러리를 통해 집한의 연산을 병렬화 할 수 있다. 예를들어 문자열의 목록에서 'A'로 시작하는 문자열의 개수를 찾기 위해서는 다음과 같이 할수 있다.

`strings.parallelStream().filter(s -> s.startWith("A")).count()`

병렬 스트림을 사용할 때 filter, map 등의 인수로 람다를 사용해 객체를 변경하는 것은 위험하므로 주의를 요한다.

병렬 스트림이 효과적인 경우는 다음과 같다.

* 병렬처리에서 발생하는 오버헤드를 상쇄할 만큼 데이터가 클때
* 데이터가 메모리에 위치할 때
* 배열이나 트리처럼 분할 가능한 스트림일때, 연결 리스트같은 경우 분할이 어렵다.
* 블록되지 않는 연산일때

Arrays는 병렬 연산을 지원하는 메서드를 가지고 있다. 예를들어 setAll은 연산결과를 배열에 병렬적으로 저장할 수 있다.

`Arrays.parallelSetAll(values, i -> i * 2)`

`Arrays.parallelSort(words, Comparator.comparing(String::length))`

## Thread-Safe Collections

### ConcurrentHashMap
많은 수의 concurrent reader와 일정 개수의 concurrent writer를 통해 복수의 스레드가 동시에 접근할 수 있다. compute, merge등을 통해 읽고 쓰는 과정을 atomic하게 할수 있다. 둘의 차지머은 compute는 key를 다루지만, merge는 key가 존재하지 않을때 초기값을 선언하고, 기존값과 새로운값을 이용해 새로운 값을 기존 key에 저장할 수 있다.

`map.merge(key, 0, (existingValue, initialValue) -> existingValue + 1)`

ConcurrentHashSet은 존재하지 않는데 이를 위해 더미값으로 Boolean.TRUE를 가지는 newKeySet을 사용할 수 있다.

`Set<String> set = ConcurrentHashMap.newKeySet()`

### BlockingQueue
큐가 비어있거나, 가득 차있을때 wait하는 메서드(put, take)를 제공한다. poll과 peek은 실패시 null을 반환하기 때문에 블럭킹큐에 null을 삽입할 수 없다.

## Atomic
AtomicInteger등의 atomic한 처리를 제공하는 클래스들은 optimistic하게 수행되기 때문에 대규모의 스레드에서 동시에 접근하는 경우 성능이 떨어질수 있으므로 주의해야한다. 높은 레이스 컨디션이 예상될 경우 LongAdder나 LongAccumulator 같은 클래스들의 사용을 고려하는것이 좋다.

## Lock
외부의 인터럽션없이 온전히 수행되는 블럭을 임계영역(Critical Section)이라고 한다.

```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    count++
} finally {
    lock.unlock();
}
```

## Synchronization
synchronization 블럭은 고유락(Intrinsic Lock)을 사용한다. 일반적으로는 객체 인스턴스를 락 객체로 사용한다. synchronization method 의 경우 해당 메서드를 호출한 this 객체에 잠금을 사용한다.

## wait & notify
wait() 메서드를 호출하면 해당 스레드는 다른 스레드에서 notify를 할때까지 조건대기하게 된다. notify는 한 스레드만 블럭에서 해제하지만 이 경우 선택받은 스레드가 오퍼레이션을 재게할 수 없는 상황인 경우에 다시 notify를 전달하지는 않기 때문에 주의해야한다.

## Interrupt
대기 혹은 잠든 스레드에 인터러트가 발생하면 즉시 재활성화 하지만 인터럽트 여부를 확인할 수 없다. 그리고 InterruptedException이 발생하기 때문에 이를 적절히 잡아서 처리해야한다. 동작중인 스레드는 Thread.currentThread().isInterrupted()로 인터럽트 여부를 확인할 수 있다.

## Daemon
다른 스레드에 서비스를 제공하는 역할을 하는 스레드. vm에 데몬 스레드만 남아있을경우 이 가상머신은 종료되게 된다. 데몬 스레드를 만들기 위해서는 스레드 시작전에 thread.setDeamon(true)을 호출한다.