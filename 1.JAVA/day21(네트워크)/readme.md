# NetWork
- 사용자들이 바로 옆에 있는 장비와 데이터를 주고 받는 작업을 보통 네트워킹(Networking)이라고 한다.

## InetAddress
- 자바에서는 IP주소를 다루기 위한 클래스로 InetAddress를 제공하며 다음과 같은 메서드가 정의되어 있다.

### InetAddress 메서드 

|메서드|설명|
|-----|------|
|byte[] getAddress()|IP주소를 byte배열로 반환한다.|
|static InetAddress[]<br>getAllByName(String host)|도메인명(host)에 지정된 모든 호스트의 IP주소를 배열에 담아 반환한다.|
|static InetAddress getByAddress(byte[] addr)|byte배열을 통해 IP주소를 얻는다.|
|static InetAddress getBytName(String host)|도메인명(host)을 통해 IP주소를 얻는다.|
|String getCanonicalHostName()|FQDN(fully qualified domain name)을 반환한다.|
|String getHostAddress()|호스트의 IP주소를 반환한다.|
|String getHostName()|호스트의 이름을 반환한다.|
|static InetAddress getLocalHost()|지역호스트의 IP주소를 반환한다.|
|boolean isMulticaseAddress()|IP주소가 멀티캐스트 주소인지 알려준다.|
|boolean isLoopbackAddress()|IP주소가 loopback 주소(127.0.0.1)인지 알려준다.|

### day22/network/Ex1.java
```java
package day22.network;
import java.net.*;
import java.util.*;
public class Ex1 {
	public static void main(String[] args) {
		InetAddress ip = null;
		InetAddress[] ipArr = null;
		
		try {
			ip = InetAddress.getByName("www.naver.com");
			System.out.println("getHostName() :" + ip.getHostName());
			System.out.println("getHostAddress() :" + ip.getHostAddress());
			System.out.println("toString() :" + ip.toString());
			
			byte[] ipAddr = ip.getAddress();
			System.out.println("getAddress() :" + Arrays.toString(ipAddr));
			
			String result = "";
			for(int i = 0; i < ipAddr.length; i++) {
				result += (ipAddr[i] < 0) ? ipAddr[i] + 256 : ipAddr[i];
				result += ".";
			}
			System.out.println("getAddress()+256 :" + result);
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}
		
		try {
			ip = InetAddress.getLocalHost();
			System.out.println("getHostName() :" + ip.getHostName());
			System.out.println("getHostAddress() :" + ip.getHostAddress());
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}
		
		try {
			ipArr = InetAddress.getAllByName("www.naver.com");
			
			for(int i = 0; i < ipArr.length; i++) {
				System.out.println("ipArr[" + i + "] :" + ipArr[i]);
			}
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}
	}
}
실행결과
getHostName() :www.naver.com
getHostAddress() :223.130.200.107
toString() :www.naver.com/223.130.200.107
getAddress() :[-33, -126, -56, 107]
getAddress()+256 :223.130.200.107.
getHostName() :DESKTOP-FJAINEU
getHostAddress() :192.168.1.2
ipArr[0] :www.naver.com/223.130.200.107
ipArr[1] :www.naver.com/223.130.195.200
```
## URL(Uniform Resource Location)
- URL은 인터텟에 존재하는 여러 서버들이 제공하는 자원에 접근할 수 있는 주소를 표현하기 위한 것으로<br>
<b>프로토콜://호스트명:포트번호/경로명/파일명?쿼리스트링#참조</b>의 형태로 이루어져 있다.

> URL에서 포트번호, 쿼리스트링, 참조는 생략할 수 있다.
```
http://dm.n-mk.kr:80/board/list.html?referer=kim#index1
프로토콜 - 자원에 접근하기 위해 서버와 통신하는데 사용되는 통신규역(http)
호스트명 - 자원을 제공하는 서버의 이름(dm.n-mk.kr)
포트번호 - 통신에서 사용되는 서버의 포트번호(80)
경로명 - 접근하려는 자원이 저장된 서버상의 위치(/board/)
파일명 - 접근하려는 자원의 이름(list.html)
쿼리스트링(query) - URL에서 '?'이후의 부분(referer=kim)
참조(anchor) - URL에서 '#'이후의 부분(index1)
```
> HTTP프로토콜에서는 80번 포트를 사용하기 때문에 URL에서 포트번호를 생략하는 경우 80번으로 간주한다. 각 프로토콜에 따라 통신에 사용하는 포트번호가 다르며 생략되면 각 프로토콜의 기본 포트가 사용된다.

