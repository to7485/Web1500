# sharedPreferences
- 안드로이드 앱 개발을 진행하다 보면, 앱의 데이터들을 저장하여 관리해야 할 상황이 존재한다.
- 간단한 설정 값이나 문자열 같은 데이터를 저장하기 위해 DB를 사용하기는 부담스럽기 때문에 SharedPreferences를 사용하는 것이 적합하다.

## sharedPreferences의 특징
- 보통 초기 설정값이나 자동 로그인 여부 등 간단한 값을 저장하기 위해 사용
- Application에 파일 형태로 데이터를 저장한다.
- Application이 삭제되기 전까지 저장한 데이터가 보존된다.
- Key-value 방식

### 객체 생성법
```java
SharedPreferences pref = getSharedPreferences("SHARE", MODE_PRIVATE);
```
### MODE의 종류
- MODE_PRIVATE : 생성한 Application에서만 사용 가능하다.
- MODE_WORLD_READABLE : 외부 App에서 사용 가능, But 읽기만 가능
- MODE_WORLD_WRITEABLE : 외부 App에서 사용 가능, 읽기/쓰기 가능

## Ex_오늘 날짜 프로젝트 생성하기

## SharedPreferencesActivity로 변경하고 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".SharedPreferencesActivity">

    <TextView
        android:id="@+id/txt_value"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:gravity="center"
        android:text="0"
        android:textSize="50dp" />

    <Button
        android:id="@+id/btn_plus"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="plus" />

    <Button
        android:id="@+id/btn_minus"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="minus" />

    <Button
        android:id="@+id/btn_reset"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="reset" />
</LinearLayout>
```
- 버튼을 눌렀을 때 1씩 증가하거나 감소 그리고 리셋버튼을 눌렀을 때는 0으로 돌아오게 만들어보자

## SharedPreferencesActivity에 코드 작성하기
- 실습으로 해보기
```java
package com.korea.ex_0716;

import androidx.appcompat.app.AppCompatActivity;

import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class SharedPreferencesActivity extends AppCompatActivity {
    //간단한 정보를 저장해야 하는 경우 DB를 쓰기에 오히려 손해인 경우가 있다.
    //내부적으로 파일형태로 기록되는 저장방식을 통해 휴대폰이 종료되거나
    // 재시작되어도 앱을 삭제하기 전까지는 남아있는 데이터를 만들 수 있다.

    SharedPreferences pref;

    TextView txt_value;
    Button btn_plus, btn_minus, btn_reset;
    int n = 0; //txt_value의 내용을 갱신하기 위한 변수

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_shared_preferences);

        btn_plus = findViewById(R.id.btn_plus);
        btn_minus = findViewById(R.id.btn_minus);
        btn_reset = findViewById(R.id.btn_reset);

        txt_value = findViewById(R.id.txt_value);



        btn_plus.setOnClickListener(click);
        btn_minus.setOnClickListener(click);
        btn_reset.setOnClickListener(click);
    }//onCreate

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            int id = view.getId();

            if(id == R.id.btn_plus){
                n++;
                txt_value.setText(""+n);
            } else if(id == R.id.btn_minus){
                n--;
                txt_value.setText(String.valueOf(n));
            } else if(id == R.id.btn_reset){
                n = 0;
                txt_value.setText(String.valueOf(n));
            }
        }
    };
}
```
- 실행을 해보면 증가시키거나 감소시키거나 잘 작동하는걸 볼 수 있다.
- 앱을 껏다가 켰을 때도 이전에 했던 값이 저장이 되게 만들것이다.
- 어느 시점에 저장을 할 것이냐가 중요하다.
- 값을 증가시키거나 감소시키거나 초기화 하거나 버튼을 누를때마다 일일이 저장하는 방법
- 뒤로가기를 누를때나 홈으로 가거나 메뉴로 갈 때 퍼즈로 갈텐데 퍼즈에서 저장을 할 수도 있다.
- 홈이나 메뉴창을 열면 퍼즈가 호출되니까 경제적으로 작성할 수 있을 것 같다.

## SharedPreferencesActivity에 코드 작성하기
```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_shared_preferences);

