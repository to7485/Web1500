## IO(Input, Output)
- IO는 입출력 스트림을 의미합니다.
- 스트림이란, 데이터를 입출력하기 위한 방법.
프로그램에서 파일을 읽어온다든지, 콘솔에서 키보드값을 얻어오는 등의 작업을 할 수 있습니다.
자바 가상머신에서 콘솔로 값을 보낼땐 Output, 반대로 콘솔의 값을 JVM에서 읽을땐 Input

![image](https://user-images.githubusercontent.com/54658614/221475515-1bbfdce7-6880-413f-8023-3d5fbd43f2b7.png)


ex1_File 패키지 생성
 
#### Ex1_File 클래스 정의
```java
test.txt 문서를 만들고 안녕하세요 라고 적은 뒤에 d:/에 가져다 놓은 뒤에 코드 작성
public class Ex1_File {
public static void main(String[] args) {

//IO(Input,Output) : 입출력 스트림
//스트림이란 데이터 입력을 입출력하기 위한 통로


//JVM에서 콘솔로 값을 내보낼 때는 Output, 콘솔의 값을 JVM에서 읽어올 때는 Input

우리가 늘 작업하던 경로로 들어와서 메모장을 만든다.web_14.lhj
test.txt 
보기탭 누르고 파일 확장명 체크 되어있는지 확인
안녕하세요 작성 후 다른이름으로 저장

문자열이 나열된 형태가 타입이 utf-8 아스키코드에 존재하지 않는 언어를
깨지지 않게 표시하기 위한 언어형태
ANSI로 저장하기 자바에서 테스트 하는 동안에만 ANSI로 저장할꺼임

// 파일 객체를 생성할 경로 
String path = "d:/web_14_java/test.txt";  
        
// 준비된 경로로 File객체 생성   
File f = new File(path);
//File 클래스가 생성되면서 path경로까지 스트림을 생성

통로를 만들어 준다고 생각하면 된다. 앞으로 내가 만든 파일 이라는 클래스는 path까지 다이렉트로 접근하는 터널같은 것. 중간에 경로가 잘못 되지 않았는지까지도 판별해준다.
        
//f.isFile() : 파일클래스가 접근한 최종 목적지가 파일형식일 경우 true	if(f.isFile()){// 생성된 객체가 파일일 경우 !f.isFile() 
    System.out.println(f.length() + “byte”); 한글은 2byte, 영문 특수문자 1byte 
폴더의 용량도 가져올 수 있다. 파일이든 폴더든 나의 최종 목적지에 해당하는 용량을 가져올 수 있다.
  }
}
13이 안나온다면 띄어쓰기가 포함되어 있을 수 있다.
```

#### Ex2_File 클래스 정의

```
public class Ex2_File {	
  public static void main(String[] args) {
  // 파일객체로 쓰인 경로
  String path = "D:/web14_lhj/";
  //강의에 사용중인 폴더 경로 지정 \\ 쓰려면 두 개 써줘야 함
  폴더경로 보여주며 여기까지 접근한다는걸 설명 이 폴더가 가진 하위 목록들의 이름까지 가져올 수 있다.

  파일클래스가 할 수 있는 두번째 기능 폴더를 찾았을 때 내 폴더의 하위 요소들의 이름을 가져와 봅시다~

  File f = new File(path);
  파일클래스가 접근하고자 하는 최종 목저지는 폴더까지... 폴더까지 접근해야 하위 목록들의 이름을 가져올 수 있음 파일은 하위 목록이 없기 때문


  if(!f.isFile()){ //파일이 아닐경우,!f.isFile()), f.isDirectory() 애초에 폴더인지 판별하는 메서드도 가지고 잇다.
  // 디렉토리 안에있는 하위 요소들의 이름을 모두 가져온다.
  String[] names = f.list(); 반환형 String[] //f경로의 하위 요소들을 names배열에 저장

  왜 배열로 넘겨주느냐 list라고 하는 녀석은 (폴더경로 보여주며) 내가 접근한 경로에 하위 목록을 배열형태로 정렬을 해서 이름을 알파벳 순서대로 정렬까지 해놔요. 

  직접 세보지 않고도 파일 클래스를 통해서 하위 요소들이 총 몇 개인지를 알아서 list라고 하는 메서드가 구해준다. 비어있는공간이 없도록 딱 맞는 공간에 맞게 크기를 제공한다.

  경로를 입력 받을수도 있어서 배열의 크기를 미리 만들어 놓을수가 없다.

  파일 클래스에는 폴더인지 파악하는 기능, 하위 요소들의 이름을 꺼내 오는 기능도 있다.

//개선된 roop문
for(String s : names) {
    System.out.println(s);	
    }	
  }
}
```

#### Ex3_File 클래스 정의

```java
public class Ex3_File {	
	public static void main(String[] args) {	

String path = "D:/web14_lhj/aaa/bbb";//만들어질 폴더
파일클래스가 하는 기능중에 어지간한건 다할 수 있는데
단점은 폴더인지 파일인지 용량은 얼마인지 구분은 할 수 있는데 파일 일 경우에 열었을 때 속에 뭐라고 쓰여있는지 까지는 알 수 없다.

File f1 = new File(path);
aaa라고 하는 폴더가 물리적으로 존재하지 않는다. aaa가 없다....
내가 최종 목적지 까지 가는 길에 혹시라도 길이 존재하지 않는 경로가 포함되어있지 않은지 판단할 수 있어요.

//exists() : 파일 클래스가 path경로로 찾아가는 중
//정상적으로 폴더나 파일이 존재한다면 true를 반환
if(!f1.exists()){ //f.exists() == false
System.out.println("폴더생성");
f1.mkdirs();//폴더생성

우리가 미리 만들어 놓은거 아니죠 코드를 통해서 자동으로 만들어준거다.
실제로 되게 많은 프로그램들이 이런 기능들을 가진 클래스를 거진 가지고 있다.
예를들어 자바를 다운받았는데 자바 폴더 없으면 만들어라, 안에 버전에 맞는 폴더를 또 만들어라.
이렇게 메모장으로 던져주면 아무도 설치를 하려 하지 않을 것이다.

파일클래스는 특정 문서를 만드는 기능은 없다 클래스 파일은 폴더만드는거 까지만 가능하다.


    	}
   }
}
```

### FileInputStream

ex2_fileInput 패키지 생성

#### ex1_FileInputStream 클래스 생성
- 클래스가 못하는 내용까지 읽는 stram이라고 하는애가 할 수 있다.

```java
public class Ex1_FileInput {
	public static void main(String[] args) {

	Stream에 대한 특징

	//...Stream : byte기반의 스트림 -> 실무에서는 훨씬 많이 씀 한글데이터를 직접 읽어오는 경우가 거의 없음 읽어와도 db를 통해서 읽어옴
	//...Reader, ...Writer : char기반의 스트림


	String path = "D:/web14_lhj/test.txt“;
	//위 예제에서 만든 test.txt문서

	File f = new File(path);
	파일 클래스를 만들어서 경로에 오타가 났다거나 없는 경로가 있지 않은지 탐사를 보내요~

	if(f.exists()){ //파일이 실제 존재할 때만 수행!
		try{
			// 파일클래스가 가진 경로로 접근하기 위한 입력 스트림 생성	
			FileInputStream fis = new FileInputStream(f);
			밖에 있는걸 읽어올거야~ test.txt까지 읽어오는 스트림을 만들거야
			try-catch FileNotFoundException e 너가 위에서 답사를 했지만 혹시라도 내가 만들어질 때 지정해놓은 파일이 없어질 가능성이 있다.
			예측은 가능하지만 잡을수는 없는 오류이다. 그래서 try-catch로 강제로 묶어야 한다.

			// 스트림은 더 이상 읽을 것이 없다면 -1을 반환하게 되어 있다.
			// 즉 파일의 모든 내용을 읽어오기 위해 반복문을 수행하고
			// 그 반복은 파일의 끝(EOF)인 -1을 만날 때까지 반복하면 된다.

			int code = 0;

			fis는 txt까지 접근하는 작업 read라는 메서드가 안에있는 글자를 읽어오는 작업을 한다.
			read()메서드는 한번에 한번에 한글자만 읽을 수 있음 안타까운점은 read라는 메서드가 텍스트를 가지고 올 때 1byte씩 밖에 읽어오지 못한다.
			fis는 한글데이터를 읽는게 불가능 하다. 지금 당장은 기본적으로 read를 통해서 읽어올 때는 1byte로 밖에 읽어올 수 없기 때문에 2byte로 이루어진 한글은
			불가능하다.	

			//read()메서드는 한번에 1byte씩 읽어오다가
			//더이상 읽어올 단어가 업다면 문장의 끝(End Of File)인 –1을 가져온다.
			아스키코드, 유니코드에서도 –1를 갖고 있는게 없기 때문에 –1로 정함
			while((code = fis.read()) != -1) {

			//이것을 int 자료형에 담아 char로 형변환하여 출력하면
			//아스키 코드값으로 변경되어 출력되기 때문.
   				System.out.print((char)code); 
   			}
			//스트림은 사용이 완료된 이후 close를 통해 닫아주는 것이 좋다.
			//그래야 다음 작업을 하는데 문제가 생기지 않는다. 
			//close를 작성하지 않았을 때 아직도 할 작업이 남았으니까 화면에 띄우거나 파일을 만들면 안되겠구나 착각하는 경우가 있기 때문이다.
		 	fis.close();
			read메서드 사용하면 IOException 만들어짐 그래서 그냥 Exception 만들고 한번에 처리
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
```

#### Ex2_FileInput클래스 정의

```java
public class FileInput {
public static void main(String[] args){

	String path = "D:/web14_lhj/test.txt“;

	File f = new File(path);
	//넉넉하게 100. 배열은 int범위까지만 만들 수 있다.
	byte [] read = new byte[(int)f.length()];

	배열의 크기를 딱 맞춰서 메모리 낭비를 하지 않는게 좋지만 키보드에서 그때그때 다른 파일을 받는다면 용량을 알 수 없다.
	근데 length()가 long타입이라서 (int)로 형변환을 해야한다.
	
	if(f1.exists()){ //파일이 실제 존재할 때만 수행!
	   try{
		FileInputStream fis = new FileInputStream(f);
		fis.read(read);
 		fis가 읽어온걸 read 배열에 넣어주는데 방 두 개를 점유하면서 한글이 저장된다.
 		//현재 byte[]인 read에는 test.txt.에서 읽어온 데이터들이 저장되어 있다.
		//이를 문자열 형태로 재조합하여 출력할 수 있다.
		String res = new String(read); 
		스트링 클래스에 생성자가 오버로딩이 굉장히 많이 되어있다. 그중에 바이트 배열을 넣어주면 문자열로 바꿔주는 오버로딩이 있다.

		바이트 형태로 가져오는 바람에 한글이 다 깨진다면 배열을 준비해서 그 배열에 담긴 값을 문자열로 바꿔보면 어때? 라고 하는 차선택이 있다는 겁니다.
		System.out.println(res);

		//스트림 사용을 완료한 뒤에는 반드시 닫아주자!!
		fis.close();
 	   } catch(Exception e) {
		e.printStactTrace();
	   }
   	}
   }
```

#### Ex3_FileInput클래스 정의 – Scanner의 원리
```java
public class FileInput {
	public static void main(String[] args) {
	String path = "d:/test.txt";

File f = new File(path);
byte [] keyboard = new byte[100];

	try {
	System.out.print(“값: ”);
		//키보드에 값을 입력받기 위한 표준입력장치 스트림
		System.in.read(keboard);
	 
	String s = new String(keboard).trim();
	System.out.println(s)

	Scanner sc = new Scanner(System.in);
	//sc.close();

	System.in이 스테틱이라서 메모리에 한번만 올라가고 close로 닫게 되면 다른 클래	스에서도 닫아진다.
	} catch ( IOException e) {

	}

	
처음부터 스캐너를 사용하지 않고 이렇게 먼저 알려줬으면 멘탈 나가서 자바 그만뒀을 것이다.

문자와 정수를 나누는 것도 쉽지가 않기 때문이다. 이렇게 어렵게 받을 바에야 클래스를 하나 만들어서 쉽게 쓰자 라는 느낌으로 스캐너 클래스를 사용하게 된 것이다.
```

#### 자바 강의 3주차(1) 문제(회문수) 풀기

특정 경로에 file.txt문서를 만들고<br>
내용으로 아무 문장이나 입력해둔다<br>
file.txt의 내용을 FileInputStream으로 읽어온 뒤, 이 값이 회문수인지 아닌지를 판단하시오.<br>

ex1_work 클래스 생성
```java
public class Ex1_work{
public static void main(String[] args) {
	String path = "d:/file.txt";
	
	File f = new File(path);
	byte[] read = new byte[(int)f.length()]; //txt파일의 크기만큼의 배열 생성

	try{
	   if(f.exists()) {
	   	FileInputStream fis = new FileInputStream(f);
		fis.read(read);//fis가 읽어온 내용을 byte배열에 저장

		//read배열을 String타입으로 변경
		String ori = new String(read);
		String rev = “”;
		//String rev = null;  null이라고 하는 코드가 초기화 하는데 사용이 되면 		
		//heap 영역에 메모리 할당 자체가 안되어 있는다는 뜻. String 클래스는 			
		//집을 만들었는데 일단 비워놓는다는 의미로 “”로 초기화를 많이한다.
		
		rev += 1; --> rev = rev + 1; --> rev =“1”;
		rev = null; --> rev += 1; --> rev = null + 1; --> rev = “null1”

		//원본문자열인 ori를 뒤집어서 rev객체에 저장
		for(int I = ori.length()-1; I>=0; I--) {
			rev+=ori.charAt(i);
		}//for
		if(ori.equals(rev)) {
			System.out.println(ori + “는 회문수입니다”);
		} else {
			System.out.println(ori + “는 회문수가 아닙니다”):
		}
	   fis.close()
	  }//if
	} catch( Exception e) {

	}//try - catch
    }//main
}
```

ex2_outputStream 패키지

#### Ex1_PrintStream 클래스 작성 – System.out.print 에 대한 원리
public class Ex1_PrintStream{
	public static void main(String[] args) {

	//화면에 데이터를 출력하도록 하는 대표적인 클래스
	PrintStream ps = System.out; 어디서 많이 본 모양이다.

	ps.write(‘A’); 
	ps.write(‘B’);
	ps.write(‘\n’);
	ps.write(‘c’);

	
	try {
	byte[] by = {‘J’,‘A’,‘V’,‘A’};
	ps.write(by); 프린트스트림이라는 애가 write메서드에서 byte배열을 받을 수 있다.
	} catch (Exception e) {
		e.printStackTrace();
	}
	ps.close(); 내가 지금부터 코드를 기록할 예정인데 끝난거 맞어? 작업이 완전히 끝났다는걸 알려줘야 출력이 제대로 됨
	}
}

근데 우리 그러면 화면에 정수를 입력하세요 home 네글자만 받고싶어도 배열로 만들든 write를 4번 쓰든 해야한다.
system.out 하나면 해결되는데 printstream 가지고는 자바라고 하는것만 출력하려고 해도 코드를 써야되고 귀찮은 부분들이 상당히 많아지게 된다.
이렇게 고생할 필요 없이 system.out.println 화면에 출력할 수 있으니까 앞으로 이거 활용하면 된다.
println 이라고 하는걸 통해서 조금 더 간단하고 편리하게 화면에 출력해줄수 있는 요러한 기능들을 내부적으로 대체해주고 있다.
사실 공부할 필요 없음 println이 있기 때문에...
```

파일을 만드는 방법에 대해서 알아보자.<br>

#### Ex2_FileOutput – 출력하기 위한 스트림
```java
public class Ex2_FileOutput{
	public static void main(String[] args) {

	try{
	내가 기록하려고 할 때 드라이브가 뻑이난다던지 폴더가 사라질수 있기 때문에 try-catch에 넣는다.
	FileOutputStream fos = new FileOutputStream(“d:/fileOut.txt”);

	fos.write(‘f’);
	fos.write(‘i’);
	fos.write(‘l’);
	fos.write(‘e’);
	이렇게 하면 코드가 지나치게 길어지고 한글을 쓸 수가 없다.

	String msg = “fileOutput 예제입니다.\n”;
	String msg2 = “여러줄도 가능”;

	파일에 작성을 하려고 하는데 없으니 파일을 같이 만들어준다.
	이상태로 실행을 해보면 놀랍게도 우리가 지정한 경로에 fileOut 이라고 하는 파일이 만들어져있고 write한 내용도 기록되있다.
	세이브파일을 만든다거나 저장하는 문서를 만드는 경우에 요렇게 outputStream이 사용이 된다.
	확장자도 여러분 마음대로 지정할 수 있다.

	FileOutputStream에서 두 번째 파라미터로 boolean을 받으면 기존 내용에 이어붙히는 방식으로 이어붙힐수 있다.

	배열을 문자열로 바꾸는 것 String str = new String(배열);

	fos.write(msg.getBytes());//문자열 msg를 byte[]로 변경하는 메서드
	fos.write(msg2.getBytes());

	fos.close();
	} catch( Exception e) {
	
	}
    }//main
}

byte기반 스트림 끝
```
----------------------------------------------------------------------------

ex3_charStream char기반 스트림
#### Ex1_FileReader 클래스 생성 – 데이터를 읽어오는 작업
```java
Ex1_FileReader{
	public static void main(String[]args) {
	
	   //char기반의 스트림은 Reader, Writer의 자식 클래스들을 사용
	   //기본적으로 2byte를 지원하기 때문에 2byte언어로 구성된 파일도 쉽게
	   // 입출력이 가능
	try{
	   FileReader fr = new FileReader("D:/web14_lhj/test.txt“);
	   int code = 0;
	   
	   while((code=fr.read() != -1) {
		System.out.print((char)code);
		System.out.print(code + “ ”);
	1byte는 아스키 코드로 읽고 2byte는 유니코드로 알아서 읽어서 문자가 깨지거나 하는일이 없다.
	이게 더 좋은거 같은데 음악 파일같은거 전송할 때는 2바이트씩 전송하는게 좋지 않을 수 있다.
	100피스짜리 퍼즐을 옆으로 똑같이 옮긴다고 생각하면 한주먹씩 주는거보다 하나씩 주는게 상식적으로 훨씬 빠를것이다. 
	   }
		fr.close();
	} catch(Exception e) {
	
	}
     }//main
}
```


#### Ex2_FileReader클래스
```java
public class Ex2_FileReader{
	public static void main(String[] args) {

	test.txt 파일에 아무 내용이나 적는다 한글,영어 소문자 대문자 섞어서 작성
	
	//test.txt의 내용을 읽어서
	//내용에 대문자와 소문자의 개수를 출력하시오
	
	//대문자 : 7
	//소문자 : 22

	int upper = 0;
	int lower = 0;

	한글, 특수문자 판단할 필요 없다.

	try{
	FileReader fr = new FileReader("D:/web14_lhj/test.txt“);
	int code = 0;
		while((code = fr.read()) != -1) {
			if(code >= ‘A’65 && code <= ‘Z’96) {
				upper++;
			}

			if( code >=’a’ && code <=’z’) {
				lower++;
			}

		}//while
		System.out.println(“대문자: ” + upper);
		System.out.println(“대문자: ” + lower);

		fr.close();
	} catch(Exception e) {

	}
    }//main
}
```

#### Ex2_FileWriter클래스 char기반의 출력스트림
```java
public class Ex3_FileWriter{
	public static void main(String[] args) {
   

		try{
		FileWriter fw = new FileWriter("D:/web14_lhj/fileWriter예제.txt“);
		
		
		fw.write(“첫번째 줄 작성합니다 hehehe”.getBytes());
		fw.write(“\n”);
		fw.write(“두번째 줄도 문제없지 hehehe”);

		fw.close();

		} catch ( Exception e) {
			e.printStactTrace();
		}
	}
}

사실 공부를 하려면 byteStream 방식을 좀 더 공부하는게 낫다.
저희가 이제 ui쪽을 보면은 지금 우리 단계에서는 이미지파일을 전송할 수가 없어요. 그래서 저랑  fileWriter 같은건 한번 더 보지 않을까 요런거는 생각을 하고 있고 나중에 이미지 같은거 업로드 해야되면 그땐 byte기반스트림을 써야하거든요. 근데 끄때되면 바이트기반스트림으로 뭔가 업로드 하기 위한 준비가 다 되어있는 클래스를 가져다가 쓸꺼에요 우리가 직접 만드는 그런 상황은 앞으로 그렇게 많이 나오지 않을꺼에요 직접 만들어야 되는 경우도 있어서.
```



### 바이트기반Stream

- byte기반은 Input,Output Stream을... char기반은 Reader,Writer를 씀
- java.io패키지의 InputStream과 OutputStream이 byte구조의 스트림이다.

InputSteam에는 BufferedInputStream, ByteArrayInputStream, DataInputStream, FilterInputStream, read(), OutputStream, PushbackInputStream 등이 있다.<br>
이중에서 많이 쓰이는 것 위주로 알아보자<br>

※ Api의 SeeAlso 참조

### Byte기반의 InputStream
- InputStream은 키보드의 입력값을 받아 화면에 출력하는 OS의 표준 입력장치와 이미 연결되어 있다는 것만 알고 넘어가자.

#### FileInputStream
위에서 File클래스를 배우면서 FileInputStream했었죠?
거기서 확장된 예제라고 생각하시면 될 듯. 부담없이 확인하세요

#### FileInputEx클래스 정의
```java
실행 결과

전에 만들었던 test.txt파일을 읽어오자요
파일 경로 : d:/test.txt
안녕하세요abcd

public class FileInputEx {
	public static void main(String[] args) {
		FileInputStream fis = null;
		byte read[] = new byte[100];
		byte console[] = new byte[100];

		    try {	
		      System.out.print("파일 경로 : ");	
		      System.in.read(console); //읽어올 파일의 경로를 byte배열로 받는다.	
		      String file = new String(console).trim();//위에서 입력한 경로를 문자열로 변환	
		      
		      //Scanner scan = new Scanner(System.in);	
		      //String file = scan.next();
		      //위의 System.in.read(console)은 Scanner로 값을 입력받는것과 같은 결과지만	
		      //Stream을 배우고 있기 때문에 System.in.read()를 사용했다.

		      fis = new FileInputStream(file);//FileInputStream을 통해 file경로를 받는 객체를 준비
		      fis.read(read, 0, read.length);//read[]배열의 0번째부터 100번째 사이에, 읽어온 txt파일의 내용을 복사
		      System.out.println(new String(read).trim());//read[]를 문자열로 변경하여 출력
		      } catch (Exception e) {
			  e.printStackTrace();
		    } finally {//finally는 try catch구문을 마치고 무조건 실행되는 예약어
			    try {
			      //스트림을 닫아준다.
			      if(fis != null)	fis.close();
			      } catch (IOException e) {
				      // TODO Auto-generated catch block	
				      e.printStackTrace();
			      }
		   	 }
		     }
		}
```

-----------------------------------------------------------------------예제

BufferedInputStream을 알아보자

#### BufferedInputEx클래스 정의

Buffered스트림을 사용하는 이유는 입출력의 효율성을 향상시키기 위해서 이다.<br>
화장실을 예로들자면, 공용화장실 같은 경우는 남녀가 모두 이용하는 공간이기 때문에 자칫 처리가 늦어질 수 있지만,<br>
남자화장실 혹은 여자화장실로 각각의 역할을 구분해두면 쉽게 접근할 수 있는것과 비슷한 이치이다.<br>
위에 작성한 예제와 거의 흡사한 예제이지만, Buffered스트림을 연결해서 어떻게 구현되는지에 대한 예제를 확인해보자.<br>
```java
public class BufferedInputEx {
	public static void main(String[] args) {
		
		//여기 주석은 맨 마지막에 결과 확인 후, 작성해주자
		//FileInputStream과 BufferedInputStream을 연결함으로써
		//파일을 읽을 때 버퍼링 작업을 수행하도록 한다.
		//버퍼링 작업이란, 출력할 바이트를 버퍼라고 하는 메모리 공간에 바이트 배열로 저장하여 한번에 출력하는 것.
		//버퍼라는 공간은 파일을 쓰고 받기위해 마련된 기억장치의 한 부분인데,
		//입출력할 자료를 버퍼에 모아두면, 입출력시에 버퍼라는 전용 공간을 활용하기 때문에 출력속도 향상에 도움이 된다.
		//너무 어려우니, Buffer스트림은 그냥 입출력 향상에 도움이 된다. 라는 정도로만 기억하고 있으면 된다.
		
		FileInputStream fis = null;
		BufferedInputStream bis = null;

		byte _byte[] = new byte[100];
		byte result[] = new byte[100];

		try {
			System.out.print("경로 입력 : ");
			System.in.read(_byte);
			String path = new String(_byte).trim();

			//d:/test.txt를 불러올 예정
			fis = new FileInputStream(path);
			bis = new BufferedInputStream(fis);

			bis.read(result, 0, result.length);
			System.out.print(new String(result).trim());
			
		} catch (Exception e) {
			// TODO: handle exception
		}finally{

			try {
				if(fis != null)
					fis.close();
				if(bis != null)
					bis.close();
			} catch (Exception e2) {
				// TODO: handle exception
			}			
		}
	}
}
```
BufferedOutputStream에 대해 알아봅시다.<br>
앞서 진행한 FileOutputStream과 유사하지만, FileOutputStream을 BufferedOutputStream에 탑재하여 쓰기 작업에 효율성을 높여보자.<br>

#### BufferedOutputEx클래스 정의
```java
public class BufferedOutputEx {
	public static void main(String[] args) {
		FileOutputStream fos = null;
		BufferedOutputStream bos = null;
		
		try {
			fos = new FileOutputStream("d:/bufferedOutput.txt");
			bos = new BufferedOutputStream(fos);
			//BufferedOutputStream에 FileOutputStream을 연결
			
			String str = "BufferedOutputStream 예제";
			bos.write(str.getBytes());
			
			//BufferedOutputStream은 flush()메서드를 호출하지 않으면
			//파일에 데이터를 쓰는것(write)가 불가능하다.
			//write()메서드는 값을 기억하고 있고 이를 물리적으로 파일에 
			//쓰기 위해서 flush()함수를 반드시 사용해야 합니다.
			bos.flush();
			
		} catch (Exception e) {
			// TODO: handle exception
		}finally{
			try {
				if(fos != null)
					fos.close();
				if(bos != null)
					bos.close();
			} catch (Exception e2) {
				// TODO: handle exception
			}
		}
	}
}
```
BufferedReader에 대해 알아봅시다.<br>
FileReader와 결과는 동일하지만 버퍼드리더가 Buffer공간을 할당받아 처리하기 때문에<br>
입출력 속도가 향상된다.<br>

#### BufferedReaderEx클래스 정의
```java
public class BufferedReaderEx {
	public static void main(String[] args) {
		//일단 지금까지 써왔던 test.txt의 내용을 여러줄로 수정 ㅋ
		FileReader fr = null;
		BufferedReader br = null;
		
		try {
			fr = new FileReader("d:/test.txt");
			
			//BufferedReader로 fr이 읽어온 내용을 한줄단위로 처리한다.
			br = new BufferedReader(fr);
			
			String msg;
			
			while((msg = br.readLine()) != null){
				System.out.println(msg);
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				
				if(fr != null)
					fr.close();
				if(br != null)
					br.close();
			} catch (Exception e2) {
				// TODO: handle exception
			}
		}
	}
}
```

BufferedWriter봅시다<br>

#### BufferedWriterEx클래스 정의
```java
public class BufferedWriterEx {
	public static void main(String[] args) {
		FileWriter fw = null;
		BufferedWriter bw = null;
		
		try {
			
			fw = new FileWriter("d:/BufWriter.txt");
			bw = new BufferedWriter(fw);
			bw.write("BufferedWriter테스트");
			
			bw.newLine();//한줄 아래로(\n)내리는 함수 ↓↓
			//bw.write("\r\n"); 이걸 내부 함수를 이용해 구현한것임
			
			bw.write("갑돌이와 갑순이는 한마을에 살았더래요" + 
			System.getProperty("line.separator"));
			//System.getProperty("line.separator"))를 통해
			//프로그램이 라인의 끝이라는 것을 알 수 있도록 해 준다.
			//없어도 무방하지만, 명시하면 파일을 쓰는데 속도면에서 어느정도 효율성이 높아진다.
			
			bw.flush();//Buffer를 이용한 write에는 flush()를 통해 실제로 파일에 내용을 작성해야 한다.

			//bw.close();를 여기서 사용하면 flush()를 사용하지 않아도
			//close()메서드 내부 기능에 의해 파일쓰기가 가능하지만,
			//finally가 아닌 try부분에 close()를 사용하면
			//close()윗라인에서 오류가 발생했을 경우
			//스트림을 닫지 못하고 종료되기 때문에 그냥 flush()를 쓰자.
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			
			try {
				
				if(fw != null)
					fw.close();
				if(bw != null)
					bw.close();
				
			} catch (Exception e2) {
				// TODO: handle exception
			}
		}
	}
}
```

#### DataOutputStream과 DataInputStream을 알아봅시다.
- DataOutputStream은 출력 스트림에 기본자료형을 기록하기 편리하도록 메서드를 제공한다.
- DataInputStream은 받아온 정보에서 기본자료형을 읽어오는 메서드를 제공한다.

문자로 데이터를 저장하면, 다시 데이터를 읽어 올 때 문자들을 실제 값으로 변환하는<br>
예를 들면 문자열 “100”을 숫자 100으로 변환하는, 과정을 거쳐야 하고<br>
또 읽어야 할 데이터의 개수를 결정해야하는 번거로움이 있다.<br>

하지만 이처럼 DataInputStream과 DataOutputStream을 사용하면<br>
데이터를 변환할 필요도 없고, 자리수를 세어서 따지지 않아도 되므로 편리하고 빠르게 데이터를 저장하고 읽을 수 있게 된다.<br>

#### DataOutputEx클래스 정의
```java
public class DataOutputEx {
	public static void main(String[] args) {
		//DataOutput스트림과 DataInput스트림을 동시에 사용하는 예제
		FileInputStream fis = null;
		FileOutputStream fos = null;
		
		DataInputStream dis = null;
		DataOutputStream dos = null;
		
		try {
			fos = new FileOutputStream("d:/dataOutput.txt");
			dos = new DataOutputStream(fos);
			
			dos.writeBoolean(false);
			dos.writeInt(1000);
			dos.writeChar('A');
			dos.writeDouble(10.5);
			
			//여기까지 실행하고, 해당 경로에 파일이 생성되었는지 확인
			//DataOutputStream의 메서드는
			//Boolean, Inteager, Character, Double등 기본자료형을 포함하는 부모객체를 반환하므로, 결과 확인시에 값이 깨져서 나온다는 것을 알고가자.
		

			//txt파일 결과 확인 후 값을 가져오는 부분 추가
			fis = new FileInputStream("d:/dataOutput.txt");
			dis = new DataInputStream(fis);
			System.out.println(dis.readBoolean());
			System.out.println(dis.readInt());
			System.out.println(dis.readChar());
			System.out.println(dis.readDouble());
		} catch (Exception e) {
			// TODO: handle exception
		} finally {
			try {
				if(fis != null)
					fis.close();
				if(fos != null)
					fos.close();
				if(dis != null)
					dis.close();
				if(dos != null)
					dos.close();
			} catch (Exception e2) {
				// TODO: handle exception
			}
		}		
	}
}
```
byte스트림과 char스트림의 연결

#### ByteCharReader클래스 정의
```java
public class ByteCharReader {
	public static void main(String[] args) throws IOException {
		//byte스트림과 char스트림의 연결
		
		File f = new File("d:/java/Test5/src/FileReaderEx1.java");//이전예제 java파일의 경로

		FileInputStream fis = new FileInputStream(f);

		// 위는 byte기반이므로 한줄 단위처리가 안된다.
		// BufferedReader가 필요하다.
		BufferedReader br = new BufferedReader(new InputStreamReader(fis));

		String str;
		while((str = br.readLine()) != null){
			System.out.println(str);
		}

		if(fis != null)
			fis.close();
		if(br != null)
			br.close();
	}
}
```