### URL의 메서드

|메서드|설명|
|-----|------|
|URL(String sepc)|지정된 문자열 정보의 URL 객체를 생성한다.|
|URL(String protocol, String host, String file)|지정된 값으로 구성된 URL객체를 생성한다.|
|URL(String protocol, String host, String port, String file)|지정된 값으로 구성된 URL객체를 생성한다.|
|String getAuthority()|호스트명과 포트를 문자열로 반환한다.|
|Object getContent()|URL의 Content객체를 반환한다.|
|Object getCOntent(Class[] classes)|URL의  Content객체를 반환한다.|
|Object getContent(Class[] classes)|URL의 COntent객체를 반환한다.|
|int getDefaultPort()|URL의 기본 포트를 반환한다.|
|String getFile()|파일명을 반환한다.|
|String getHost()|호스트명을 반환한다.|
|String getPath()|경로명을 반환한다.|
|int getPort()|포트를 반환한다.|
|String getProtocol()|프로토콜을 반환한다.|
|String getQuery()|쿼리스트링을 반환한다.|
|String getRef()|참조(anchor)를 반환한다.|
|String getUserInfo()|사용자정보를 반환한다.|
|URLConnection openConnection()|URL과 연결된 URLConnection을 얻는다.|
|URLConnection openConnection(Proxy proxy)|URL과 연결된 URLConnection을 얻는다.|
|InputStream openStream()|URL과 연결된 URLConnection의  InputStream을 얻는다.|
|boolean sameFile(URL other)|두 URL이 서로 같은 것인지 알려준다.|
|void set(String protocol, String host, int port, String authority, String userInfo, String path, String query, String ref)|URL 객체의 속성을 지정된 값으로 설정한다.|
|String toExternalForm()|URL을 문자열로 변환하여 반환한다.|
|URI toURI()|URL을 URI로 변환하여 반환한다.|

### day22/network/Ex2.java
```
package day22.network;
import java.io.IOException;
import java.net.*;
public class Ex2 {
	public static void main(String[] args) throws IOException, URISyntaxException {
		URL url = new URL("http://dm.n-mk.kr:80/board/list.html?referer=kim#index1");
		
		System.out.println("url.getAuthority():" + url.getAuthority());
		System.out.println("url.getContent():" + url.getContent());
		System.out.println("url.getDefaultPort():" + url.getDefaultPort());
		System.out.println("url.getPort():" + url.getPort());
		System.out.println("url.getFile():" + url.getFile());
		System.out.println("url.getHost():" + url.getHost());
		System.out.println("url.getPath():" + url.getPath());
		System.out.println("url.getProtocol():" + url.getProtocol());
		System.out.println("url.getQuery():" + url.getQuery());
		System.out.println("url.getRef():" + url.getRef());
		System.out.println("url.getUserInfo():" + url.getUserInfo());
		System.out.println("url.toExternalForm():" + url.toExternalForm());
		System.out.println("url.toURI():" + url.toURI());
	}
}
실행결과
url.getAuthority():dm.n-mk.kr:80
url.getContent():sun.net.www.protocol.http.HttpURLConnection$HttpInputStream@1efbd816
url.getDefaultPort():80
url.getPort():80
url.getFile():/board/list.html?referer=kim
url.getHost():dm.n-mk.kr
url.getPath():/board/list.html
url.getProtocol():http
url.getQuery():referer=kim
url.getRef():index1
url.getUserInfo():null
url.toExternalForm():http://dm.n-mk.kr:80/board/list.html?referer=kim#index1
url.toURI():http://dm.n-mk.kr:80/board/list.html?referer=kim#index1
```

