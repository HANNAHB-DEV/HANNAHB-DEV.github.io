## Huffman Method with Run-Length Encoding

- 파일을 구성하는 각각의 run들을 하나의 super-symbol로 본다. 이 super-symbol들에 대해서 Huffman coding을 적용한다
- ex) 문자열 AAABAACCAABA는 5개의 super-symbol들 AAA, B, AA, CC, A이며 super-symbol들의 등장회수는 다음과 같다.

|            | AAA  |  CC  |  A   |  B   |  AA  |
| ---------- | :--: | :--: | :--: | :--: | :--: |
| symbol     |  A   |  C   |  A   |  B   |  A   |
| run length |  3   |  2   |  1   |  1   |  2   |
| frequency  |  1   |  1   |  1   |  2   |  2   |

if) AAA = 0, B=10, AA=110, CC=1110, A1=11110로 코드를 부여한다면(prefix code), 0101101110110100으로 인코딩됨. 20bits.
서로 다른 문자들에게 코드를 부여하지 않고, run을 대상으로 부여하고 카운트를 하도록 한다.

이미지 2-1

1. 다섯개의 super-symbol들 중에서 빈도가 가장 적은 것을 고른다. if AAA, CC를 고른다면, 두개를 묶어 트리를 만들고 부모로 1+1=2(빈도)를 가진다.
2. 1,2,2,2가 있을 때 A, B를 고른다고 가정하면 3이 됨.
3. 3,2,2일 때 AA와 AAA,C의 합트리일 때는 4.
4. 둘을 합쳐 7이 됨
5. 왼쪽 노드를 0, 오른쪽을 1이라고 정한다면, A=00, B=01, AA=10.. 

### 제1 단계 : Run과 Frequency 찾기

- 압축할 파일을 처음부터 끝까지 읽어서 파일을 구성하는 run들과 각 run들의 등장횟수를 구한다.
- 먼저 각 run들을 표현할 하나의 클래스 class Run을 정의함.
  클래서 run은 적어도 세 개의 데이터 멤버 symbol, runLen, freq를 가져야 한다. 
  여기서 symbol은 byte, 나머지는 정수들이다.


- 인식한 run들은 하나의 ArrayList에 저장한다.
- 적절한 생성자와 equals 메서드를 구현한다.

```java
class Run{
  byte symbol;
  int runLen;
  int freq;
}
```

데이터 파일은 적어도 두 번 읽어야 한다. 한번은 run을 찾고, 다음은 압축을 실행하기 위함.
여기서는 RandomAccessFile을 이용하여 데이터 파일을 읽어본다.

```java
/*읽을 데이터 파일을 연다*/
RandomAccessfile fin = new RandomAccessFile(fileName, "r");

/*한 byte를 읽어온다. 읽어온 byte는 0~255사이의 정수로 반환된다.*/
/*파일의 끝에 도달하면 -1을 반환한다.*/
int ch = fin.read();

/*필요하다면 byte로 casting해서 저장한다.*/
byte symbol = (byte) ch;
```

### Run 인식하기

AAABBCAAB*...

1. 파일의 첫 byte를 읽고, 이것을 start-symbol이라고 한다. ==> A
2. 파일의 끝에 도달하거나, 혹은 start_symbol와 다른 byte가 나올 때 까지 연속해서 읽는다. 현재까지 읽은 byte 수를 count라고 할 떄, 여기서는 count=4.
3. (start_symbol, count-1)인 run이 하나 인식됨. 이 run을 저장하고 가장 마지막에 읽은 byte를 start_symbol로, count=1로 reset하고 다시 반복함

```java
/*적절한 생성자와 equals 메서드를 완성*/
class Run{
  public byte symbol;
  piblic int runLen;
  public int freq;
}
```

```java
public class HuffmanCoding{
  /*인식한 run들을 저장할 ArrayList를 만듦*/
  private ArrayList<Run> runs = new ArrayList<Run>();
  
  private void collectRuns(RandomAccessFile fin) throws IOException{
    /*데이터 파일 fin에 등장하는 모든 run들과 각각의 등장횟수를 count하여 ArrayList에 저장한다.*/
  }
  
  static public void main(String args[]){
    HuffmanCoding app = new HuffmanCoding();
    RandomAccessFile fin;
    try{
      fin = new RandomAccessFile("sample.txt","r");
      app.collectRuns(fin); //위의 method
      fin.close();
    } catch (IOException io){
      System.err.println("Cannot open" + fileName);
    }
  }
}
```

```c
typedef struct run Run;
typedef struct run{
  unsigned char symbol;
  int run_length;
  int freq;
};

Run *runs[MAX];
int number_runs=0;

void collectRuns(File *fin){
  /*데이터 파일 fin에 등장하는 모든 run들과 각각의 등장횟수를 count하여 배열 runs에 저장한다.*/
  /*Run객체들을 동적메모리할당으로 생성하여 배열 runs에서 객체의 주소를 저장하고*/
  /*배열의 크기가 부족할 경우 array doubling을 하도록 구현하라*/
}
```

