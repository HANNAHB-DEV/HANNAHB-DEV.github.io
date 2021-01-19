### 3단계 : Codeword 부여하기

image1

코드워드를 부여할 run은 leaf node.
RUN A22, B12, .... 와 같이. theRoot의 왼쪽은 0, 오른쪽은 1로 정의해 각각의 트리에 코드워드를 부여한다.

```java
assignCodeword(prefix, node){
  if node is a leaf
  	assign prefix to the node; //리프노드라면 코드워드 부여
  else
  	assignCodeword(prefix+'0', node.left);
  	assignCodewrod(prefix+'1', node.right);
}
```

- 여기서 prefix를 하나의 32비트 정수로 표현한다. 그러나 32비트 중에서 하위 몇 비트만이 실제로 부여된 codeword이다. 따라서 codeword의 길이를 따로 유지해야한다.

````java
class Run implements Comparable<Run>{
  public byte symbol;
  public int runLen;
  public int freq;
  
  /*트리의 노드로 사용하기 위해서 왼쪽 자식과 오른쪽 자식 노드 필드를 추가한다.*/
  /*노드에 부여된 codeword를 저장하기 위한 필드들을 다음과 같이 추가한다.*/
  
  public int codeword;		//부여된 codeword를 32비트 정수로 저장
  public int codewordLen;	//부여된 codeword의 길이, 즉 codeword의 하위 codewordLen비트가 실제 codeword
}
````

### 자바에서의 비트연산

```java
public class Test{
  public static void main(String args[]){
    int a=60; /*60= 00111100*/
    int b=13; /*13= 00001101*/
    int c=0;
    
    c = a & b; /*12= 00001100 둘다1인 애들만 1*/ 
    System.out.println(" a & b = "+c);
    c = a | b; /*61= 00111101 둘다0인 애들만*/
    System.out.println(" a | b = "+c);
    c = a ^ b; /*49=00110001 같으면0, 다르면1*/
    System.out.println(" a & b = "+c);
    c = -a; /*-61=11000011 끝에 1하나 추가*/
    System.out.println("-a = "+c);
    c = a << 1;
    System.out.println("a << 2 = "+c);
    c = (a << 1) + 1;
    System.out.println("a << 2 = "+c);
  }
}
```

### codeword부여하기

```
private void assignCodewords(Run p, int codeword, int length){ //assignCodewrods(theRoot,0,0)으로 호출한다.
  if (p.left == null && p.right = null){ //leafonde라면
    p.codeword = codeword;
    p.codewordLen = length;
  } else{
    assignCodewords(,,); //왼쪽 자식노드에게는 codeword+0
    assignCodewords(,,); //오른쪽 자식노드에게는 codeword+1
  }
}
```

### main과 compressFile 메서드

```java
public class HuffmanCoding{
  ...
  
  public void compressFile(RandomAccessFile fin){
    collectRuns(fin);
    createHuffmanTree();
    assignCodeword(theRoot, 0,0);
  }
  
  static public void main (String args[]){
    HuffmanCoding app = new HuffmanCoding();
    RandomAccessFile fin;
    try{
      fin = newRanomAccessFile("sample.txt","r");
      app.compressFile(fin);
      fin.close();
    } catch(IOException io){
      System.err.println("Cannot open" + fileName);
    }
  }
}
```

