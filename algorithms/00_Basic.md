# Basic of Algorithms

### 알고리즘 공부 방법

1. 알고리즘이나 문제를 푸는 방법을 이해 - 완벽하지 않거나 일부만 이해해도 됨.
2. 관련 문제를 풀어본다. - 한 문제는 길어야 2시간 정도만 고민, 모르겠으면 포기하고 정답 소스를 보거나 풀이를 본다.
3. 1 ~ 2를 반복하며, 모르는 것은 질문한다.
4. 다시 알고리즘을 이해해보고 문제를 다시 풀어본다.

## Queue, Deque

### Queue

* 한쪽 끝에서만 자료를 넣고 다른 한쪽 끝에서만 뺄 수 있는 자료구조
* First In First Out(FIFO)
* push : 큐에 자료를 넣는 연산
* pop : 큐에서 자료를 빼는 연산
* front : 큐의 가장 앞에 있는 자료를 보는 연산
* back : 큐의 가장 뒤에 있는 자료를 보는 연산
* empty : 큐가 비어있는지 아닌지를 알아보는 연산
* size : 큐에 저장되어 있는 자료의 개수를 알아보는 연산

큐는 C++의 경우에는 STL의 queue, 자바의 경우에는 java.util.Queue를 사용

Q. 조세퍼스 문제