///////////////////////////////////////////////////////////////////////////
        pref = getSharedPreferences("SHARE",MODE_PRIVATE);
///////////////////////////////////////////////////////////////////////////

        btn_plus = findViewById(R.id.btn_plus);
        btn_minus = findViewById(R.id.btn_minus);
        btn_reset = findViewById(R.id.btn_reset);

        txt_value = findViewById(R.id.txt_value);



        btn_plus.setOnClickListener(click);
        btn_minus.setOnClickListener(click);
        btn_reset.setOnClickListener(click);
    }//onCreate
```
- pref 객체를 하나 만들어주자.
- onPause() 함수가 호출됐을때 저장이 되게 만들자.

```java
   @Override
    protected void onPause() {
        super.onPause();

        SharedPreferences.Editor edit = pref.edit();

        //edit.put을 하면 저장할수 있는 목록이 나오는데 기본자료형 정도 저장할 수 있다.
        edit.putInt("save",n);
        edit.commit(); //물리적으로 세이브데이터를 저장
        //애플리케이션 레지스트리 영역에 저장된다고 한다. 파일 탐색기로는 찾을 수 없다.
        //앱이 삭제될때 같이 삭제되고 다시 설치하면 값이 저장되어있지 않고 처음부터 다시 시작한다.
    }
```

- n값을 저장하지만 앱을 껏다 켜면 값을 불러오지는 않는다.
- 다시켰을 때는 onCreate가 호출되니 onCreate에서 저장된 값을 불러오자.

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_shared_preferences);

        pref = getSharedPreferences("SHARE",MODE_PRIVATE);
///////////////////////////////////////////////////////////////////////////

        //앱을 실행했을 때save라는 이름으로 저장해둔 n값을 읽어온다.
        //최초에 다운을 받았을 때 save라는 key값으로 저장된 값이 없다.
        //n = pref.getInt("save");

        //save라고 하는 키값에 있는 값을 찾지 못할 때 기본값을 n에 넣어라.
        n = pref.getInt("save",0);

///////////////////////////////////////////////////////////////////////////

        btn_plus = findViewById(R.id.btn_plus);
        btn_minus = findViewById(R.id.btn_minus);
        btn_reset = findViewById(R.id.btn_reset);

        txt_value = findViewById(R.id.txt_value);

///////////////////////////////////////////////////////////////////////////
        txt_value.setText(""+n);
///////////////////////////////////////////////////////////////////////////

        btn_plus.setOnClickListener(click);
        btn_minus.setOnClickListener(click);
        btn_reset.setOnClickListener(click);

```
- SharedPreferences 같은 경우에는 간단한 데이터를 저장하는 용도로 많이 사용한다.
- 이 Preference 객체를 만약에 다른 액티비티에 같은 이름으로 생성만 되어 있다면 다른 액티비티에서도 pref.getInt()를 통해서 언제든지 값을 가져올 수 있다.

# Inflater
- inflator 라고 하는걸 메뉴 만들 때 메뉴인플레이터만 본적이 있다.
- xml파일을 view 형태로 보여줄 때 사용하는 함수 형태로 사용을 해봤다.
- 앞으로 서버랑 통신을 하면서 눈에 보이지 않는 참조파일을 view로 만들어서 화면에 보여줘야 되는 경우가 있다.

## InflaterActivity 생성하고 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".InflaterActivity"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="text1"
        android:textSize="30dp" />

레이아웃을 만들다보면 지나치게 길어진다거나 상황에 따라서 수정을 해야 할 경우가 있다.
textview 사이에다가 리니어 레이아웃을 하나 만들고 id를 레이아웃으로 주자.
    <LinearLayout
        android:id="@+id/layout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical" />

이 레이아웃에는 우리가 별도로 만든 액티비티와 세트가 아닌 다른 ooo.xml 참조파일을 만들어서
디자인을 한다음에 리니어레이아웃 안에다가 넣을 수 있다. 리스트뷰를 하기 전에 반드시 알아야 할 기술이다.


    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="text2"
        android:textSize="30dp" />

</LinearLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/35287268-49d9-4256-8dc4-613a3e88a8b8)

## InflaterActivity 코드 작성하기
```java
package com.korea.ex_0716;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.LinearLayout;

public class InflaterActivity extends AppCompatActivity {

    LinearLayout layout;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_inflater);

        layout = findViewById(R.id.layout);
        //아무것도 갖고있지 않은 비어있는 레이아웃
        //여기에 액티비티와 쌍을 이루고 있지 않은 레이아웃만 별도로 하나 만들것이다.
        //레이아웃은 별도로 만들어 봤자 누군가 참조해주지 않으면 어차피 눈에 보이지 않는다. 
        //그래서 짝궁이 없는 레이아웃을 만든다는거 자체가 좀 생소한 작업일 수 있다.
    }
}

```

## layout파일만들기
-layout -> new -> Layout -> Layout Resource File

![image](https://github.com/to7485/Web1500/assets/54658614/c6ca6c73-91b0-4bd5-ae06-01a2ddcf922c)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!--만들어진 요소만큼 view가 만들어진다 요소가 없는 부분은 알아서 잘라냄-->

    <Button
        android:id="@+id/btn1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="btn1" />

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:src="@mipmap/ic_launcher_round" />

</LinearLayout>
```

- 현재 우리가 만든 my_inflater.xml 파일은 R.java에만 등록되어 있고 눈으로는 확인이 불가능하다.
- activity_inflater.xml에 만들어 놓은 textview 사이에 리니어레이아웃에 우리가 만든 my_inflater.xml을 넣어보자.

## InflaterActivity 코드 추가하기

```java
package com.korea.ex_0716;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.LinearLayout;

public class InflaterActivity extends AppCompatActivity {

    LinearLayout layout;

    //xml문서를 view구조로 캐스팅해주는 객체
    LayoutInflater inflater;

    View inner;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_inflater);

        //안드로이드에서 xml을 view로 만들어주기 위해서 가지고 있는 상수값을 통해서
        // 앞으로 나한테 주어진 xml문서는 inflater 기능을 통해서 눈으로 확인 가능한 레이아웃이 될겁니다.
        // 라고 준비된 상태 입니다.
        inflater = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);
        inner = inflater.inflate(R.layout.my_inflater,null);


        layout = findViewById(R.id.layout);
        //아무것도 갖고있지 않은 비어있는 레이아웃
        //여기에 액티비티와 쌍을 이루고 있지 않은 레이아웃만 별도로 하나 만들것이다.
        //레이아웃은 별도로 만들어 봤자 누군가 참조해주지 않으면 어차피 눈에 보이지 않는다.
        //그래서 짝궁이 없는 레이아웃을 만든다는거 자체가 좀 생소한 작업일 수 있다.

        //LinearLayout안쪽에 inner라고 생성해둔 my_inflater추가
        layout.addView(inner);
    }
}

```
- 짝꿍이 없던 레이아웃이 view로 만들어서 inner로 들어가서 layout에 추가함

![image](https://github.com/to7485/Web1500/assets/54658614/edb6bbc9-7ab9-4410-b208-a174d14164fb)

- inner = inflater.inflate(R.layout.my_inflater,null); null 자리에 레이아웃을 다이렉트로 넣어주면<br>layout.addView(inner);를 생략해도 된다.

```java
   @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_inflater);


        inflater = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);
        inner = inflater.inflate(R.layout.my_inflater,layout);
        
        layout = findViewById(R.id.layout);

        //layout.addView(inner);
    }
```

- my_infalter에 있는 버튼을 추가해서 이벤트 처리를 할 수 있다
- 토스트를 출력해보자.

```java
Button btn1;


