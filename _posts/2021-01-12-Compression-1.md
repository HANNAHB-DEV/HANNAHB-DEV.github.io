---
layout: post
title:  "[알고리즘] Case study - Huffman Coding 1"
date:   "2021-01-12 21:59:52"
author: Hannah-B
categories: Algorithm
tags: 허프만코딩 압축
---

## 압축(compression)

### Huffman Coding

데이터 압축 알고리즘, 이에는 무손실과 손실 압축이 존재.
ex) 이미지, 동영상 multimedia의 경우 : 손실압축기법
​	텍스트, 데이터의 경우 무손실 압축. 압축 해제 떄 원본과 동일하게 보존.
허프만 코딩은 무손실 압축 알고리즘이다.

- if) 6개의 문자 a~f 로 이루어진 파일이 있을 때, 문자의 총 개수는 100000개이고 각 문자의 회수가 다음과 같을 때

  ![](/assets/Algorithm/short/comp1.PNG)

- 고정길이코드를 사용하면 각각의 문자를 표현하기 위해 3비트 필요, 파일의 길이는 300000비트가 됨.

- 위 테이블 가변길이 코드를 사용하면 224000비트가 됨
  즉, 압축효과가 더 좋아짐. 디코딩이 가능하도록 규칙적으로 만들어진 코드
  -> prefix code

### Prefix Code

- 위의 101 같이 어떤 codeword도 다른 codeword의 prefix가 되지 않는 코드, 여기서 codeword란 하나의 문자에 부여된 이진코드
  fixed-length code는 무조건 prefix code.
- 모호함이 없이 decode가 가능함
- prefix code는 하나의 이진트리로 표현 가능함

  ![](/assets/Algorithm/short/comp2.PNG)

(a) a 000, b 001...
(b) a 0, c, 100....
우리가 부여한 코드 워드, 문자 노드들은 트리의 leaf가 된다. 모든 문자 노드들이 leaf라면 prefix 코드이다.
Prefix는 하나의 이진트리로 표현 가능 하고, 문자 노드들이 leaf이다.

데이터를 인코딩하기 위해 만든 prefix 들 중에서 가장 optimal하게 만드는 코딩을 허프만 코딩이라고 한다.

  ![](/assets/Algorithm/short/comp3.PNG)

.09, .12, .19, .21, .39 <- a, b, c, d, e빈도의 퍼센테이지라고 가정. 상대적 빈도. 
(1) 다섯개의 빈도값 중 가장 작은 값을 찾는다. 9, 12를 자식 노드로 가지는 부모 노드를 하나 추가한다.
(2) 둘을 더 하면 ,21, 19, 21, 39. 또 이 중에서 가장 작은 값 두개를 찾는다. 왼쪽은 왼쪽 21, 오른쪽은 오른쪽 21을 묶었을 때이다.
(3) 새롭게 만들어진 부모 노드들은 자식 노드 둘을 합한 값이다.

  ![](/assets/Algorithm/short/comp4.PNG)

(4) 40+60은 100이므로 1
(5) a=000,  b=001, ... e=11
(6) 코드 워드가 다름. e가 01이고니까.. 전에껀 e가 11

  ![](/assets/Algorithm/short/comp5.PNG)
  ![](/assets/Algorithm/short/comp6.PNG)
P, Q, R, S, T가 등장하는 빈도가 0.1, 0.1, 0.1, 0.2, 0.5 일때로 가정하여 가지를 그림. 누구를 먼저 선택하느냐에 따라 다른 코드를 가질 수 있다. 코드가 달라지더라도 데이터 파일을 인코딩 했을 때 파일의 길이는 같다.

### Run-Length Encoding (lossless)

- Run은 동일한 문자가 하나 혹은 그 이상 연속해서 나오는 것.
  ex) String s = "aaabba"일 경우 "aaa", "bb", "a"가 각각의 런
- run-length encoding에서는 각각의 run을 그 "run을 구성하는 문자"와 "run의 길이"의 순서쌍 (n,ch)로 인코딩. 여기서 ch가 문자, n은 길이.
  ex) String s = 3a2b1a
- Run-length encoding은 길이가 긴 run들이 많은 경우에 효과적
  ex) aaaaaa...a 보다는 30a가 효과적
