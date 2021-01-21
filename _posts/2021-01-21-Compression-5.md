---
layout: post
title:  "[알고리즘] Case study - Huffman Coding 5"
date:   "2021-01-21 18:06:52"
author: Hannah-B
categories: Algorithm
tags: 허프만코딩 압축
---

## CodeWord 검색하기

#### 인코딩

- 데이터 파일을 압축하기 위해선, 데이터 파일을 다시 시작부터 읽으면서 run을 하나씩 인식한 후 해당 run에 부여된 codeword를 검색한다.
- Huffman트리에는 모든 run들이 리프노드에 위치하므로 검색하기에 불편함. 
- 검색하기 편리한 구조를 만드려면?
  중간 노드는 허프만 코드를 만들기 위해 부여되는 것이라 트리 구조는 필요가 없음. 리프노드의 코드워드만 있으면 된다.

![](/assets/Algorithm/short/comp5-1.PNG)

AAA라는 run이 있을 때, symbol=A, runLen=3
표 상에서 right이 이어지므로 AAA, B, C가 됨
next field가 필요한데, right, left field를 만들어서 그 이전/다음 필드의 주소가 들어간다.
chars라는 배열에 256개의 바이트를 저장한다. 그리고 배열 인덱스로 꺼냄.
0~255 사이의 칸에 A로 연결된 run의 연결리스트를 hashing한다.

#### storeRunsIntoArray

```java
private Run[] chars = new Run[256];

/*Huffman트리의 모든 리프노드들을 chars에 recursion으로 저장한다.*/
private void storeRunsIntoArray(Run p){
  if (p.left == null && p.right == null){
    insertToArray(p); //배열 (chars(unsigned int)p.symbol)가 가리키는 연결리스트의 맨 앞에 p를 삽입한다.
  } else {
    storeRunsIntoArray(p.left);
    storeRunsIntoArray(p.right);
  }
}

public void compressFile(RandomAccessFile fin){
  collectRuns(fin);
  createHuffmanTree();
  assignCodewords(theRoot, 0, 0);
  storeRunsIntoArray(theRoot);
}
```

#### Run 검색하기

symbol과 runLength가 주어질 때, 배열 chars를 검색하여 해당하는 run을 찾아 반환하는 메서드를 작성한다.

```
public Run findRun(byte symbol, int length){
  /*배열 chars에서 (symbol, length)에 해당하는 run을 찾아 반환한다.*/
}
```

