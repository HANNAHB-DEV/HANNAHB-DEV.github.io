### 인코딩 하기

#### 인코딩

- 압축파일의 맨 앞부분(Header)에 원본파일을 구성하는 run들에 대한 정보를 기록한다.
- 이 때, 원본 파일의 길이도 함께 기록한다. 원본 파일의 길이를 모르면, padding 역할을 하는 padding bit인지 실제 데이터 길이인지 모르므로 기록을 해 주어야 함.
- 원본이 bit여도 적어도 byte 단위로 인코딩 한다.

#### outputFrequencies

원본파일 구성하는 run들과, 그 freq를 출력하고 헤더에 저장하는 메소드

```java
private void outputFrequencies(RandomAccessFile fin, RandomAccessFile fOut) throws IOException{ //fIn:압축할 파일,fOut:압출된 파일
  
  fOut.writeInt(runs.size()); //run의 개수를 하나의 정수로 출력
  fOut.writeLong(fIn.getFilePointer()); //원본 파일의 크기(byte)단위를 출력
  
  for(int j=0;j<runs.size();j++){ //각각의 run들을 출력함
    Run r = runs.get(j);
    fOut.write(r.symbol);
    fOut.writeInt(r.runLen);
    fOut.writeInt(r.freq);
  }
}
```

#### compressFile

```java
puvlic void compressFile(String inFileName, RandomAccessFile fIn) throws IOException{
  //fIn:압축할 파일, inFileName:그 파일 이름. 압축된 파일의 이름을 정하기 위해서 parameter로 받음
  String outFileName = new Stirng(inFileName + ".z");//압축파일의 이름은 파일이름.z
  
  RandomAccessFile fOut = new RandomAccessFile(outFileName, "rw");//압축파일을 여기서 생성하여, outPutFrequencies와 encode메서드에 제공
  
  collectRuns(fIn);
  outputFreqeuncies(fIn, fOut);
  createHuffmanTree();
  assignCodewords(theRoot, 0,0);
  storeRunsIntoHashMap(theRoot);
  fIn.seek(0);
  encode(fIn, fOut);
}
```

main

```java
public class HuffmanCoding{
  
  ...
  public void compressFile(String inFileName, RandomAccessFile fIn) throws IOException{
    ...
  }
  
  static public void main(String args[]){
    HuffmanCoding app = new HuffmanCoding();
    RandomAccessFile fIn;
   try{
     fIn = new RandomAccessFile("sample.txt", "r");
     app.compressFile("sample.txt", fIn);
     fIn.close();
   } catch (IOException io){
     System.err.println("cannot open" + fileName);
   }
  }
}
```

#### encode()

encode를 위하여 하나의 buffer를 사용한다.

이미지

1. collectRuns를 실행하여 run을 인식하고, codeWord를 검색해 압축파일에 적어주어야 한다.
2. buffer를 사용하여 비트를 채워 나가고, byte가 꽉 차면 실제로 파일에 write하게 된다

```java
private void encode(RandomAccessFile fIn, RandomAccessFile fOut){
  while there remains bytes to read in the file{
    recognize a run; //파일을 읽으며 run을 인식하고(compressRun)
    find the codeword for the run; //run에 해당하는 codeword를 찾음. HashMap의 map.get(newRun(symbol,runLen)); 임시 런 객체를 생성하여 Hashmap에서 searching.
    pack the codeword into the buffer;
    if the buffer becomes full
      write the buffer into the compressed file;
  }
  if buffer is not empty{ //buffer가 완전히 비어져 있지 않다면,
    append 0s into the buffer;
    write the buffer into the compressed file;
  }
}
```

#### class HuffanEncoder

```java
public class HuffmanEncoder{
  static public void main(String args[]){
    String fileName="";
    HuffmanEncoding app = new HuffmanCoding)_;
    RandomAccessFile fIn;
    Scanner kb = new Scanner(System.in);
    try{
      System.out.pringln("Enter a file name" );
      fileName = kb.next();
      fIn = new RandomAccessFile(fileName, "r");
      app.compressFile(fileName, fIn);
      fIn.close();
    } catch(IOException io){
      System.err.println("Cannot open" + fileName);
    }
  }
}
```