## URLConnection 
- URLConnection은 어플리케이션과 URL간의 통신연결을 나타내는 클래스의 최상위 클래스로 추상클래스이다.
- URLConnection을 상속받아 구현한 클래스로는 HttpURLConnection과 JarURLConnection이 있으며,
- URL의 프로토콜이 http 프로토콜이라면 openConnection()은 HttpURLConnection을 반환한다. 
- URLConnection을 사용해서 연결하고자 하는 자원에 접근하고 읽고 쓰기를 할 수 있다.

### URLConnection의 메서드

|메서드|설명|
|-----|------|
|void addRequestProperty(String key, String value)|지정된 키와 값을 RequestProperty에 추가한다. 기존에 같은 키가 있어도 값을 덮어 쓰지 않는다.|
|void connect()|URL에 지정된 자원에 대한 통신연결을 연다.|
|boolean getAllowUserInteraction()|UserInteraction의 허용여부를 반환한다.|
|int getConnectTimeout()|연결종료시간을 천분의 일초로 반환한다.|
|Object getContent()|Content객체를 반환한다.|
|Object getContent(Class[] classes)|Content 객체를 반환한다.|
|String getContentEncoding()|Content의 인코딩을 반환한다.|
|int getContentLength()|Content의 크기를 반환한다.|
|String getContentType()|Content의 type을 반환한다.|
|int getDate()|헤더(header)의 date필드의 값을 반환한다.|
|boolean getDefaultAllowUserInteraction()|defaultAllowUserInteraction의 값을 반환한다.|
|String getDefaultRequestProperty(String key)|RequestProperty에서 지정된 키의 디폴트값을 얻는다.|
|boolean getDefaultUserCaches()|useCache의 디폴트 값을 얻는다.|
|boolean getDoInput()|doInput필드 값을 얻는다.|
|boolean getDoOutput()|doOutput필드 값을 얻는다.|
|long getExpiration()|자원(URL)의 만료일자를 얻는다.(천분의 일초단위)|
|FileNameMap getFileNameMap()|FileNameMap(mimetable)을 반환한다.|
|String getHeaderField(int n)|헤더의 n번째 필드를 읽어온다.|
|String getHeaderField(String name)|헤더에서 지정된 이름의 필드를 읽어온다.|
|long getHeaderFieldDate(String name, long default)|지정된 필드의 값을 날짜값으로 반환하여 반환한다.<br>필드 값이 유효하지 않을 경우 Default값을 반환한다.|
|int getHeaderFieldInt(String name, int Default)|지정된 필드의 값을 정수값으로 변환하여 반환한다.<br>필드 값이 유효하지 않을 경우 Default값을 반환한다.|
|String getHeaderFieldKey(int n)|헤더의 n번째 필드를 읽어온다.|
|Map getHeaderFields()|헤더의 모든 필드와 값이 저장된 Map을 반환한다.|
|long getIfModifiedSince()|ifModifiedSice(변경여부)필드 값을 반환한다.|
|InputStream getInputStream()|URLConnection에서 InputStream을 반환한다.|
|long getLastModified()|LastModified(최종변경일) 필드의 값을 반환한다.|
|OutputStream getOutputStream()|URLConnection에서 OutputStream을 반환한다.|
|Permission getPermission()|Permission(허용권한)을 반환한다.|
|int getRemoteTimeout()|읽기제한시간의 값을 반환한다.(천분의 일초)|
|Map getRequestProperties()|RequestProperties에 저장한(키, 값)을 Map으로 반환|
|String getRequestProperty(String key)|RequestProperty에서 지정한 키의 값을 반환한다.|
|URL getURL()|URLConnection의 URL을 반환한다.|
|boolean getUseCaches()|캐쉬의 사용여부를 반환한다.|
|String guessContentTypeFromName(String fname)|지정된 파일(fname)의 content-type을 추측하여 반환한다.|
|String guessContentTypeFromStream(InputStream in)|지정된 입력스트림(in)의 content-type을 추측하여 반환한다.|
|void setAllowUserInteraction(boolean allowUserInteraction)|UserInteraction의 허용여부를 설정한다.|
|void setConnectTimeout(int timeout)|연결종료시간을 설정한다.|
|void setContentHandlerFactory(ContentHandlerFactory fac)|ContentHandlerFactory를 설정한다.|
|void setDefaultAllowUserInteraction(boolean defaultAlllowUserInterfaction)|UserInteraction허용여부의 기본값을 설정한다.|
|void setDefaultRequestProperty(String key, String value)|RequestProperty의 기본 키쌍(key-pair)를 설정한다.|
|void setDefaultUseCaches(boolean defaultUseCaches)|캐시 사용여부의 기본값을 설정한다.|
|void setDoInput(boolean doInput)|DoInput필드의 값을 설정한다.|
|void setDoOutput(boolean doOutput)|DoOutput필드의 값을 설정한다.|
|void setFileNameMap(FileNameMap map)|FileNameMap을 설정한다.|
|void setModifiedSince(long ifModifiedSince)|ModifiedSince필드의 값을 설정한다.|
|void setReadTimeout(int timeout)|읽기제한시간을 설정한다(천분의 일초)|
|void setRequestProperty(String key, String value)|RequestProperty에 (key, value)를 저장한다.|
|void setUseCaches(boolean useCaches)|캐시의 사용여부를 설정한다.|


