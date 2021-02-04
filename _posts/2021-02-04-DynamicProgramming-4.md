---
layout: post
title:  "[알고리즘] 동적계획법 4"
date:   "2021-02-04 13:52:52"
author: Hannah-B
categories: Algorithm
tags: 동적계획법
---

### 동적계획법 4

Matrix-Chain Multiplication

#### 행렬의 곱셈 - pxq 행렬 A와 qxr 행렬 B곱하기

```java
void product(int A[][], int B[][], int C[][]){
	for(int i=0; i<p; i++){
		for (int j=0; j<r; j++){
			c[i][j]=0;
			for (int k=0; k<q; k++){
				C[i][j] += A[i][k] * b[k][j]; //aik X bkj임
			}
		}
	}
}
곱셈연산의 횟수 = pqr
```

pxq, qxr (여기서 q는 같아야 함)를 곱하면  pxr의 행렬이 나온다.



#### Matrix-Chain 곱하기 - AxBxCxD 곱하기

- 행렬 A는 10x100, B는 100x5, C는 5X50
- 세 행렬의 곱 ABC는 두 가지 방법으로 계산 가능(결합법칙 성립)
  - (AB)C : 7500번의 곱셈이 필요(10x100x5 + 10x5x50)
  - A(BC): 75000번의 곱셈이 필요(100x5x50 + 10x100x50) 
- 즉 곱하는 순서에 따라 연산량이 다름
- n개의 행렬 곱 A1A2A3...An을 계산하는 최적의 순서는?
- 여기서 Ai는 pk-1 x pk행렬이다.
- 앞쪽의 열의 개수와 뒤의 행의 개수가 같아야 한다. 

#### Optimal Substruction

![](/assets/Algorithm/short/dp4-1.JPG)

순환식을 구해야 하는데, 최적해를 구하고 일 부분에 대한 최적해가 맞는지 확인해야 함.
A1~An을 곱할 떄 최선의 과정이 있다고 가정하는데, n개의 행렬을 곱하는 것은 결과적으로 마지막 행렬 Z가 나오는 것이고, Z는 X, Y의 두개의 행렬을 곱한 것이므로, 여기서 X, Y에 대해서 생각해야 함.
X란 A1~Ak를 곱한 것이고, Y는 Ak+1~An의 곱

if 위의 계산식이 최적해라면(과정이 최소라면) k개를 곱하는 것도 계산량이 적어야 함.

#### 순환식

![](/assets/Algorithm/short/dp4-2.JPG)

i<=j 라는 가정 하에서, 최소 곱셈횟수를 m[i,j]라고 함.
Ai~Ak 곱하고(Pi-1Pk), Ak+1~Aj(PkPj)까지 곱해서 최선의 방법으로 곱한다.

Base Case가 필요함. 
i~j 사이의 k를 호출하는데, i~j의 간격이 좁아지므로 이 길이가 0이 될 때 basecase가 됨.
하나의 행렬이 될 때 0

#### 계산순서

![](/assets/Algorithm/short/dp4-3.JPG)

bottom up의 방식으로 해야함
m[i,i], m[i,i+1], .... m[i,j-1]
i행의 대각선의 값을 다 알아야 계산할 수 있다?
빨간색의 왼쪽의 값과 아랫쪽 값을 다 알아야 한다.

#### 동적계획법

```java
int matrixChain(int n)
{
    for(int i=1; i<=n; i++)
        m[i][i]=0; //대각선의 값은 0으로 채우고 시작
    for(int r=1; r<=n-1; r++){ //1개의 대각선을 채우니, 남아있는 대각선은 2~n까지므로 n-1번 loop 돌아야함.
        for(int i=1; i<=n-r; i++)//2 대각선 개수는 n-1, 3 대각선은 n-2.. 각 대각선의 값의 개수
        int j = i+r; //r번쨰 대각선의 i번쨰 값
        m[i][j] = m[i+1][j] + p[i-1]*p[i]*p[j]; 
        for(int k = i+1; k<=j-1; k++){
            if(m[i][j]> m[i][k] + m[k+1][j]+p[i-1]*p[k]*p[j])
                m[i][j] = m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j];
        }
    }
    return m[1][n];
}

시간복잡도 Θ(n³)
```

