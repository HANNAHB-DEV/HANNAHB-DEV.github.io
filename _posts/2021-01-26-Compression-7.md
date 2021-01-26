### 압축7 : 디코딩하기

#### Class HuffmanDecoder

```java
public class HuffmanDecoder{
	static public void main(String args[]){
		String fileName="";
		HuffmanCoding app = new HuffmanCoding();
		RandomAccessFile fIn;
		Scanner kb = new Scanner(system.in);
		try{
			System.out.print("Enter a file name:"); //압축파일을 입력받아
			fileName = kb.next();
			fIn = new RandomAccessFile(fileName, "r");
			app.decompressFile(fileName, fIn);
			fIn.close();
		} catch (IOException io){
			System.err.println("Cannot open" + fileName);
		}
	}
}
```

#### decompressFile (HuffmanCoding class)

```java
public void decompressFile(String inFileName, RandomAccessFile fIn) throws IOException{ //압축해제할 파일을 받음
	String outFileName = new String (inFileName + ".dec");
    RandomAccessFile fOut = new RandomAccessFile(outFileName, "rw");
    inputFreqeunceis(fIn); //디코딩 시작, 압축된 헤더를 복원 시키는 것
    createHuffmanTree();
    assignCodewords(theRoot,0,0);
    decode(fIn, fOut);
}
```

압축파일을 생성할 때 헤더 부분에 압축하기 위한 허프만 코드를 읽어와야 뒷 부분을 decoding할 수 있다. 

#### inputFreqeuncies

```java
private void inputFrequencies(RandomAccessFile fIn) throws IOException{
	int dataIndex = fIn.readInt(); //run의 개수
	charCnt = fIn.readLong(); //원본파일의 길이(byte)
	runs.ensureCapacity(dataIndex); //runs:ArrayList 배열의 길이는 고정이여서, 그 길이의 배열을 만드는 것이 효율적임
	for(int j=0;j<dataIndex;j++){
		Run r = new Run();
		r.symbol = (byte)fIn.read();
		r.runLen = fIn.readInt();
		r.freq = fIn.readInt();
		runs.add(r);
	}
}
```

#### decode

```java
private void decode(RandomAccessFile fIn. RandomAccessFile fOut) throws IOExcpetion{
 //leaf node까지 내려가는 동안 bit들을 읽어 내려감
}
```

