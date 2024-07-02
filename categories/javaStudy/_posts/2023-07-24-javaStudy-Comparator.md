---
layout: post
title:  "[Java] Comparator"
description: >
    "Comparator"

hide_last_modified: false
---
* TOC
{:toc}
***
### 정의
***

```

    @FunctionalInterface
    public interface Comparator<T>

```

- 비교 함수로 몇몇 객체에 전체 순서를 조절한다.

### 특징
***
- Comparators는 자료구조와 sort method에서 좀 더 정밀한 정렬을 할 수 있도록 도와준다.   
- 기본적으로 sort method는 Compare method를 호출해서 사용된다. 두 개의 elements를 비교하기 위해 Compare method는 **비교하는 대상보다 작으면 -1, 같으면 0, 더 크다면 1을 반환한다.**

```

    public void sort(List list, ComparatorClass c)

```

- 주어진 List를 정렬하기 위해서 ComparatorClass는 위의 Comparator interface를 implements 하고 있다. 
- 그렇기 때문에 Comparator interface에 compare method를 **반드시**  재정의 해줘야 한다. 

### 예시
***
- 해당 코드는 정렬을 위해 Comparator를 활용한 코드 예시이다. 

```

    public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());

        String[] words = new String[N];
        for (int i = 0; i < N; i++){
            st = new StringTokenizer(br.readLine());
            words[i] = st.nextToken();
        }

        Arrays.sort(words, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if(o1.length() == o2.length()){
                    return o1.compareTo(o2);
                } else {
                    return o1.length() - o2.length();
                }
            }
        });
    }

```

- 문자열의 길이에 따라 정렬하는 로직을 구현해보았다.

### 참조
***
[Oracle docs](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)   
[Geeks for Geeks](https://www.geeksforgeeks.org/comparator-interface-java/)