btn1 = findViewById(R.id.btn1);
버튼을 검색하고 사용하려고 하면 사용이 안된다.
```
- 그런데 activity_inflater에 있던게 아니기 때문에 오류가 난다.

```java
inner = inflater.inflate(R.layout.my_inflater,layout);
btn1 = inner.findViewById(R.id.btn1);
```
- 버튼이 inner라고 하는 객체 안에 있기 때문에 inner 밑에다가 검색을 해줘야 한다!
- 누구한테 가져온건지 확실하게 명시를 해줘야 한다.
- 버쪽 통신을 보면서 사용할 기술중에서 정말 핵심적으로 사용될 기술입니다.

# 네이버 API 사용해보기

## Ex_NaverAPI 프로젝트 생성하기
- 패키지 이름을 이용해야 api를 사용할 수 있다 나중에 갖다 쓸꺼니 복사해서 잘 갖고 있자.
- NaverActivity로 이름 바꾸기

## 네이버에 프로젝트 등록하기
네이버 로그인 -> 아래 네이버 개발자 센터 -> application -> 등록 -> 휴대폰 인증 <br>
회사이름은 korea로 해도됨 아무거나<br>

![image](https://github.com/to7485/Web1500/assets/54658614/43dc3929-908c-4181-a68e-09b17a2732cc)

- 애플리케이션이름 도서검색을 할거라서 BookSearch 아니면 원하는거
- 사용 API -> 검색
- 환경추가 -> 안드로이드 설정

![image](https://github.com/to7485/Web1500/assets/54658614/fae166b7-edae-42e7-8f0a-55aa87ec2238)

- 메모장에 복사해서 갖고 있기.

![image](https://github.com/to7485/Web1500/assets/54658614/1134e63d-7fba-4c66-91a6-ffdedb06905a)


- 2만 5천건 검색은 무료인데 공부용으로는 충분함 무한루프문 잘못써서 다 날리는일 없도록 하자.
- Documents -> 검색 -> 책

![image](https://github.com/to7485/Web1500/assets/54658614/89b588d7-899d-4eb3-9b2d-7fe961b13027)

- 전부다 get 방식으로 요청을 해야 한다. 그리고 json 형태로 받는 것이 아니라 xml 형태로 받을것이다.
요청 URL 복사하기.

- URL 뒤에 파라미터를 붙혀서 ?query = aaaa 이런식으로 사용할 것이다.
- display 검색결과 건수 지정 100개씩 보여줄 수 있어서 사용하자!
- 제목, 이미지, 저자, 가격 정도의 정보만 가져와서 호출하려고 한다.
- vo에 담아서 arraylist에 담으면 목록을 다 보여줄 수 있다.
- url이랑 id 비밀번호 3가지 메모장에 저장 잘 해놓기

## activity_naver 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".NaverActivity"
    android:orientation="vertical">


<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:hint="input text"
            android:id="@+id/search"
            android:imeOptions="actionSearch"/>
이제 EditText에서 검색을 해서 ok버튼을 누르면 밑에 전화번호부처럼 스크롤 할 수 있는 목록이 나왔으면 좋겠어요 이걸 ListView라고 해요.

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="ok"
            android:id="@+id/search_btn"/>
    </LinearLayout>

책이 한권만 조회가 될건 아니기 때문에 ListView 라고 하는걸 사용한다.
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/myListView"/>

리스트 뷰는 안에 무언가 들어가는 태그가 아니기 때문에 다이렉트로 닫아도 된다.
전화번호부나 카카오톡도 그렇고 휴대폰 보면서 리스트로 되어있는건 많이 접하고 잇다.
리스트마다 모양이 다르다. 리스트 뷰를 사용하면 한칸당 내가 어떻게 디자인을 할지 직접 결정을 해야한다.
짝궁이 없는 xml을 만들어서 view로 만들어서 한칸으로 붙혀줘야 한다.
이미지 제목 저자,가격 에 대한 정보를 넣을 것이다. 한칸에 대한 디자인을 해보자
</LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:background="#70aaaaff"
        android:gravity="center"
        android:id="@+id/loading"
        android:visibility="gone">

        <ProgressBar
            android:layout_width="wrap_content"
            android:layout_height="wrap_content" />

    </LinearLayout>
</FrameLayout>
```
## layout 폴더에 Layout Resource File만들기
- book_item, LinearLayout으로 Layout Resource File을 하나 만들어주자
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#aac"
    android:padding="10dp">

    <ImageView
        android:layout_width="150dp"
        android:layout_height="200dp"
        android:src="@mipmap/ic_launcher_round"
        android:scaleType="fitXY"
        android:id="@+id/book_img"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="10dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="제목"
            android:textColor="#00f"
            android:textSize="20dp"
            android:id="@+id/book_title"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="저자"
            android:textColor="#00f"
            android:id="@+id/book_author"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="가격"
            android:textColor="#00f"
            android:id="@+id/book_price"/>

    </LinearLayout>

