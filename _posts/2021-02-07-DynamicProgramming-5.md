동적계획법5

#### Longest Common Subsequence(LCS)

- 입력으로 2개의 문자열이 주어진다.
- bcdb는 문자열 abcdbdab의 subsequence이다. (순서 그대로)
- bca는 문자열 abcbdab와 bdcabd의 common sequence이다. 공통으로 들어가 있는 것.
- Longest common subsequence(LCS)
  - common subsequence들 중 가장 긴 것
  - bcba는 abcddab와 bdcaba의 LCS

![](/assets/Algorithm/short/dp5-1.PNG)

최적해의 한 부분을 구해야 한다. x와 y열이 입력으로 들어왔고, z가 이 둘의 LCS(최적해)일 때, 최적해의 일부분이 x,y에 대응하는 부분이 있음. A를 제외한 보라색은, X'와 Y'의 LCS이다. 

![](/assets/Algorithm/short/dp5-2.PNG)

X= x1, x2 ... xm / Y=y1, y2, ... yn
경우 1) 두 문자열이 같다면, X1..xn-1, Y...Yj-1의 LCS를 구해서 + A한게 치적해

![](/assets/Algorithm/short/dp5-3.PNG)

같지 않다면, 둘 중 하나는 버려야한다, xi를 버린다면 x1... xi-1과 y1..yj의 LCS를 구하는 것

![](/assets/Algorithm/short/dp5-4.PNG)

![](/assets/Algorithm/short/dp5-5.PNG)

i, j를 알기 위해서는 (i, j-1) (i-1, j) (i-1,j-1) 값을 미리 알아야 함.

```java
int lcs(int m, int n) //m:length of X, n:length of Y
{
  for (int i=0; i<=m; i++){
    c[i][0]=0;
  }
  for(int j=0; j<=n; j++){
    c[0][j] = 0;
  }
  for(int i=0; i<=m; i++){
    for (int j=0; j<=n; j++){
      if(x[i]==y[j])
      	c[i][j] = c[i-1][j-1] +1; //같으면 1을 구함
      else
      	c[i][j] = Math.max(c[i-1][j], c[i][j-1]); //둘 중 긴것을 선택
    }
  }
  return c[m][n];
}

시간복잡도 Θ(mn)
```

