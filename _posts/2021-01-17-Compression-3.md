---
layout: post
title:  "[알고리즘] Case study - Huffman Coding 3"
date:   "2021-01-17 11:44:52"
author: Hannah-B
categories: Algorithm
tags: 허프만코딩 압축
---

### Huffman Coding

- 트리들의 집합을 유지하면서
- 매 단계에서 가장 frequency가 작은 두 트리를 찾아서
- 두 트리를 하나로 합침(마지막 한개가 남을 때까지)
- 이런 연산에 가장 적합한 자료 구조는 최소 힙(minimal heap)이다.
  =우선순위 queue
- 즉, 힙에 저장된 각각의 원소들은 하나의 트리.(not 노드, run객체)
- extractMin, insert를 사용

### 최소 힙

![](/assets/Algorithm/short/comp3-1.PNG)

5개의 run, 각각의 run을 single node tree로 본다.
5개의 single-node tree를 heap에 저장하는데, minimal heap은 complete binary tree이며, heap property를 만족해야함(부모는 자식보다 작거나 같다.)
크기의 기준은 frequency이며, 부모가 자식보다 작으므로 complete binary tree임을 알 수 있다. 각각의 노드들은 노드가 아니라 tree이며(single-node tree)임

1차원 배열 heap, 부모노드가 i일 때, 왼쪽 자식은 2i+1, 오른쪽 자식은 2i+2
(배열의 값을 통해 누가 누구의 자식이고 부모인지 알 수 있다.)

부모에서 자식, 왼쪽에서 오른쪽 방향으로 heap에 넣어준다.

![](/assets/Algorithm/short/comp3-2.PNG)

1. heap을 만들어야 함, 부모는 자식보다 작아야 하며 complete binary tree의 형태로 만들어야 한다.
2. frequency가 최소인 2개의 tree를 추출해낸다(extractMin)
   항상 root가 최소값이 므로 root를 선택하고, 맨 마지막 노드(맨 마지막 레벨의 오른쪽 노드)가 위로 올라가고, heapify를 해야함.
3. A11을 삭제, B12를 루트로 가져와 다시 heapify(더 작은 값을 선택)
4. 삭제된 두 트리를 합치기 위해 루트노드로 자식노들의 합인 트리를 만든 다음, 다시 heap에 insert함(single-node tree아님)
5. heap size=4, 그 중 하나는 single-node 트리가 아니라 3개의 트리가 합쳐진 트리

![](/assets/Algorithm/short/comp3-3.PNG)

6. 다음의 과정을 계속해서 반복함
7. C21, --2가 삭제되고 또 둘을 합쳐서 부모노드를 만들어 다시 트리에 insert함
8. A22 - B12 - --3 heap size=3.

![](/assets/Algorithm/short/comp3-4.PNG)

9. A22, B12를 삭제, 이 둘을 합쳐 - - 4
10. 다시 insert, heap size = 2. 노드가 3개인 트리 하나, 5개인 트리 하나.

![](/assets/Algorithm/short/comp3-5.PNG)

11. extractMin, heap은 empty상태가 됨
12. 4+3인 부모 노드를 만들어(extracMin) insert한다. heap size=1
    Huffman Tree, root를 theRoot

```java
class Run implements Comparable<>
	public byte symobl;
	public int runLen;
	public int freq;
/*트리의 노드로 사용하기 위해서 왼쪽, 오른쪽 자식 노드 필드를 추가한다.*/
/*각각의 tree의 값을 비교해야 하므로 두 run의 크기관계를 비교하는 compareTo 메소드를 overriding*/
/*비교의 기준은 freq*/
```

- Heap class를 가져와서 사용, Generics로 수정 후 heapift, insert, extractMin 등의 함수들을 min heap에 맞게 수정

```java
public class HuffmanCoding{
  private ArrayList<Run> runs = new ArrayList<Run>();
  private Heap<Run> heap; //minimum heap
  private Run theRoot = null; //Huffman tree의 루트노드
  
  private void createHuffmanTree(){
    heap = new Heap<Run>();
    
    /* 1. 모든 런을 힙에 저장한다.*/
    /* 2.힙의 사이즈가 1보다 크면 계속해서 반복한다(=1이 될대까지 반복)*/
    /*	(1)extractMin을 두번 반복한다.*/
    /*	(2)삭제된 트리들을 합한 트리를 만든다.*/
    /*	(3)합쳐진 트리를 다시 힙에 insert한다.*/
    /* 3.허프만 트리의 root를 트리의 root가 되도록 한다.*/
  }
}
```

Huffman Tree 출력해보기

```java
private void printHuffmanTree(){
  preOrederTraverse(theRoot, 0);
}

private voide preOrederTraverse(Run node, int depth){
  for(int i=0;i<depth;i++){
    System.out.print(" ");
    if(node == null){
      System.out.println("null");
    } else {
      System.out.println(node.toString());
      preOrderTraverse(node.left, depth+1);
      preOrderTraverse(node.right, depth+1);
    }
  }
}
```

- 들여쓰기를 이용하여 tree의 level을 표현함