</LinearLayout>
```

- book_item.xml은 나중에 객체화가 될것이고 우리가 서버를 통해서 접근했을 때 가져올 목록들(제목,이미지경로,저자,가격)<br>이 네가지를 저장할 수 있는 vo를 만들어야 한다

## java폴더에서 vo 패키지 생성하고 BookVO 클래스 만들기
- 액티비티 아님
```java
package vo;

public class BookVO {
    //네이버 도서 서버에서 얻어올 정보를 저장할 변수
    //제목, 저자, 가격, 이미지

    private String b_title,b_author,b_img, b_price;

우클릭 generater -> getter setter

    public String getB_title() {
        return b_title;
    }

    public void setB_title(String b_title) {
        this.b_title = b_title;
    }

    public String getB_author() {
        return b_author;
    }

    public void setB_author(String b_author) {
        this.b_author = b_author;
    }

    public String getB_img() {
        return b_img;
    }

    public void setB_img(String b_img) {
        this.b_img = b_img;
    }

    public String getB_price() {
        return b_price;
    }

    public void setB_price(String b_price) {
        this.b_price = b_price;
    }
}

```
- vo에 저장하고 arrayList에 담아주는 역할을 하는게 dao이다.
- 안드로이드에서는 서버에서 넘어온 데이터를 안드로이드에 맞게 변경해주는 작업을 파싱이라고 합니다.

## java에 parser패키지 생성하고 Parser클래스 생성하기
```java
package parser;

import com.lhj.ex_naverapi.NaverActivity;

import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserFactory;

import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;
import java.util.ArrayList;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import vo.BookVO;

public class Parser {
    //웹에서 요소 (제목, 저자, 이미지, 가격)를 검색하여 vo에 저장 및 List에 저장
    BookVO vo;
    String myQuery= ""; //검색어

