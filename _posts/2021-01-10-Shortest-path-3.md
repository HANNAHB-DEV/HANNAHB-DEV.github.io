## Floyed-Warshall 알고리즘 (all to all)

Dynamic Prog

- 가중치 방향 그래프 G=(V,E), V={1,2,....,n}

- 모든 노드 쌍들간의 최단경로의 길이를 구함

- dⁿ[i,j]

  - 중간에 노드 집합 {1,2,....,n}에 속한 노드들만 거쳐서 노드 i->j까지 가는 최단경로의 길이
    모든 노드들은 1~n까지의 정수, i->j로 까지 갈 떄 1,2...n를 지난다.

    ![](C:\Users\Administer\Desktop\short3-1.PNG)

d0이라면, k=0, {1...0}이므로 공집합, 즉 아무 노드도 지나지 않고 직접 i->j로 간다.
i.j가 존재한다면 에지의 weight는 wij이다. 에지가 없다면 경로가 존재하지 않으므로 무한대.

dⁿ[i,j]의 경우 {1,2,....n} 에서는 아무 조건 없이 가장 최단경로를 선택한다.

dk[i,j] i->j 사이에 1~k가 있는 경우, 노드 k를 지나는 경우가 있을 수 있고, 아닐 수도 있음.
k를 지나지 않는다면, 1~k-1의 노드들을 지난다. 그 경우 dk-1[i,j]![](C:\Users\Administer\Desktop\short3-2.PNG)

밑의 케이스가 dk-1[i,j], 위의 왼쪽 경우 dk-1[i,k] (k-1개가 속해있고 k까지의 경로이므로) 오른쪽은 dk-1[k,j]

즉, 두 가지 케이스에 대해서 최소값을 선택한다.

```
FloydWarshall(G){
	for i <- 1 to n
		for j <- 1 to n
			d0[i,j] <- wij;
	for k <- 1 to n //중간정점집합 {1,2,...,k}
		for i <- 1 to n
			for j <- 1 to n
				dk[i,j] <- min{dk-1[i,j], dk-1[i,k]+dk-1[k,j]};
}

시간복잡도: O(n³):3차원 배열 d[k][i][j]
```

Dijkstra의 경우 O(n²), one to all이였고 이건 all to all이니 n개의 케이스가 더 생기므로

```
2차원 배열
FloydWarshall(G){
	for i <- 1 to n
		for j <- 1 to n
			d0[i,j] <- wij;
	for k <- 1 to n //중간정점집합 {1,2,...,k}
		for i <- 1 to n
			for j <- 1 to n
				d[i,j] <- min{d[i,j], d[i,k]+d[k,j]};
}
```

왜 이 코드는 괜찮은가?
d[i,j]가 2차원 좌표의 한 점이라고 할 떄, k도 좌표상의 한 점.
k-1을 덮어써버려도 상관이 없음..

### 경로 찾기

```
FloydWarshall(G){
	for i <- 1 to n
		for j <- 1 to n
			d[i,j] <- wij;
			π[i,j] <- NIL; //초기화
	for k <- 1 to n //중간정점집합 {1,2,...,k}
		for i <- 1 to n
			for j <- 1 to n
				if d[i,j] > d[i,k]+d[k,j] then
					d[i,j] = d[i,k]+d[k,k=j]; //새 경로가 weight가 더 적다면, 바꿈
				π[i,j] <- k; //i에서 j로 가는 최단경로는 ->k->j임
}
```

```
Print-PATH(s,t,π){
	if π[s,t] = NIL then return; //s->t 다이렉트라면
	print-PATH[s,π(s,t)];
	print(π(s,t));
	print-PATH[π[s,t],t];
} 
```

s에서 t까지 가는 경로가 존재한다는 가정 하에 최단 경로상의 중간노드들(s,t 제외)를 출력함