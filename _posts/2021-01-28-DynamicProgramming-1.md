---
layout: post
title:  "[알고리즘] 동적계획법 1"
date:   "2021-01-28 17:35:52"
author: Hannah-B
categories: Algorithm
tags: 동적계획법
---

### Dynamic Programming

#### Fibonacci Numbers (피보나치 수열)

1 1 2 3 5 8 13 ...
f(n)= f(n-1) + f(n-2) n>2 , f(1) = 1

```java
int fin(int n){
  if (n==1 || n==2)
  	return 1;
  else
  	return fib(n-2) + fib(n-1);
}
```

![](/assets/Algorithm/short/dp1-1.PNG)

피보나치 수열을 recursion으로 하는 것은 많은 계산이 반복되기 떄문에 비효율적

#### Memoization

```java
int fib(int n){
  if(n==1 || n==2)
    return 1;
  else if (f(n) > -1) /*배열 f가 -1로 초기화되어 있다고 가정*/
    return f(n);	/*즉 이미 계산된 값이라는 의미*/
  else{
    f(n) = fib(n-2) + fib(n-1); /*중간 계산 결과를 caching*/
    return f(n)
  }
}
```

![](/assets/Algorithm/short/dp1-2.PNG)

배열에 중간 계산을 저장하는 것 = caching

#### Dynamic Programming

```java
inf fib(int n){
  f(1) = f(2) = 1;
  for(int i=3;i<=n;i++)
    f(n) = f(n-1) + f(n-2);
  return f(n);
}
```

![](/assets/Algorithm/short/dp1-3.PNG)

순서대로 쭉 계산해서 올라와 bottom-up방식으로 중복계산을 피한다.
역으로 가지 않으면 순차적으로 계산해 올라와 있기에 그 전의 값들을 다 알고 있다.



#### 이항계수

n개 중에서 k를 선택한다

![](/assets/Algorithm/short/dp1-4.PNG)

n개 중에서 n개를 뽑는 경우의 수는 1, k를 뽑지 않는 경우도 1

```java
int binomial(int n, int k){
  if(n==k||k==0)
 	 return 1;
  else
	 return binomial(n-1,k) + binomial(n-1,k-1);
}
```

역시 많은 계산이 중복된다.

#### Memoization

```java
int binomial(int n, int k){
  if(n==k||k==0)
    return 1;
  else if(binom[n][k] >-1 ) /*배열 binom이 -1로 초기화 되어있다고 가정*/
    return binom[n][k];
  else {
    binom[n][k] = binomial(n-1,k) + binomial(n-1,k-1);
    return binom[n][k];
  }
}
```

![](/assets/Algorithm/short/dp1-5.PNG)

n,k 2개를 저장하기 때문에 2차원 배열을 만들어야 한다. n>=k 이기 때문에 대각선 아래의 값들만 유효하다.

#### Dynamic Programming

```java
int binomial(int n, int k){
  for(int i=0; i<=n; i++){
    for (int j=0; j<=k && j<=i; j++){
	    if (k==0||n==k)
    	  binom[i][j] = 1;
    	else
      binom[i][j] = binom[i-1][j-1] + binom[i-1][j];
    }
  }
  return binom[n][k];
}
```

![](/assets/Algorithm/short/dp1-6.PNG)

순환식을 이용해 오른쪽에 등장하는 값들이 왼쪽에 등장하는 값들이 계산되어 있다면, recursion이 필요 없음.

#### Memoization vs Dynamic Programming

- 순환식의 값을 계산하는 기법들이다.
- 둘 다 동적계획법의 일종으로 보기도 한다.
- Memoization은 top-down 방식이며, 실제로 필요한 subproblem만을 푼다.
- 동적계획법은 bottom-up방식이며, recursion에 수반되는 overhead가 없다.
