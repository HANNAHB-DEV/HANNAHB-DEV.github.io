---
layout: post
title:  "[알고리즘] 최단경로 알고리즘 1"
date:   "2021-01-05 18:25:52"
author: Hannah-B
categories: Algorithm
tags: 최단경로, 최단경로 알고리즘
---

## **최단경로 Shortest path**

- 가중치(방향) 그래프 G=(V,E), 즉 모든 에지에 가중치가 있음
- 경로 p=(v0,v1,...vk)의 길이는 경로상의 모든 에지의 가중치의 합
- 노드 u에서 v까지의 최단경로의 길이를 δ(u,v)라고 표시한다.

if) 에지의 개수가 경로의 길이, 모든 weight가 동일한다면 개수가 가장 작은게 최단 경로.

**최단 경로 문제의 유형**

- single-source 
  - 하나의 출발노드 s로부터 다른 모드 노드까지의 최단 경로
  - ex) Dijkstra의 알고리즘
- single-destinations
  - 모든 노드로부터 하나의 목적지 노드까지의 최단 경로
  - ex) single-source 문제와 동일
  - 방향이 없는 그래프라면 위의 그래프와 동일함
- single-pair
  - 주어진 하나의 출발노드 s로부터 하나의 목적지 노드 t까지의 최단 경로
  - 최악의 경우 시간 복잡도에서 single-source 문제보다 나은 알고리즘 X
- All-pairs
  - 모든 노드 쌍에 대해서 최단 경로 찾기
  - ex) Floyd 알고리즘, all-to-all

**최단경로와 음수 가중치**

![](https://hannahb-dev.github.io/assets/Algorithm/short/short.PNG)

가중치에 음수가 존재하는 경우도 있음, 보통 최단경로는 음수 가중치가 없다고 가정하는 경우가 많음. 위의 이미지에서 처럼 음수싸이클이 있으면 최단경로가 정의되지 않는다.

**최단경로의 기본 특성**

- 최단경로의 어떤 부분경로도 역시 최단경로이다.

  ![short2](https://hannahb-dev.github.io/assets/Algorithm/short/short2.PNG)

- 최단 경로는 사이클을 포함하지 않는다.(음수 사이클이 없다는 가정 하에서)

**Single-source 최단 경로 문제**

- 입력 : 음수 사이클이 없는 가중치 방향그래프 G=(V,E)와 출발노드 s∈V
- 목적 : 각 노드 v∈V에 대해서 다음을 계산한다
  - d[v] (distance estimate) 거리에 대한 측정식, 추정식
    - 처음에는 d[s]=0, d[v]=무한대로 시작한다.
    - 알고리즘이 진행됨에 따라서 감소해간다. 하지만 항상 d[v]>=δ(s,v)를 유지한다.
    - 최종적으로는 d[v]=δ(s,v)
    - s에서 v로 가는 경로 중 최소의 길이. 자기 자신에서 출발하니까 0, 나머지에 대해서는 아는게 없으니 무한대로 가정.
  - π[v]: s에서 v까지 최단 경로상에서 v의 직전 노드(predecessor)
    - 그런 노드가 없는 경우 π[v]=NIL

**기본 연산: Relaxation**

그래프의 에지에 대해서 relax한다. 

![short3](https://hannahb-dev.github.io/assets/Algorithm/short/short3.PNG)

1.  5->9에서 가는.. d[u]=5, d[v]=9

   출발점s로 부터 u까지 길이가 5인 경로를 알고있다는 것. s->u 길이가 5인 경로 有

   s->v까지는 9의 길이. relax한다는 것은, 기존에 알고있던 경로보다 더 나은 경로를 발견한다는 것.

   s->v로 갈때, s->u->v가 더 나은 연산일 수 있다.

2. d[u]=5, d[v]=6 s에서 v까지 길이 6인걸 알고있음

   s->u->v가 7이니까 더 길어서 relax하지 않는다.

**single-source의 최단 경로**

- 대부분의 single-source 최단 경로 알고리즘의 기본 구조
  1. 초기화:  d[s]=0, 노드 v≠s에 대해서 d[v]=무한대, π[v]=NIL
  2. 에지들에 대한 반복적인 relaxation
- 알고리즘들 간의 차이는 어떤 에지에 대해서, 어떤 순서로 relaxation을 하느냐에 있음. relax 연산을 어느 에지에서, 어떤 순서로 하느냐에 따라 다름.

```
Generic-Single-Sources(G,w,s)
	INITIALISE-SINGLE-SOURCES(G,s) //초기화 d[s]=0, d[u]=무한대 u≠s, π[v]=NIL
	repeat 
		for each edge(u,v)∈E
			RELAX(u,v,w)
	until there is no change. //모든 에지들에 대해서 relax하는데, 변화가 없을 때 까지.
```

Q1. 이렇게 반복하면 최단 경로가 찾아지는가?

Q2. 몇 번 반복해야 하는가? (repeat~until)

어떤 노드의 d값이 update가 될 수도 있고, 안될 수도 있는데 하나라도 된다면 다시 repeat함.

![](/assets/Algorithm/short/short4.PNG)

if) 임의의 노드 v에 대해서 s->v까지 간다고 가정할 때, 최단 경로에 포함된 에지의 개수는 n-1개.

모든 에지들에 대해서 relax하는데, repeat-until이 한번 도는 걸 한 라운드라고 가정할 때, d(s)=0, d(v1)=무한대였는데, 최단 경로의 일부는 최단경로 이므로 d(v1) = d(s)+u(s,v1) = δ(s,v1)

d(v2) = d[v1] + w(v1,v2) = δ(s,v2)... 이렇게

d(vi) =  δ(s,vi)

**Bellman-Ford 알고리즘**

```
BELLMAN-FORD(G,w,s)
	INITIALIZE-SINGLE-SOURCE(G,s)
	for i <- 1 to |V[G]| -1 //n-1번
		do for each edge(u,v) ∈ E[G]
			do RELAX(u,v,w) //각각의 라운드(모든 에지)에 대해 relax한다
	for each edge (u,v) ∈ E[G]
		do if d[v] > d[u] + w(u,v)
			then return FALSE //음수 사이클이 존재한다면 false.
	return FLASE
	
시간복잡도 O(nm)
```

