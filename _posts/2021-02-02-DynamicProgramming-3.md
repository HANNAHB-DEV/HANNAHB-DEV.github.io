---
layout: post
title:  "[알고리즘] 동적계획법 3"
date:   "2021-02-02 20:07:52"
author: Hannah-B
categories: Algorithm
tags: 동적계획법
---

### 최단경로 동적계획법

#### Floyd-Warshall Algorithm

one-to-all 출발점이 하나고, 다른 모든 노드까지 가는 알고리즘인데
이번 알고리즘은 all to all

- 가중치 방향 그래프 G=(V,E), V={1,2,....,n}
- 모든 노드 쌍들간의 최단경로의 길이를 구함
- d의k승[i,j]
  - 중간에 노드집합 {1,2,....,k}에 속한 노드들만 거쳐서 노드 i에서 j까지 가는 최단경로의 길이

![](/assets/Algorithm/short/dp3-1.PNG)

(1) k=0이라는 것은 1~0 사이라는 것이니 공집합임. i에서 j까지 가는데 중간에 거칠 게 없음, 즉 i에서 j로 가는 직빵=weight
(2) i->j 인데 1~k 사이의 노드들만 지날 수 있을 때, 노드 k를 지나는 경우와, 지나지 않는 경우가 존재함.
(3) 1에서 n까지의 노드를 지날 수 있는데, 아무 조건이 없으므로 최단으로 가는 것 = 목적

![](/assets/Algorithm/short/dp3-2.PNG)

k를 지나지 않는 경우라면 dk-1[i,j]

i->k로 가는 것은 1~k-1까지 가는 경로 중 가장 minimal 한 것을 골라야 함. dk-1[i,k]
k->j 1~k-1 속하는 것만 갈 수 있고 dk-1[k,j] 그래서 위에 저런 식이 나왔다.

```java
FloydWarshall(G){
  for i<- 1 to n
  	for j <- 1 to n
      d0[i,j] <- wij;
  for k <- 1 to n
    for i <- 1 to n
      for j <- 1 to n
        dk[i,j] <- min{dk-1[i,j], dk-1[i,k]+dk-1[k,j]};
}

시간복잡도: O(n³)
```

```java
FloydWarshall(G){
  for i<- 1 to n
  	for j <- 1 to n
      d0[i,j] <- wij;
  for k <- 1 to n
    for i <- 1 to n
      for j <- 1 to n
        d[i,j] <- min{d[i,j], d[i,k]+d[k,j]};
} // 아무 문제가 없음
```

#### 경로 찾기

```java
FloydWarshall(G){
  for i <- 1 to n
  	for j <- 1 to n
  		d[i,j] <- wij;
  		π[i,j] <- NIL;
  for k <- 1 to n
  	for i <- 1 to n
  		if d[i,j] > d[i,k]+d[k,j] then
  			d[i,j] = d[i,k]+d[k,j];
  			π[i,j] = k; //i->j까지 가는 저점을 배열에 표시한 것
}
```

```java
print-PATH(s, t, π){
/*s에서 t까지 가는 경로가 존재한다는 가정 하에 최단경로상의 중간노드들(s,t 제외)을 순서대로 출력함*/
  if [s,t]=NIL then return; //어떤 노드도 지나지 않음
  print-PATH(s, π[s,t]);
  print(π[s,t]);
  print-PATH(π[s,t], t);
}
```