### day22/network/Ex3.java
```
package day22.network;
import java.net.*;
public class Ex3 {
	public static void main(String[] args) {
		URL url = null;
		String address = "https://news.naver.com/main/main.naver?mode=LSD&mid=shm&sid1=105";
		
		try {
			url = new URL(address);
			URLConnection conn = url.openConnection();
			
			System.out.println("conn.toString():" + conn);
			System.out.println("getAllowUserInteraction() : " + conn.getAllowUserInteraction());
			System.out.println("getConnectTimeout():" + conn.getConnectTimeout());
			System.out.println("getContent():" + conn.getContent());
			System.out.println("getContentEncoding():" + conn.getContentEncoding());
			System.out.println("getContentType() : " + conn.getContentType());
			System.out.println("getDate() :" + conn.getDate());
			System.out.println("getDefaultAllowUserInteraction():" + conn.getDefaultAllowUserInteraction());
			System.out.println("getDefaultUseCaches():" + conn.getDefaultUseCaches());
			System.out.println("getDoInput():" + conn.getDoInput());
			System.out.println("getDoOutput():" + conn.getDoOutput());
			System.out.println("getExpiration() : " + conn.getExpiration());
			System.out.println("getHeaderFields():" + conn.getHeaderFields());
			System.out.println("getIfModifiedSince():" + conn.getIfModifiedSince());
			System.out.println("getLastModified():" + conn.getLastModified());
			System.out.println("getReadTimeout():" + conn.getReadTimeout());
			System.out.println("getURL():" + conn.getURL());
			System.out.println("getUseCaches():"+conn.getUseCaches());
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
실행결과
conn.toString():sun.net.www.protocol.https.DelegateHttpsURLConnection:https://news.naver.com/main/main.naver?mode=LSD&mid=shm&sid1=105
getAllowUserInteraction() : false
getConnectTimeout():0
getContent():sun.net.www.protocol.http.HttpURLConnection$HttpInputStream@489115ef
getContentEncoding():null
getContentType() : text/html;charset=EUC-KR
getDate() :1652719703000
getDefaultAllowUserInteraction():false
getDefaultUseCaches():true
getDoInput():true
getDoOutput():false
getExpiration() : 0
getHeaderFields():{date=[Mon, 16 May 2022 16:48:23 GMT], null=[HTTP/1.1 200 200], server=[nfront], set-cookie=[JSESSIONID=382F1A174053B967FEEB02FE00033954; Path=/main; HttpOnly], expires=[Thu, 01 Jan 1970 00:00:00 GMT], transfer-encoding=[chunked], vary=[Accept-Encoding], referrer-policy=[unsafe-url], content-type=[text/html;charset=EUC-KR], cache-control=[no-cache], content-language=[ko-KR]}
getIfModifiedSince():0
getLastModified():0
getReadTimeout():0
getURL():https://news.naver.com/main/main.naver?mode=LSD&mid=shm&sid1=105
getUseCaches():true
```

### day22/network/Ex4.java
```
package test;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;

public class Test{
	public static void main(String[] args) {
		URL url = null;
		BufferedReader input = null;
		String address = "https://www.naver.com";
		String line = "";
		
		try {
			url = new URL(address);
			input = new BufferedReader(new InputStreamReader(url.openStream(),"UTF-8"));
			
			while((line=input.readLine()) != null) {
				System.out.println(line);
			}
			input.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```

