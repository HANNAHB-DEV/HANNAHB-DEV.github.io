### 18-2 동적계획법2 - Basic Example

#### 행렬 경로 문제

- 정수들이 저장된 nxn 행렬의 좌상단에서 우하단까지 이동, 단 오른쪽이나 아래쪽 방향으로만 이동 가능
- 방문한 칸에 있는 정수들의 합이 최소화 되도록 하라

![](/assets/Algorithm/short/dp2-1.PNG)

동적계획법은 순환식을 푸는 것과 관련이 있으므로, 문제가 주어졌을 때 필요한 순환식을 세우고 순환식 계산

#### Key Observation

![](/assets/Algorithm/short/dp2-2.PNG)

(1,1)에서 (i,j)를 오려면 위에서 오거나, 옆에서 와야하므로 (i,j-1) or (i-1,j)에서 와야함

#### 순환식

- L|i,j| : (1,1)에서 (i,j)까지 오는 최소합

![](/assets/Algorithm/short/dp2-3.PNG)

i, j-1까지 최소한으로 오거나 (L|i-1,j|) + mij
i-1,j까지 최소한으로 오거나 (L|i,j-1|) + mij (i,j에 있는 값)
그 둘 중에 최소한을 뽑아야 하므로 min함수 사용

#### Recursive Algorithm

```java
int mat(int i, int j){ //L|i,j|
  if (i==1 && j==1)
    return m[i][j];
  else if (i==1)
    return mat(1, j-1) + m[i][j];
  else if (j==1)
    return mat(i-1, j) + m[i][j];
  else return Math.min(mat(i-1,j), mat(i,j-1)) + m[i][j];
}
```

#### Memoization

```java
int mat(int i, int j){ //L|i,j|
  if (L[i][j] != -1) return L[i][j]; //-1로 초기화, 이미 L[i][j]가 캐싱되어 있음
  if (i==1 && j==1)
    L[i][j] = m[i][j]; //저장하는 것임
  else if (i==1)
    L[i][j] = mat(1, j-1) + m[i][j];
  else if (j==1)
    L[i][j] = mat(i-1, j) + m[i][j];
  else 
    L[i][j] = Math.min(mat(i-1,j), mat(i,j-1)) + m[i][j];
  return L[i][j];
}
```

#### Bottom up

![](/assets/Algorithm/short/dp2-4.PNG)

L(i-1,j), L(-,j-1)은 L(i,j)보다 선행되어 계산되어 있음

```java
int mat(){
  for (int i=1; i<=n; i++){
    for(int j=1; j<=n; j++){
      if (i==1 && j==1)
      	L[i][j] = m[1][1];
 	 else if (i==1)
 	 	L[i][j] = m[i][j] + L[i][j-1]; //항상 먼저 선행되어 계산된다
 	 else if (j==1)
 	 	L[i][j] = m[i][j] + L[i-1][j];
 	 else
 	 	L[i][j] = m[i][j] + Math.min(L[i-1][j], L[i][j-1]);
    }
  }
  return L[n][n];
}

시간복잡도: O²
```

#### Common Trick

```java
/*Initialize L with L[0][j]=L[i][0]=무한대 for all i and j*/

int mat(){
  for (int i=1; i<=n; i++){
    for(int j=1; j<=n; j++){
      if (i==1 && j==1)
      	L[i][j] = m[1][1];
 	 else
 	 	L[i][j] = m[i][j] + Math.min(L[i-1][j], L[i][j-1]);
    }
  }
  return L[n][n];
}
```

#### 경로구하기

![](/assets/Algorithm/short/dp2-5.PNG)

28은 25로부터 오거나, 31로부터 왔는데 25로 부터 왔음. 

```java
void printPath(){
  int i=n, j=n;
  while (p[i][j] != '-'){
    print (i + " " + j);
    if (p[i][j] == '<-')
    	j = j-1;
    else
    	i = i-1;
  }
  print (i + " " + j);
}
```

```java
void printPathRecursive(i,j){
  while (p[i][j] -= '-')
  	print(i + " " + j);
  else{
    if (P[i][j] == '<-')
    	printPathRecursive(i, j-1);
    else
    	printPathRecursive(i-1,j);
    print(i + " " + j);
  }
}
```