    //서버에 연결을 해서 xml파일을 불러오고, 자바로 파싱해서 필요한 요소만 vo에 담고 arrayList에 저장해서 list로 반환을 해주는 작업
    public ArrayList<BookVO> connectNaver(){
        ArrayList<BookVO> list = new ArrayList<>();

        try{
            //검색어(myQuery)를 UTF-8 형태로 인코딩

            myQuery = URLEncoder.encode(NaverActivity.search.getText().toString(),"UTF-8");

            //NaverActivity에 public static EditText search;
            // 스태틱 변수로 만들고 검색하기 하고 돌아오기
            //url 집어넣기 대신 어떤걸 검색할건지 파라미터를 보내야 한다.
```

![image](https://github.com/to7485/Web1500/assets/54658614/9e581d75-1b27-4a68-ae80-3fa273df2fbe)

``` java
            // = 뒤에 띄어쓰기 하지 않게 조심하기
            String urlstr = "https://openapi.naver.com/v1/search/book.xml?query="+myQuery+"&display=100";

            //저 경로에 검색어를 가지고 들어가고 들어가면 연결을 하기 위해 커넥션 객체가 필요하다.
            //url로 접근하면서 id와 secret을 보내줘야 한다.
            //일단은 서버에 접근하기 위해서 검색어랑 url만 만들어 놓은 상태다.

            //진짜 연결하려면 URL 이라고 하는 클래스가 있어야 한다.
            ///URL 클래스로 URL을 알면 CONNECTION이라고 하는 객체가 URL을 가지고 연결을 해준다.
            URL url = new URL(urlstr); -> 연결할 곳이 어딘지 알기 때문에
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            ㄴurl에 대한 정보가 url.openConnection()이 알고 있으니  통로를 열어주겠다

            //발급받은 ID와 Secret을 서버로 전달
            connection.setRequestProperty("X-Naver-Client-Id","UI_98rUkiVwuN_h1z_Qg");
            connection.setRequestProperty("X-Naver-Client-Secret","oVAafbTjPt");

            아이디와 비밀번호가 맞다면 connection을 통해서 연결이 될거고 연결이 됐다면
            받은 자원들은 xml 형식으로 넘어오기 때문에 자바코드로 바꿔주는 작업이 필요하다.

            //위의 URL을 수행하여 받은 자원들을 자바코드로 파싱
            XmlPullParserFactory factory = XmlPullParserFactory.newInstance(); -> 싱글톤

            //팩토리가 정보를 통채로 불러온오는 객체
            //pullparser는 item별로 하나씩 데이터를 가지고 올 수 있도록 하는 객체

            XmlPullParser parser = factory.newPullParser();

            //connection 객체가 접속후 가지게 된 내용을 parser가 스트림으로 읽어옵니다.
            parser.setInput(connection.getInputStream(), null);

            파서가 inputSteam을 통해서 connection 객체가 요청을 하고
            최종적으로 돌려받은 결과값을 inputStream으로 읽어올 수 있다.
            null은 encoding타입이라 정의하지 않아도 된다.
            결론적으로 parser라고 불리는 이 객체가 xml문서 내부에 가상의 커서를 만들어 주게 된다.
            next 요청해서 태그 안에 내용을 가져올 수 있게 준비하는 역할을 한다.

            //파서 객체를 통해 각 요소별 접근을 하게되고, 태그(요소)내부의 값들을 가져온다.
            //while문을 돌리면서 더이상 읽어올 책이 없을 때 까지 모든 정보를 다 가져올 것이다.
            int parserEvent = parser.getEventType();

            while(parserEvent != XmlPullParser.END_DOCUMENT) {//문서의 끝
                //서버쪽 xml문서의 끝을 만날 때 까지 while문이 만복

                //시작태그의 이름을 가져와 vo에 담을 수 있는 정보라면 vo에 추가
                if( parserEvent == XmlPullParser.START_TAG){
                    String tagName = parser.getName();

                    //next로 내려가다가 파싱해야 하는 요소로 접근을 했는
                    //파싱을 안해놓고 넘어가게 되면 다시 파싱을 할 수 가 없게 된다. 
```

```java
                    if(tagName.equals("title")) {
                        vo = new BookVO();
                        String title = parser.nextText();
                        //가져온 title에 <b>태그가 들어있는지 검사
                        //한글자 짜리 태그들을 찾아주면서 닫히는 태그들 까지 감지를 해줄수 있는 정규식
                        Pattern pattern = Pattern.compile("<.*?>");
                        Matcher matcher = pattern.matcher(title);

                        if(matcher.find()){
                            String s_title = matcher.replaceAll("");
                            vo.setB_title(s_title);
                        } else {
                            vo.setB_title(title);
                        }
                    } else if(tagName.equals("image")) {
                        String img = parser.nextText();
                        vo.setB_img(img);
                    } else if(tagName.equals("author")){
                        String author = parser.nextText();
                        vo.setB_author(author);
                    } else if(tagName.equals("price")){
                        String price = parser.nextText();
                        vo.setB_price(price);
                        list.add(vo); //마지막 정보인 price까지 찾고난 뒤 list에 저장
                    }
                }//if(.START_TAG)
                parserEvent = parser.next(); //다음요소를 가져올 때 순서대로 가져와야 한다.
            }//while

        } catch(Exception e){

        }
        return list;
    }
}

```