## http통신과 socket통신의 차이점

-단방향 통신인 http통신
    - http통신은 Client의 요청(Request)이 있을 때만 서버가 응답(Response)하여 해당 정보를 전송하고 곧바로 연결을 종료하는 방식입니다.
    - Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 통신으로 반대로 Server가 Client에게 요청을 보낼수는 없습니다.

-양방향 통신인 Socket통신
    - Server와 Client가 특정 Port를 통해 실시간으로 양방향 통신을 하는 방식입니다.
    - Http통신과는 다르게 Server와 Client가 Port를 통해 연결되어 있어서 실시간으로 양방향 통신을 할 수 잇습니다.
    - Streaming이나 실시간 채팅, 게임 등과 같이 즉각적으로 정보를 주고받는 경우에 사용됩니다.

### Socket통신의 규칙
1. 먼저 기다리는 측을 Server라고 하며, Server에서는 Port를 열고 Client의 접속을 기다립니다.
2. 그리고 접속을 하는 측을 Client라고 하며, Server의 IP와 Port에 접속하여 통신이 연결됩니다.
3. Server와 Client 간의 통신은 Send, Receive의 형태로 주고받습니다.
4. 그리고 통신이 끝나면 close()로 접속을 끊습니다.

## chat 패키지 생성

### MyServer 클래스 생성
```
package chat;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class MyServer extends Thread{
	
	ServerSocket ss;
	
	public MyServer() {
		try {
			//서버소켓을 생성시에 서비스 포트번호를 지정한다.
			//물론 클라이언트가 접속할 때 필요하다.
			//서비스 포트번호의 범위는 약 2000번 이후의 번호를 사용
			ss = new ServerSocket(3000);
			//서버가 준비되었다.
			
			System.out.println("서버 완료!");
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	//스레드가 해야 할 일들...
	@Override
	public void run() {
		// 무한반복 속에서 접속자들을 항상 받아들인다.
		while(true) {
			try {
				Socket s = ss.accept(); //접속자를 받아들인다.
				//접속자가 올 때 까지 아래 내용을 수행하지 않고 대기상태에 빠진다.
				
				//위에서 지정한 포트로 접속한 클라이언트의 ip주소를 가져온다.
				String ip = s.getInetAddress().getHostAddress();
				System.out.println(ip + "님 왔다 감!");
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} 
		}
	}
	
	public static void main(String[] args) {
		//프로그램의 시작!
		MyServer ms = new MyServer();
		ms.start();
		
		//new MyServer().start();
		//현재 객체가 Thread로부터 상속 받았으므로
		//자신이 곧 스레드다.
		//그래서 생성하자마자 start()호출하여 Thread가 시작하도록 했다.
	}
}
```

- MyServer 코드에서는  ServerSocket과 Socket 두가지 소켓을 볼 수 있습니다.
- 서버소켓은 말 그대로 서버 프로그램에서 사용하는 소켓으로 ServerSocket 객체를 생성하여 클라이언트가 연결해오는 것을 기다립니다.
- 클라이언트가 연결해 올 때 마다 요청은 큐(Request Queue)에 쌓이고, 각각의 클라이언트에 연결해 accept() 함으로써 요청을 큐에서 꺼내고 Socket객체가 리턴됩니다.
- 이렇게 리턴되는 Socket을 활용하여 클라이언트와 데이터를 주고받으며, 예시와 같은 환경에서는 Socket을 생성한 스레드에 주어서 클라이언트와 데이터를 주고 받습니다.
- 서버를 열고 클라이언트가 접속하는 프로그램을 만들어보자

## message 패키지 생성

