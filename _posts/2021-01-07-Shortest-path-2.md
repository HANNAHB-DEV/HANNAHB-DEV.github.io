**최단경로(Shortest path problem)**

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

![image-20210107195413156](C:\Users\Administer\AppData\Roaming\Typora\typora-user-images\image-20210107195413156.png)

어떤 순서로 하느냐에 따라 순서가 달라짐. 워스트 케이스에 가까운 예.

(a) d(s)=0, 어떤 에지들을 먼저 릴렉스에 하느냐에 따라 최단경로가 달라짐
0+6<무한대라, d(t)=6, d(y)=7

(d)처럼 만약 나중에 릴렉싱 되었다면 값이 바뀔수도 있음

(c)에 도달한 이후에는 어떠한 에지를 릴렉싱하더라도 더 나은 답 없음

**Worst Scenario**

![image-20210107195757459](C:\Users\Administer\AppData\Roaming\Typora\typora-user-images\image-20210107195757459.png)

d(v)=1000이고 뒤쪽이 각각 100이라고 가정할 떄, 1100, 1200, 1300 등으로 측정됨
d(v)= 800일때는 뒤는 900,1000,1100 등
d(v)= 600일떈 700,800,900...
d(v)가 바뀔 떄 마다 값이 계속 바뀐다.
나중에 d(v)=250일떄 릴렉싱을 한다면 불필요하게 뒤의 과정을 여러번 반복할 필요가 없음.

**Dijkstra의 알고리즘**

![image-20210107200125394](C:\Users\Administer\AppData\Roaming\Typora\typora-user-images\image-20210107200125394.png)

최소의 노드로 나가는 에지. 앞의 10, 5 뒤에는 한번 더 거쳐야 하므로 최소가 아님
d(s)=δ(s,s) 일때 릴렉싱하였을 떄 출발점에 대해서는 릴렉싱할 필요가 없다(출발점?)
d(v)=5가 최단 경로의 길이. 방문한 노드는 칠함. 5+2=7
10은 8로 제해지고, 저긴 무한대였으니까 9(우측 상단), 우측 하단은 5+2=7
5,7을 제하고 8로 가서 나머지 노드에 대해 릴렉스. 14->13이 됨

- 음수 가중치가 없다고 가정
- S로부터의 최단경로의 길이를 이미 알아낸 노드들의 집합 s를 유지, 맨 처음에는 S=Φ
- Loop invariant: u∈XS인 각 노드 u에 대해서 d(u)는 이미 s에 속한 노드들만 거쳐서 s로부터 u까지 가는 최단경로의 길이

정리: d(u)=min v∈XS 인 d(v)인 노드 u에 대해서 d(u)는 s->u까지의 최단 경로의 길이
증명: proof by contradiction

아니라고 한다면 s->u까지 다른 최단경로가 존재

![](C:\Users\Administer\Desktop\short2-6.PNG)

d(v)>=d(u) 이므로 모순.
처음으로 s에 속하지 않는 다른 경로로 간다면.. 녹색>=d(v)+v->w

- d(u)가 최소인 노드 u∈XS를 찾고, s에 u를 추가
- s가 변경되었으므로 다른 노드들의 d(v)값을 갱신

![](C:\Users\Administer\Desktop\short2-8.PNG)

d(v)=min{d(v),d(u)+w(u,v)} 
**즉, 에지 (u,v)에 대해서 relaxation하면 loop invariant가 계속 유지됨**



(a) d(s)=0, d=δ 출발점에서 목적지까지 가장 최소인 거리, 0을 제외시킴. 나가는 에지들에 대해서 릴렉스 해줌.
(b) 0은 고려할 필요가 없음(갱신되지 않음), 5가 최단길이, 나가는 노드들에 대해 relax. 2가 더 적으니까
(c) 7로 옴. S={0,5}가 됨. 7에서 나가는 에지들은 6뿐이니 7+6=13, 14가 13이 됨.

![](C:\Users\Administer\Desktop\short2-9.PNG)

```
gijkstra(G,w,s)
	for each u∈V do
		δ[u] <- 무한대
		π[u] <- NIL
	end.
	S<-Φ
	d[s] <- 0
	whild |S|<n do //while문은 n번 반복
		find u∈xs with the minimum d[u] value; //최소값 찾기(δ(s,u))
		S <- S∪{u}
		for each v∈xs adjacent to u do //degree(u) = O(n)
			if d[v] > d[u]+w(u,v) then
				d[v] <- d[u]+w(u,v)
				π[v] <- u
			end. //모든 노드들이 s에 포함되면 종료
		end.
	end.
	
	시간복잡도: O(n²)
```

```
Dijkstra의 알고리즘

DIJKSTRA(G,w,s)
INITIALIZE-SINGLE-SOURCE(G,s)
S<-Φ
Q<-V[G]
while Q≠Φ
	do u <- EXTRACT-MIN(Q) //o(logN)
		S<- S∪{u}
		for each vertex v∈Adj[u] //degree(u)의 합(v에 속하는)
			do RELAX(u,v,w) //heap복원, d(v) 감소
```

**시간복잡도**

- Prim의 알고리즘과 동일함
- 우선순위 큐를 사용하지 않고 단순하게 구현할 경우 O(n²)
- 이진힙을 우선순위 큐로 사용할 경우 O(nlog2n + mlog2n)
- 피보나치 heap을 사용하면 O(nlong2n+m)에 구현 가능