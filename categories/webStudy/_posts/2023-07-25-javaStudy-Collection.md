---
layout: post
title:  "[Java] Collection"
description: >
    "Collection Framework"

hide_last_modified: false
---
* TOC
{:toc}
***
### Framework란 무엇인가
***
- Framework는 클래스들과 인터페이스들을 모아놓은 것으로 이미 만들어진 architecture

### Collection Framework
***
- 과거에도 java에 Arrays, Vectors, Hashtables와 같은 Method들이 존재했다. 
- 같은 표현을 하지만 interface가 만들어지지 않아 복잡하고 어려웠다. 
- 그래서 Collection interface에 list와 set 등을 구현하고 Map interface가 구현되어 크게 두 개의 interface가 만들어졌다. 

![GeeksforGeeks_CollectionFramework](/assets/img/javaStudy/geekforgeeks_HierachyOfTheCollecion.png)

### 장점
***
1. 일관적인 API
- 하나의 interface의 구현체들이기 때문에 비슷한 method로 구성되어있다.

2. 프로그래밍의 노력 감소   

3. 프로그램 속도와 퀄리티의 증가   

### 구성
1. List interface

```

    List<T> arr = new ArrayList<>();
    List<T> ll = new LinkedList<>();
    List<T> v = new Vector<>();

```

1.1. ArrayList는 동적 배열을 지원해준다.   
1.2. LinkedList는 선형 자료구조로 Element들이 연속적으로 저장되어 있는 것이 아니라 데이터 파트와 주소 파트로 구분되어 독립된 object로 만들어진다.    
1.3. Vector도 동적 배열을 지원해준다. 많은 연산이 필요할 때 빠른 편이다. 또한 ArrayList는 non-synchronized인데 비해 vector는 syncronized이다.    

> 👉syncronized란?   
> 간단하게, 모든 메소드들이 동기화 되어 thread-safe 상태가 된다는 것   

2. Stack, Queue, Deque interface

```
    // Iterator for the stack
    Iterator<String> itr = stack.iterator();
    
    Stack<String> stack = new Stack<>();

    // Queue
    Queue <T> pq = new PriorityQueue<>();
    Queue <T> ad = new ArrayDeque<>();
    
    // Deque
    Deque<T> ad = new ArrayDeque<>();

```

2.1. Stack은 last-in-first-out의 특징을 가진다.    
2.2. Queue는 기본적으로 FIFO(First In First Out)의 특징을 가진다.      
2.3. Deque은 Double-Ended-queue(양방향 큐)로 생각하면 된다.     

3. Set interface

```

    Set<T> hs = new HashSet<>();
    Set<T> hs = new LinkedHashSet<>();
    Set<T> hs = new TreeSet<>();

```
3.1. HashSet은 해시테이블 자료구조로 같은 순서로 입력되는 것이 보장되지 않고 해시코드에 따라 저장된다.    
3.2. LinkedHashSet은 HashSet과 동일하지만 Doubly linkedList를 활용하는 것이 특징이다.      
3.3. TreeSet은 입력 받을 때 중복을 검사한 후 자동으로 정렬되어 저장해준다. 즉 Sorted Set Interface를 받는다.    

4. Map interface

```

    Map<T> hm = new HashMap<>();
    Map<T> tm = new TreeMap<>();

```

4.1. HashMap은 데이터를 Key값과 Value값 쌍으로 저장해준다. 

### 참조
***
[Geeks For Geeks](https://www.geeksforgeeks.org/collections-in-java-2/)