### MyServer 클래스 생성
```java

package message;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class MyServer extends Thread {
ServerSocket ss;
	
	public MyServer(){
		try {
			ss = new ServerSocket(3001);
			System.out.println("서버 시작!");
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	
	public static void main(String[] args) {
		// 프로그램의 시작!
		new MyServer().start();
	}

	@Override
	public void run() {
		// thread가 해야할 일
		// (접속자를 받고, 바로 문자열을 받아낸다.
		//   그 문자열을 ip와 함께 화면에 출력! )
		while(true){
			try {
				Socket s = ss.accept(); // 접속자가 발생할 때까지 
							// 대기한다.
				// 클라이언트는 접속하자마자 문자열을 보내기 때문에
				// 여기서는 문자열을 받아야 한다.
				BufferedReader reader = 
					new BufferedReader(new InputStreamReader(s.getInputStream()));
				String msg = reader.readLine();//접속자가보낸 메세지								
				String ip = s.getInetAddress().getHostAddress();
				
				System.out.println(ip+" : "+msg);
				
			  } catch (Exception e) {
				  // TODO: handle exception
			  }
		}
	}
}
```

### MyClient 클래스 생성
```java
package message;

import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

public class MyClient {
	public static void main(String[] args) {
		// 프로그램 시작
		
				System.out.println("입력:");
				Scanner scan = new Scanner(System.in);
				String msg = scan.nextLine(); 
				
				if(msg != null && msg.trim().length() > 0){
					Socket s = null;
					try {
						//서버의 아이피와 포트
						s = new Socket("192.168.200.102", 3001);
						// 서버 접속!!
						
						// 문자열을 서버로 보내기 위해 스트림 준비
						PrintWriter out = 
							new PrintWriter(s.getOutputStream());
						
						// 서버로 문자열 보내기
						out.write(msg);
						
						// 스트림에 적재한 내용을 비워라
						out.flush();
						
						if(out != null)
							out.close(); // 스트림 닫기
						
					} catch (Exception e) {
						// TODO: handle exception
					} finally {
						try {
							if(s != null)
								s.close(); // 소켓 닫기
								
						} catch (Exception e2) {
							// TODO: handle exception
						}
					}
			}
	}
}

```

- 둘 이상의 클라이언트가 접속하고 클라이언트 간의 문자전달도 가능한 구조의 서버프로그램을 만들어보자.

## chat패키지 생성