[조세퍼스 문제](https://www.acmicpc.net/problem/1158)

1번부터 N번까지 N명의 사람이 원을 이루면서 앉아있고, 양의 정수 M(≤ N)이 주어진다. 이제 순서대로 M번째 사람을 제거한다. 한 사람이 제거되면 남은 사람들로 이루어진 원을 따라 이 과정을 계속해 나간다. 이 과정은 N명의 사람이 모두 제거될 때까지 계속된다. 원에서 사람들이 제거되는 순서를 (N, M)-조세퍼스 순열이라고 한다. 예를 들어 (7, 3)-조세퍼스 순열은 <3, 6, 2, 7, 5, 1, 4>이다.

N과 M이 주어지면 (N,M)-조세퍼스 순열을 구하는 프로그램을 작성하시오.

입력 : 첫째 줄에 N과 M이 빈 칸을 사이에 두고 순서대로 주어진다. (1 ≤ M ≤ N ≤ 5,000)

출력 : <3, 6, 2, 7, 5, 1, 4> 과 같이 조세퍼스 순열을 출력한다.

시간복잡도 : 각각의 사람들에 대해서 총 N번 순회를 하고 제거를 하기 때문에 시간 복잡도 O(NM)을 갖는다.

문제에 나와있는대로 큐를 이용해서 시뮬레이션하면 된다.
```java
queue.offer() // 제일 뒤에 값을 추가
queue.poll() // 맨 앞의 값을 가져오고 큐에서 제거
queue.element() // 맨 앞의 값을 가져옴
```


```java
import java.util.*;

public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int m = sc.nextInt();
        StringBuilder sb = new StringBuilder();
        sb.append('<');
        Queue<Integer> queue = new LinkedList<Integer>();
        for (int i=1; i<=n; i++) {
            queue.offer(i);
        }
        for (int i=0; i<n-1; i++) {
            for (int j=0; j<m-1; j++) {
                queue.offer(queue.poll()); 
                //제일 앞의 값을 제거하고 뒤에 붙이는 것을 m번 반복 
            }
            sb.append(queue.poll() + ", ");
            //m번째 값이 제일 앞에 위치하므로 sb에 붙이고 제거
        }
        sb.append(queue.poll() + ">");
        System.out.println(sb);
    }
}
```

### Deque 덱

* 양 끝에서만 자료를 넣고 양 끝에서 뺄 수 있는 자료구조
* Double-ended queue의 약자이다.
* push_front : 덱의 앞에 자료를 넣는 연산
* push_back : 덱의 뒤에 자료를 넣는 연산
* pop_front : 덱의 앞에서 자료를 빼는 연산
* pop_back : 덱의 뒤에서 자료를 빼는 연산
* front : 덱의 가장 앞에 있는 자료를 보는 연산
* back : 덱의 가장 뒤에 있는 자료를 보는 연산

[덱 문제](https://www.acmicpc.net/problem/10866)

문제 : 정수를 저장하는 덱(Deque)를 구현한 다음, 입력으로 주어지는 명령을 처리하는 프로그램을 작성하시오.

명령은 총 여덟 가지이다.

* push_front X: 정수 X를 덱의 앞에 넣는다.
* push_back X: 정수 X를 덱의 뒤에 넣는다.
* pop_front: 덱의 가장 앞에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
* pop_back: 덱의 가장 뒤에 있는 수를 빼고, 그 수를 출력한다. 만약, 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
* size: 덱에 들어있는 정수의 개수를 출력한다.
* empty: 덱이 비어있으면 1을, 아니면 0을 출력한다.
* front: 덱의 가장 앞에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.
* back: 덱의 가장 뒤에 있는 정수를 출력한다. 만약 덱에 들어있는 정수가 없는 경우에는 -1을 출력한다.

입력 : 첫째 줄에 주어지는 명령의 수 N (1 ≤ N ≤ 10,000)이 주어진다. 둘쨰 줄부터 N개의 줄에는 명령이 하나씩 주어진다. 주어지는 정수는 1보다 크거나 같고, 100,000보다 작거나 같다. 문제에 나와있지 않은 명령이 주어지는 경우는 없다.

출력 : 출력해야하는 명령이 주어질 때마다, 한 줄에 하나씩 출력한다.

```java
import java.util.*;

public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        sc.nextLine();
        ArrayDeque<Integer> queue = new ArrayDeque<Integer>();
        for (int k=0; k<n; k++) {
            String line = sc.nextLine();
            String[] s = line.split(" ");
            String cmd = s[0];
            if (cmd.equals("push_front")) {
                int num = Integer.parseInt(s[1]);
                queue.offerFirst(num);
            } else if (cmd.equals("push_back")) {
                int num = Integer.parseInt(s[1]);
                queue.offerLast(num);
            }  else if (cmd.equals("front")) {
                if (queue.isEmpty()) {
                    System.out.println("-1");
                } else {
                    System.out.println(queue.peekFirst());
                }
            } else if (cmd.equals("size")) {
                System.out.println(queue.size());
            } else if (cmd.equals("empty")) {
                if (queue.isEmpty()) {
                    System.out.println("1");
                } else {
                    System.out.println("0");
                }
            } else if (cmd.equals("pop_front")) {
                if (queue.isEmpty()) {
                    System.out.println("-1");
                } else {
                    System.out.println(queue.pollFirst());
                }
            } else if (cmd.equals("pop_back")) {
                if (queue.isEmpty()) {
                    System.out.println("-1");
                } else {
                    System.out.println(queue.pollLast());
                }
            } else if (cmd.equals("back")) {
                if (queue.isEmpty()) {
                    System.out.println("-1");
                } else {
                    System.out.println(queue.peekLast());
                }
            }
        }
    }
}
```

## 다이나믹 프로그래밍

다이나믹 프로그래밍 : 큰 문제를 작은 문제로 나눠서 푸는 알고리즘. 두 가지 속성을 만족해야 다이나믹 프로그래밍으로 문제를 풀 수 있다.

1. Overlapping Subproblem 겹치는 부분 문제
2. Optimal Substructure 문제의 정답을 작은 문제의 정답에서 구할 수 있음

1\. Overlapping Subproblem

* 큰 문제와 작은 문제를 같은 방법으로 풀 수 있다.
* 문제를 작은 문제로 쪼갤 수 있다.

피보나치 수

문제 : N번째 피보나치 수를 구하는 문제
작은 문제 : N-1번째 피보나치 수를 구하는문제, N-2번째 피보나치 수를 구하는 문제

문제 : N-1번째 피보나치 수를 구하는 문제
작은 문제 : N-2번째 피보나치 수를 구하는문제, N-3번째 피보나치 수를 구하는 문제

...

2\. Optimal Substructure

* 문제의 정답을 작은 문제의 정답에서 구할 수 있다.

예시 

* 서울에서 부산을 가는 가장 빠른 길이 대전과 대구를 순서대로 거쳐야 한다면
* 대전에서 부산을 가는 가장 빠른 길은 대구를 거쳐야 한다.

피보나치 수는 문제의 정답을 작은 문제의 정답을 합하는 것으로 구할 수 있다.

* 다이나믹 프로그래밍에서 각 문제는 한 번만 풀어야 한다.
* Optimal Substructure를 만족하기 때문에, 같은 문제는 구할 때마다 정답이 같다.
* 따라서, 정답을 한 번 구했으면, 정답을 어딘가에 메모해놓는다.
* 이런 메모하는 것을 코드의 구현에서는 배열에 저장하는 것으로 할 수 있다.
* 메모를 한다고 해서 영어로 Memoization이라고 한다.

### 피보나치 수

```java
static int fibonacci(int number) {
    if(number <= 1) {
       return number;
    } else {
       return fibonacci(number - 1) + fibonacci(number - 2);
    }
}
```

중복을 제거하고 한 번 답을 구할 때, 어딘가에 메모를 해놓고, 중복 호출이면 메모해놓은 값을 리턴한다.

```java
static int fibonacci(int number) {
    int [] memo = new int [100];
    if(number <= 1) {
        return number;
    } else {
        if(memo[number] > 0) {
            return memo[number];
        }
        memo[number] = fibonacci(number - 1) + fibonacci(number - 2);
        return memo[number];
    }
}
```

### 다이나믹 프로그래밍을 푸는 두 가지 방법

1. Top-down
2. Bottom-up

Top-down ; 보통 재귀함수를 이용해서 푼다.

문제를 작은 문제로 나누고, 작은 문제를 푸고, 문제를 푼다.

1\. 문제를 풀어야 한다.

> fibonacci(n)

2\. 문제를 작은 문제로 나눈다.

> fibonacci(n-1)과 fibonacci(n-2)로 문제를 나눈다.

3\. 작은 문제를 푼다.

> fibonacci(n-1)과 fibonacci(n-2)를 호출해 문제를 푼다.

4\. 작은 문제를 풀었으니, 이제 문제를 푼다.

> fibonacci(n-1)의 값과 fibonacci(n-2)의 값을 더해 문제를 푼다.


Bottom-up

문제의 크기가 작은 문제부터 차례대로 푼다. 문제의 크기를 조금씩 크게 만들면서 문제를 점점 푼다. 작은 문제를 풀면서 왔기 때문에, 큰 문제는 항상 풀 수 있다. 그러다보면, 언젠간 풀어야 하는 문제를 풀 수 있다.

보통 for문을 이용해서 구현한다.

```java
static int fibonacci(int number) {
    int [] d = new int [100];
    d[0] = 0;
    d[1] = 1;
    for(int i = 2; i <= number; i++) {
        d[i] = d[i-1] + d[i-2];
    }
    return d[number];  
}
```


### 문제풀이1

문제. [1로 만들기](https://www.acmicpc.net/problem/1463)

정수 X에 사용할 수 있는 연산은 다음과 같이 세 가지 이다.

1. X가 3으로 나누어 떨어지면, 3으로 나눈다.
2. X가 2로 나누어 떨어지면, 2로 나눈다.
3. 1을 뺀다.

정수 N이 주어졌을 때, 위와 같은 연산 세 개를 적절히 사용해서 1을 만드려고 한다. 연산을 사용하는 횟수의 최소값을 출력하시오.

입력 : 첫째 줄에 1보다 크거나 같고, 106보다 작거나 같은 자연수 N이 주어진다.

풀이 :

D[n] = n -> 1 만드는데 필요한 연산의 최소값

D[n/3] + 1 = n/3 -> 1 만드는데 필요한 연산의 최소값

D[n/2] + 1 = n/2 -> 1 만드는데 필요한 연산의 최소값

1. X가 3으로 나누어 떨어지면, 3으로 나눈다. D[n/3] + 1
2. X가 2로 나누어 떨어지면, 2로 나눈다. D[n/2] + 1
3. 1을 뺀다. D[n-1] + 1

D[n] = min(1, 2, 3)

```java
    static int go(int n) {
        int [] d = new int [100];
        if(n == 1) return 0;
        if (d[n] > 0) return d[n];
        d[n] = go(n-1) + 1;
        if(n%2 == 0) {
            int temp = go(n/2) + 1;
            if (d[n] > temp) d[n] = temp;
        }
        if(n%3 == 0) {
            int temp = go(n/3) + 1;
            if (d[n] > temp) d[n] = temp;
        }
        return d[n];
    }
```

```java
    static int go(int n) {
        int [] d = new int [100];
        d[1] = 0;
        for(int i=2; i<=n; i++) {
            d[i] = d[i-1] + 1;
            if(i%2 == 0 && d[i] > d[i/2] + 1) {
                d[i] = d[i/2] + 1;
            }
            if(i%3 == 0 && d[i] > d[i/3] + 1) {
                d[i] = d[i/3] + 1;
            }
        }
        return d[n];
    }
```

### 문제풀이2

[2xn타일링](https://www.acmicpc.net/problem/11726)

문제 : 2×n 크기의 직사각형을 1×2, 2×1 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오.

입력 : 첫째 줄에 n이 주어진다. (1 ≤ n ≤ 1,000)

출력 : 첫째 줄에 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력한다.

직사각형 : 2 X n

D[n] = 2 X n 채우는 방법의 수

1. D[n - 1]
2. D[n - 2]

D[n] = D[n - 1] + D[n - 2]

```java
import java.util.*;
public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] d = new int[1001];
        d[0] = 1;
        d[1] = 1;
        for (int i=2; i<=n; i++) {
            d[i] = d[i-1] + d[i-2];
            d[i] %= 10007;
        }
        System.out.println(d[n]);
    }
}
```


### 문제풀이 3

[2xn타일링 2](https://www.acmicpc.net/problem/11727)

문제 : 2×n 직사각형을 1x2, 2×1과 2×2 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오.

입력 : 첫째 줄에 n이 주어진다. (1 ≤ n ≤ 1,000)

출력 : 첫째 줄에 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력한다.

D[n] = 2 X n 채우는 방법의 수

1. D[n - 1]
2. D[n - 2]
3. D[n - 2]

D[n] = D[n - 1] + 2 x D[n - 2]

```java
import java.util.*;
public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] d = new int[1001];
        d[0] = 1;
        d[1] = 1;
        for (int i=2; i<=n; i++) {
            d[i] = d[i-1] + 2*d[i-2];
            d[i] %= 10007;
        }
        System.out.println(d[n]);
    }
}
```


### 문제풀이 4

[1,2,3더하기](https://www.acmicpc.net/problem/9095)

문제 : 정수 4를 1, 2, 3의 조합으로 나타내는 방법은 총 7가지가 있다.

* 1+1+1+1
* 1+1+2
* 1+2+1
* 2+1+1
* 2+2
* 1+3
* 3+1

정수 n이 주어졌을 때, n을 1,2,3의 합으로 나타내는 방법의 수를 구하는 프로그램을 작성하시오.

입력 : 첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있고, 정수 n이 주어진다. n은 양수이며 11보다 작다.

출력 : 각 테스트 케이스마다, n을 1,2,3의 합으로 나타내는 방법의 수를 출력한다.

풀이

D[n] = n을 1,2,3의 조합으로 나타내는 방법의 수

마지막에 올 수 있는 수가 무엇인지 살펴보면 된다. 1, 2, 3이 올 수 있다.

1이 온다면 : 나머지 앞 부분의 합은 n - 1 --> D[n-1]

2가 온다면 : 나머지 앞 부분의 합은 n - 2 --> D[n-2]

3이 온다면 : 나머지 앞 부분의 합은 n - 3 --> D[n-3]

D[n] = D[n-1] + D[n-2] + D[n-3] 복잡도 O(n)

```java
import java.util.*;
import java.math.*;

public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int[] d = new int[11];
        d[0] = 1;
        for (int i=1; i<=10; i++) {
            for (int j=1; j<=3; j++) {
                if (i-j >= 0) {
                    d[i] += d[i-j];
                }
            }
        }
        int t = sc.nextInt();
        while (t-- > 0) {
            int n = sc.nextInt();
            System.out.println(d[n]);
        }
    }
}
```

### 문제풀이 5

[붕어빵 판매하기](https://www.acmicpc.net/problem/11052)

문제 : 강남역에서 붕어빵 장사를 하고 있는 해빈이는 지금 붕어빵이 N개 남았다.
해빈이는 적절히 붕어빵 세트 메뉴를 구성해서 붕어빵을 팔아서 얻을 수 있는 수익을 최대로 만드려고 한다. 붕어빵 세트 메뉴는 붕어빵을 묶어서 파는 것을 의미하고, 세트 메뉴의 가격은 이미 정해져 있다. 붕어빵 i개로 이루어진 세트 메뉴의 가격은 Pi 원이다. 붕어빵이 4개 남아 있고, 1개 팔 때의 가격이 1, 2개는 5, 3개는 6, 4개는 7인 경우에 해빈이가 얻을 수 있는 최대 수익은 10원이다. 2개, 2개로 붕어빵을 팔면 되기 때문이다. 1개 팔 때의 가격이 5, 2개는 2, 3개는 8, 4개는 10 인 경우에는 20이 된다. 1개, 1개, 1개, 1개로 붕어빵을 팔면 되기 때문이다. 마지막으로, 1개 팔 때의 가격이 3, 2개는 5, 3개는 15, 4개는 16인 경우에는 정답은 18이다. 붕어빵을 3개, 1개로 팔면 되기 때문이다. 세트 메뉴의 가격이 주어졌을 때, 해빈이가 얻을 수 있는 최대 수익을 구하는 프로그램을 작성하시오.

입력 : 첫째 줄에 해빈이가 가지고 있는 붕어빵의 개수 N이 주어진다. (1 ≤ N ≤ 1,000) 둘째 줄에는 Pi가 P1부터 PN까지 순서대로 주어진다. (1 ≤ Pi ≤ 10,000)

출력 : 해빈이가 얻을 수 있는 최대 수익을 출력한다.

풀이 

붕어빵 N개를 가지고 있다.

붕어빵 i개를 팔아서 얻을 수 있는 수익이 P[i]일 때, N개를 모두 판매해서 얻을 수 있는 최대 수익 구하기

D[n] = n개 팔아서 얻을 수 있는 최대 수익

O + O + O + O' = n

O'에게 몇 개를 팔 것인지 정해줘야 한다.

1. O'에게 1개를 팔면 앞에 사람들에겐 n-1개를 팔아야 한다. 최대수익은 D[n-1] + P[1]
2. O'에게 2개를 팔면 앞에 사람들에겐 n-2개를 팔아야 한다. 최대수익은 D[n-2] + P[2]
3. O'에게 3개를 팔면 앞에 사람들에겐 n-3개를 팔아야 한다. 최대수익은 D[n-3] + P[3]
4. O'에게 4개를 팔면 앞에 사람들에겐 n-4개를 팔아야 한다. 최대수익은 D[n-4] + P[4]
5. O'에게 n개를 팔면 앞에 사람들에겐 0개를 팔아야 한다. 최대수익은 D[0] + P[n]

일반화 : 최대수익 = MAX(D[n-i] + P[i]) (1<=i<=n)

```java
import java.util.*;

public class Main {
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] a = new int[n+1];
        for (int i=1; i<=n; i++) {
            a[i] = sc.nextInt();
        }
        int[] d = new int[n+1];
        for (int i=1; i<=n; i++) {
            for (int j=1; j<=i; j++) {
                if (d[i] < d[i-j] + a[j]) {
                    d[i] = d[i-j] + a[j];
                }
            }
        }
        System.out.println(d[n]);
    }
}
```


