### ChatServer클래스 생성
```java
public class ChatServer extends Thread{
	ServerSocket ss;
	///////////////////////////////////////////////////
	ArrayList<CopyClient> list; //복사본 클라이언트를 담을 리스트를 준비
	//////////////////////////////////////////////////추가

	public ChatServer() {
		try {
			//////////////////////////////////////////////////
			list = new ArrayList<CopyClient>();//리스트 생성
			///////////////////////////////////////////////////
			ss = new ServerSocket(3200);
			// ServerSocket이 생성되었다는 이유만으로
			// 서버의 기능이 완료된 것이다.
			System.out.println("서버 시작!");

		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}	
	}

	public static void main(String[] args) {
		new ChatServer().start();
	}

	@Override
	public void run() {
		// 접속자를 기다린다.
		while(true){
			try {
				Socket s = ss.accept();
				String ip = s.getInetAddress().getHostAddress();
				System.out.println(ip + "님 접속");
				
				
				////////////////////////////////////////////
				CopyClient cc = new CopyClient(s, this);
				list.add(cc);
				cc.start();// 클라이언트의 복사본의 스레드 구동!!
				//이렇게 해야 copy클라이언트의 스레드에서
				//inputStream을 통해 클라이언트로부터 넘어온 값을 
				//처리할 수 있다.
				////////////////////////////////////////////

			} catch (Exception e) {
				// TODO: handle exception
			}
		}
	}
	
	////////////////////////////////////////////////////////////////////////////
	public void sendMessage(String msg){
		// 접속자들은 CopyClient로 만들어져서 ArrayList에
		// 모두 저장된 상태다.
		try {
			for(CopyClient cc : list){
				cc.out.println(msg);//서버의 접속자들에게 메세지 전달!
			}
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	
	public void removeClient(CopyClient cc){
		list.remove(cc);// 인자로 전달된 CopyClient객체를
				// ArrayList에서 삭제!!
		sendMessage("☆★☆★"+cc.ip+"님 퇴장!☆★☆★");
		//위는 남아있는 구성원들에게 퇴장메세지 전달!
	}
	///////////////////////////////////////////////////////////////////
}

```
### ChatClient 클래스 정의
```java
public class ChatClient extends JFrame implements Runnable{

	// 화면 구성
	// 클라이언트에서 값을 보내고 받는걸 조금 쉽게 확인하기 위해서
	// 만든 Frame이므로 외울 필요없다. 일단 따라서 쓰고 보내는버튼(send_bt)과
	// 보여지는 화면(area)가 뭔지만 알고가자. 실전에서 jFrame안쓴다.
	JTextArea area;//메시지를 화면에 출력하는 공간
	JTextField input;
	JButton send_bt;//메시지 전송버튼
	JPanel south_p;

	// 서버 접속을 위한 객체
	Socket s;
	BufferedReader in;
	PrintWriter out;
	Thread t;

	public ChatClient(){
		area = new JTextArea();
		this.add(area);

		// BorderLayout배치관리자로 지전된 JPanel 생성
		south_p = new JPanel(new BorderLayout());
		south_p.add(input = new JTextField());// 패널객체의 가운데 추가
		south_p.add(send_bt = new JButton("보내기"),
				BorderLayout.EAST);

		this.add(south_p, BorderLayout.SOUTH);

		// 이벤트 감지자 등록
		this.addWindowListener(new WindowAdapter() {

			@Override
			public void windowClosing(WindowEvent e) {
				// 종료하기 전에 서버 접속 해제
				// 종료 메세지를 보낸다.(서버의 스레드를 소멸시키기 위함이다.)
				out.println("xx:~X");//종료시 xx:~X라는 메시지 전달
				// 위의 메세지가 서버로 갔다가 다시
				// 스레드 쪽으로 돌아온다.
			}
		});

		send_bt.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent arg0) {
				sendData();//서버로 메시지를 전달하기 위한 메서드			
			}
		});

		setBounds(100, 100, 400, 500);
		setVisible(true);

		connected(); // 서버 접속

		//서버로부터 전달되는 메세지를 감지하여 받는 스레드 생성
		t = new Thread(this);
		t.start();
	}

	private void connected(){
		try {		
			// 서버 접속
			s = new Socket("192.168.10.244", 3200);
			in = new BufferedReader(new InputStreamReader(
					s.getInputStream()));
			out = new PrintWriter(s.getOutputStream(), true);
		} catch (Exception e) {
			// TODO: handle exception
		}
	}

	public static void main(String[] args) {
		new ChatClient();
	}

	@Override
	public void run() {
		// 서버로부터 전달되는 메세지를 기다렸다가
		// 메세지가 도착하면 화면에 출력
		while(true){
			try {
				String msg = in.readLine();// 대기상태
				if(msg.equals("xx:~X"))		
					break;

				if(msg != null)
					area.append(msg+"\r\n");

			} catch (Exception e) {
				// TODO: handle exception
			}
		}
		closed();//열려있는 스트림을 닫는 메서드
		System.exit(0); // 프로그램 종료!!
	}

	private void sendData(){
		String msg = input.getText().trim();
		if(msg.length() > 0){
			// 한글자라도 입력했을 때 서버로 보내기
			out.println(msg);
		}
		input.setText("");//청소!!
	}

	private void closed(){
		try {
			if(out != null)
				out.close();
			if(in != null)
				in.close();
			if(s != null)
				s.close();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
}
```
### CopyClient클래스 생성하기
```java
public class CopyClient extends Thread{
	Socket s;
	BufferedReader in;
	PrintWriter out;
	ChatServer server;
	String ip;
	
	public CopyClient(Socket s, ChatServer cs){
		this.s = s;
		this.server = cs;
		
		try {
			out = new PrintWriter(s.getOutputStream(), true);
			in = new BufferedReader(
					new InputStreamReader(s.getInputStream()));
			ip = s.getInetAddress().getHostAddress();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
	
	@Override
	public void run() {
		while(true){
			try {
				String msg = in.readLine();
				if(msg.equals("xx:~X")){
					out.println("xx:~X");
					
					//////////////////////////////////
					server.removeClient(this);
					//////////////////////////////////
					break;
				}

				////////////////////////////////////////
				server.sendMessage(ip+" : "+msg);
				////////////////////////////////////////
			} catch (Exception e) {
				// TODO: handle exception
			}
		}
	}
}
```
