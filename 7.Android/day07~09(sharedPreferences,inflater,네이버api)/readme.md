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
                        vo.setB_title(title);
                    } else if(tagName.equals("image")) {
                        String img = parser.nextText();
                        vo.setB_img(img);
                    } else if(tagName.equals("author")){
                        String author = parser.nextText();
                        vo.setB_author(author);
                    } else if(tagName.equals("discount")){
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

- connectNaver 메서드만 잘 호출하면 서버로 들어가서 내가 입력한 검색어에 대한 결과를 list에 담아서 빠져나올 수 있다.
- ArrayList에 담아온 내용들을 ListView에 담아줘야 한다.

## NaverActivity 에 코드 작성하기
```java
package com.lhj.ex_naverapi;

import androidx.appcompat.app.AppCompatActivity;

import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;
import android.view.KeyEvent;
import android.view.View;
import android.view.inputmethod.EditorInfo;
import android.widget.Button;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.ListView;
import android.widget.TextView;

import java.util.ArrayList;

import parser.Parser;
import vo.BookVO;

public class NaverActivity extends AppCompatActivity {

     public static EditText search;
     ListView myListView;
     Button search_btn;
     Parser parser; //connectNaver()메서드 가져오려면 parser객체 있어야함.

    ListView라고 하는걸로 집어넣으려면 어댑터라고 하는 개념이 필요한데 조금 나중에 추가해보도록 하겠다.

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_naver);

        search = findViewById(R.id.search);
        myListView = findViewById(R.id.myListView);
        search_btn = findViewById(R.id.search_btn);
        loading = findViewById(R.id.loading);
        parser = new Parser();

    }
}
```

- 서버에 들어가서 xml파싱 즉 웹에 있는 코드를 자바 형태로 변경해주는 작업, vo에 담고 arraylist에 담아주는 작업을search_btn을 눌렀을 때 진행이 되야되죠.

## parser 객체 아래에 이벤트 감지자 생성
```java
search_btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
정말 쉽게 생각하자면 parser.connectNaver() 호출하면 끝날 것 같죠 옛날 에는 됐었다.
근데 connectNaver도 메서드이기 때문에 메서드가 완전히 마무리 되야 다음 작업을 할 수 있다.
그런데 데이터를 가지고 올게 많다거나 하면 로딩이 되는 동안 다른 작업을 아무것도 할 수 없다.
뒤로가기도 막힘 터치도 안되고 클릭도 안되고 아무것도 안된다.
    //parser.connectNaver();

백그라운드 쪽에서 데이터 가져오는 동안
버튼 클릭을 하든 검색어를 바꾸든 니가 하고 싶은 작업을 프론트에서는 할 수 있어야 하기 때문에 막아버렸습니다.
앞으로 서버 통신을 할 때 백그라운드를 통해서 해야합니다.
    }
});
```

- 외부에 있는 정보를 데이터를 가져와야 하기 때문에 인터넷 접근 허용 권한을 줘야 한다.
- manifest.xml에 추가하기
```java
<!-- 인터넷 사용을 위해 반드시 필요한 권한 -->
<uses-permission android:name="android.permission.INTERNET" />
```

## onCreate() 밖에 NaverAsync 클래스 만들기
```java
class NaverAsync extends AsyncTask<String, Void, ArrayList<BookVO>>{
        @Override

        //doInBackground(String... strings) ...의 의미: variable arguments 배열의 개수를 따지지 않고 파라미터로 들어오는
        //모든 것들을 배열로 만들어 주는 기능을 가능하게 합니다.
        // *개수의 제한이 없습니다.
        //Strings[0] -> 홍
        //Strings[1] -> 길
        //Strings[2] -> 동
        protected ArrayList<BookVO> doInBackground(String... strings) {
           //필수 메서드!!
           //각종 반복이나 제어등의 백그라운드에서 필요한 처리코드를 담당하는 메서드

           return null;
        }
    }
```
- AsyncTask : 백그라운드에서의 서버통신을 위해 반드시 필요한 클래스
- AsyncTask는 세개의 제너릭 타입을 가지고 있다.
    1. doInBackground의 파라미터 타입
    2. onProgressUpdate가 오버라이딩 되어 있다면 이 메서드에서 사용할 타입
    3. doInBackground의 반환형이자, 작업의 최종 결과를 차지하는 onPostExecute()의 파라미터 타입


## search_btn 버튼 감지자 내용 채워주기
```java
        search_btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //서버에 연결(Async클래스의 doInBackgound()메서드를 호출)
                new NaverAsync().execute("홍","길","동");

                //로딩시작
                loading.setVisibility(View.VISIBLE);


            }
        });
```
- execute()로 doInBackground 메서드가 호출이 되고 doInBackground가 주요 코드들을 실행하고 이게 완료가 되면 그 다음 호출되는 메서드가 있다.

## onPostExecute메서드 추가하기
- 최종 작업 결과를 받는 메서드

```java
class NaverAsync extends AsyncTask<String, Void, ArrayList<BookVO>>{
        @Override

        //doInBackground(String... strings) ...의 의미: variable arguments 배열의 개수를 따지지 않고 파라미터로 들어오는
        //모든 것들을 배열로 만들어 주는 기능을 가능하게 합니다. *개수의 제한이 없습니다.
        //Strings[0] -> 홍
        //Strings[1] -> 길
        //Strings[2] -> 동
        protected ArrayList<BookVO> doInBackground(String... strings) {
            //필수 메서드!!
            //각종 반복이나 제어등의 백그라운드에서 필요한 처리코드를 담당하는 메서드드

           return parser.connectNaver();
        }

        @Override
        protected void onPostExecute(ArrayList<BookVO> bookVOS) {
            //doInBackground에서 return된 최종 작업 결과를 bookVOS가 받게 된다.
            //Log.i("my",""+bookVOS.size());
            //에뮬레이터를 켜보고 android를 치고 ok를 눌러보면 logcat에 100이 나온다.

            //반복문을 작성해서 검색어로 책의 제목들을 받아올 수 있다.
            for(int i = 0; i<bookVOS.size(); i++){
                Log.i("my" ,""+bookVOS.get(i).getB_title());
            }

        }
    }
```
- 앱을 실행하면 객체들이 검색되고 앱 화면에서 검색어를 입력받고 ok 버튼을 눌렀을 때 search_btn의 이벤트감지자가 실행이 된다.
- execute()라는 메서드를 통해 doInBackground가 호출이 되고 뒷단에서 connectNaver()가 수행이 된다.
- 작업이 정상적으로 완료되면 도서의 정보를 잔뜩 들고있는 arrayList가 반환이 되고 그 반환된 list를 onPostExecute로 보내는거다.

※검색결과가 0으로 나온다면 url이나 발급받은 ID, SECRET을 잘못썼을 가능성이 높다.

![image](https://github.com/to7485/Web1500/assets/54658614/d69d0414-f435-4e72-ad5b-f323a1d032ae)

- 이제 이걸 레이아웃에 담아서 List로 보여줄것이다.
- 목록화를 가능하게 하기 위해서 어댑터라고 하는 클래스를 만들어보자.

## ViewModelAdapter 클래스 생성하기
```java
package com.lhj.ex_naverapi;

import android.content.Context;
import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.AsyncTask;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.ListView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

import org.w3c.dom.Text;

import java.io.BufferedInputStream;
import java.net.URL;
import java.util.ArrayList;

import vo.BookVO;

public class ViewModelAdapter extends ArrayAdapter<BookVO> {

    Context context;
    int resource;
    BookVO vo;
    ArrayList<BookVO> list;

    //기본생성자가 없고 파라미터를 받는 생성자 밖에 없는데 ArrayAdapter에서 마우스 클릭하고 alt+enter 누르고 가장 위에 있는것 만들기.
    //(원래는 파라미터 두 개 받는데 생성자에 ArrayList<BookVO> list추가하기)

    //외부에서 두가지 정보를 받아온다고 해도 부모(super)에는 두가지 정보만 들어갈 뿐 list는 전달을 할 수 없기 때문에
    //list까지 부모한테 전달해주는 이 생성자가 ArrayAdapter를 상속받는 클래스가 가지는 가장 기본적인 모양이다.

    public ViewModelAdapter( Context context, int resource, ArrayList<BookVO> list, ListView myListView) {
        super(context, resource, list);
        this.context = context;
        this.resource = resource;
        this.list = list;

        myListView.setOnItemClickListener( click );

    }//생성자
}
```

## NaverActivity 클래스에 내용 추가하기
```java
ViewModelAdapter adapter; ViewModelAdapter 객체 준비하기

search_btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
            //검색을 했다가 다른걸 검색하면 list를 싹 비워야 한다.
            //java를 검색했다가 android를 검색하면 다른 내용이 나오듯이. 그래서 null을 한번 넣어주자.
            adapter = null;

        //서버연결(Async클래스의 doInBackground()메서드를 호출)
        new NaverAsync().execute(); //Async클래스의 doInBackground()메서드를 호출
    }
});

... 중략

@Override
protected void onPostExecute(ArrayList<BookVO> bookVOS) {
    //doInBackground에서 return된 최종 작업 결과를 bookVOS가 받게 된다.
    Log.i("my",""+bookVOS.size());

    //bookVOS를 ListView를 그리기 위해 존재하는 adapter클래스에게 넘겨줘야 한다.
    //방금 우리가 만든 클래스를 객체생성했는데 ViewModelAdapter는 파라미터를 3개 요구한다.
    adapter = new ViewModelAdapter(NaverActivity.this,R.layout.book_item,bookVOS, myListView);
    //두번째 파라미터는 우리가 만들어놓은 book_item.xml을 말한다.
    //그래서 리스트뷰 한칸 디자인 어떻게 하고싶은지 물어보는 것, 도서정보를 갖고 있는 bookVOS를 준다.

    //준비된 어댑터를 ListView에 탑재
    myListView.setAdapter(adapter);

    //로딩종료
    loading.setVisibility(View.GONE);


}

```

- 준비된 어댑터를 ListView에 탑재하면서 ViewModelAdapter클래스가 getView라는 메서드를 자동으로 호출한다.

## ViewModelAdapter 클래스에 getView메서드 작성하기
```java
package com.lhj.ex_naverapi;

import android.content.Context;
import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.AsyncTask;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.ListView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

import org.w3c.dom.Text;

import java.io.BufferedInputStream;
import java.net.URL;
import java.util.ArrayList;

import vo.BookVO;

public class ViewModelAdapter extends ArrayAdapter<BookVO> {

    Context context;
    int resource;
    BookVO vo;
    ArrayList<BookVO> list;

    public ViewModelAdapter( Context context, int resource, ArrayList<BookVO> list, ListView myListView) {
        super(context, resource, list);
        this.context = context;
        this.resource = resource;
        this.list = list;

        myListView.setOnItemClickListener( click );

    }//생성자

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        //myListView.setAdapter(adapter) 하는 순간 호출되는 메서드(getView())
        //생성자의 파라미터를 받은 list의 사이즈만큼 getView()메서드가 반복 호출

        //position : 처음 1회전 할 때 포지션은 0 부터 호출이 되서 100번이 돌면 포지션은 99가 된다.


        //xml파일을 view 형태로 만들 준비
        //일반 액티비티에서는 LayoutInflater linf = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICET); 가능하다.
        //일반클래스에서는 context가 항상 필요하다.
        LayoutInflater linf = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);

        //conertView -> boo_item.xml 짝궁이 없는 xml파일을 view 형태로 변경
        convertView = linf.inflate(resource, null);

        return convertView;
    }//getView()
}

```

- 에뮬레이터 실행하고 아무키워드나 검색해보기

![image](https://github.com/to7485/Web1500/assets/54658614/8370da57-24c1-45df-b5dc-241e3b2076c3)

- 제목,저자,가격을 각각의 정보에 맞게 바꿔보자.

## ViewModelAdapter 클래스에 내용 추가하기
```java
 @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        //myListView.setAdapter(adapter) 하는 순간 호출되는 메서드(getView())
        //생성자의 파라미터를 받은 사이즈만큼 getView()메서드가 반복 호출

        //xml파일을 view 형태로 만들 준비
        LayoutInflater linf = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);

        //conertView -> boo_item.xml 짝궁이 없는 xml파일을 view 형태로 변경
        convertView = linf.inflate(resource, null);

        vo = list.get(position);
        //포지션이 0~99번까지 알아서 반복하기 때문에 반복문을 쓰지 않아도 100개에 대한
        //내용을 vo에 순차적으로 담을 수 있다.

        TextView title = convertView.findViewById(R.id.book_title);
        TextView author = convertView.findViewById(R.id.book_author);
        TextView price = convertView.findViewById(R.id.book_price);
        ImageView img = convertView.findViewById(R.id.book_img);

        title.setText(vo.getB_title());
        author.setText(vo.getB_author());
        price.setText(vo.getB_price() + "원");

        return convertView;
    }//getView()
```

- 에뮬레이터 실행해서 내용이 나오는지 확인하기
- 실행을 해서 검색을 해보면 태그도 같이 나오는걸 확인 할 수 있다.
- 태그를 지워보는 작업을 해보자.
- 태그가 나오는 이유는 api가 앱에서만 사용하는게 아닌 웹에서도 사용을 할 수 있기 때문이다.
- 안드로이드에서는 b태그가 안먹는다. Strign 타입으로 되어있는데 태그를 지우는 방법은 정규식을 이용하면 된다.

## parser 클래스로 수정해주기
```java
 while(parserEvent != XmlPullParser.END_DOCUMENT) {//문서의 끝
                //서버쪽 xml문서의 끝을 만날 때 까지 while문이 만복

                //시작태그의 이름을 가져와 vo에 담을 수 있는 정보라면 vo에 추가
                if( parserEvent == XmlPullParser.START_TAG){
                    String tagName = parser.getName();
///////////////////////////////////////////////////////////////////////////////////////////////
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
///////////////////////////////////////////////////////////////////////////////////////////////
                    } else if(tagName.equals("image")) {
                        String img = parser.nextText();
                        vo.setB_img(img);
                    } else if(tagName.equals("author")){
                        String author = parser.nextText();
                        vo.setB_author(author);
                    } else if(tagName.equals("discount")){
                        String price = parser.nextText();
                        vo.setB_price(price);
                        list.add(vo); //마지막 정보인 price까지 찾고난 뒤 list에 저장
                    }
                }//if(.START_TAG)
                parserEvent = parser.next(); //다음요소를 가져올 때 순서대로 가져와야 한다.
            }//while
```

- 에뮬레이터를 켜고 검색을 해보면 b태그들이 빠져잇는걸 확인할 수 있다.

# 이미지 넣기
- 사실 이미지에 대한 경로도 vo에 저장이 되어있다.
- 이미지는 텍스트보다 로딩을 하는 시간이 오래 걸린다.
- url로 접근을 해서 가져와야 하는 상황이기 때문에 이미지를 가지고 오려면 서버통신을 위한 어싱크 클래스가 또 있어야 한다.

## ViewModelAdapter 클래스 코드 추가하기
```java
class ImgAsync extends AsyncTask<Void, Void, Bitmap>{
        Bitmap bm;
        ImageView img;
        BookVO vo;

        public ImgAsync(ImageView img, BookVO vo) {
            this.img = img;
            this.vo = vo;
        }

        @Override
        protected Bitmap doInBackground(Void... voids) {
            return null;
        }
        @Override
        protected void onPostExecute(Bitmap bitmap) {
            //비트맵 객체를 이미지뷰로 전환
            img.setImageBitmap(bitmap);
        }
    }

```

## ViewModelAdapter 클래스 코드 수정하기
- 백그라운드에서 이미지 로드하도록 수정하기
```java
price.setText(vo.getB_price() + "원");

////////////////////////////////////////////////////////////////////
    new ImgAsync(img,vo).execute();//doInBackground() 호출
////////////////////////////////////////////////////////////////////

    return convertView;
}//getView()
```

## ViewModelAdapter 클래스 doInBackground 수정하기
```java
@Override
protected Bitmap doInBackground(Void... voids) {
    try{
        //vo가 가지고 있는 vo.getB_img()를 통해
        //이미지 경로를 따라 들어가자.
        URL img_url = new URL(vo.getB_img());

        //BufferedInputStream을 통해 이미지를 로드 : 전용공간을 만들어서 데이터를 빠르게 가져올 수 있다.
        //buffered : InputStream을 도와 더 빨리 데이터를 입력, 출력 하기 위한 스트림
        BufferedInputStream bis = new BufferedInputStream(img_url.openStream());

img_url을 통해서 얻어온 이미지 경로가 있다. 그러면 inputStream을 통해서 데이터를 읽어올수 있다.

bis가 이미지의 정보를 데이터 단위로 저장을 해놓는다.
바로 이미지뷰로 넣을수는 없다.
1바이트 단위의데이터를 이미지형태로 쓸수 있도록 하는 클래스가 bitmap이다.


        //bis가 읽어온 데이터를 이미지로 변환하기 위해
        //bitmap 형태로 변경
        bm = BitmapFactory.decodeStream(bis);

        bis.close();

    }catch (Exception e) {


    }
    if(bm != null){
        return bm;
    }
    //불러올 이미지가 없을 때 기본이미지로 bitmap 설정
    Bitmap bitmap = BitmapFactory.decodeResource(context.getResources(),R.drawable.rabbit);
    return bitmap;
}
@Override
protected void onPostExecute(Bitmap bitmap) {
    //비트맵 객체를 이미지뷰로 전환
    img.setImageBitmap(bitmap);
}

```

# 상세보기
- 항목을 클릭했을 때 안에 있는 정보들을 다른 activity로 보내서 상세보기 페이지처럼 정리를 해보자.

## SubActivity생성하기
- 레이아웃으로 이동해 디자인하기

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".SubActivity">
    <!--그림을 위 가운데에 나오게 해보자-->

    <ImageView
        android:id="@+id/img"
        android:layout_width="250dp"
        android:layout_height="350dp"
        android:layout_gravity="center"
        android:src="@drawable/rabbit" />
    <!--사진이 너무 찌그러져 보이지 않도록 조정하자-->

    <!--밑에 제목이랑 저자 가격 그리고 확인버튼 누르면 다시 첫페이지로 돌아가도록 처리하고 싶어서 TextView를 만들자.-->

    <TextView
        android:id="@+id/txt_title"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="제목"
        android:textSize="30dp" />
    <!-- 제목이 들어갈 예정입니다.-->
    <!--화면을 넘어가는 상황도 어느정도 계산하고 만드는 중이다.-->

    <TextView
        android:id="@+id/txt_author"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="저자"
        android:textSize="30dp" />

    <TextView
        android:id="@+id/txt_price"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="가격"
        android:textSize="30dp" />

   <!-- 버튼을 하나 더 만들어서 확인 이라고 넣어주자-->

    <Button
        android:id="@+id/btn_ok"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="확인" />
</LinearLayout>
```

## SubActivity코드작성하기
```java
package com.korea.ex_0716;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

public class SubActivity extends AppCompatActivity {

    ImageView img;
    TextView txt_title, txt_author, txt_price;
    Button btn_ok;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sub);

        img = findViewById(R.id.img);
        txt_title = findViewById(R.id.txt_title);
        txt_author = findViewById(R.id.txt_author);
        txt_price = findViewById(R.id.txt_price);
        btn_ok = findViewById(R.id.btn_ok);//앞으로 ok 버튼을 누르면 이전화면으로 돌아가도록 지정을 할것이다.

        btn_ok.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                finish();/* 버튼을 눌렀을 때 이전페이지로 보내기 위해서 finish()를 써주자*/
            }
        });


    }
}
```

## ViewModelAdapter 클래스에 코드 작성하기
- 어댑터 쪽에서 항목을 눌렀을 때 0번을 눌렀으면 0번에 대한 정보가 서버로 가야 할 것이다.
- 이벤트 처리를 하려면 리스트뷰가 클릭이 되었다는걸 알아야 한다.
- 리스트뷰가 클릭되었다는걸 알아야 해서 어댑터가 생성될 때 리스트까지 받고난 다음
- ListView myListView를 하나 더 파라미터로 받아주자.
```java
public ViewModelAdapter( Context context, int resource, ArrayList<BookVO> list, ListView myListView) {
myListView는 부모에게 넘겨주는게 아니다. 이벤트 처리 하고싶어서 요청을 하고 있는거다. 
        super(context, resource, list);
        this.context = context;
        this.resource = resource;
        this.list = list;
우리가 ViewModelAdapter를 호출하고 있는 곳이 네이버액티비티 이다.

    }//생성자
```

## NaverActivity 클래스 코드 추가하기
```java

 @Override
protected void onPostExecute(ArrayList<BookVO> bookVOS) {
    //doInBackground에서 return된 최종 작업 결과를 bookVOS가 받게 된다.

    //bookVOS를 ListView를 그리기 위해 존재하는 adapter클래스에게 넘겨줘야 한다.
    adapter = new ViewModelAdapter(NaverActivity.this,R.layout.book_item,bookVOS, myListView);

파라미터가 하나 모자라졌기 때문에 myListView를 담아서 보내주자. 이제 myListView도 가져가봐 하고 보내준다 

    //준비된 어댑터를 ListView에 탑재
    myListView.setAdapter(adapter);

    //로딩종료
    loading.setVisibility(View.GONE);

}

ViewModelAdapter에서 받아준 myListVeiw 라고 하는 녀석은 어댑터 클래스에서 이벤트 감지자를 추가할 수 있다.
```

## ViewModelAdapter 클래스에 이벤트 처리하기
```java
    public ViewModelAdapter( Context context, int resource, ArrayList<BookVO> list, ListView myListView) {
        super(context, resource, list);
        this.context = context;
        this.resource = resource;
        this.list = list;

 //메인에서 넘어온 listView에게 이벤트 감지자 등록
클릭이라고 해서 onclick리스너는 아니다. item을 클릭했을 때를 감지해야한다.
        myListView.setOnItemClickListener( click );

    }//생성자

//리스트뷰의 클릭을 감지하는 감지자 생성
AdapterView.OnItemClickListener click = new AdapterView.OnItemClickListener() {
int i는 책에 대한 자세한 내용을 보기 위해서 항목을 누를때 맨 위에 걸 클릭하면 i가 0이다
    @Override
    public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
            String title = list.get(i).getB_title();
            String author = list.get(i).getB_author();
            String price = list.get(i).getB_price();

이미지 뷰 같은 경우에는 메인에서 서브로 곧장 넘길 수 있는 방법이 없다.
비트맵이면 가능 하겠지만 이미지뷰는 불가능 하다.
이미지 경로를 가지고 넘어간 페이지에서 통신을 한번 더할거다.
어차피 이미지 경로는 알아야 하기 때문에 추가해주자.

            String img = list.get(i).getB_img();

        //화면전환을 위한 Intent 준비
Intent에서 context가 필요한데 ViewModelAdapter는 액티비티가 아니기 때문에 쓸수 없다/
우리가 생성자로 받아놓은 context가 있기 때문에 그걸 사용하면 된다.
여기가 마치 Naver액티비티인것 처럼 생각하고 사용하자. 목적지는 Sub액티비티
        Intent intent = new Intent(context,SubActivity.class);
        intent.putExtra("title",title);
        intent.putExtra("author",author);
        intent.putExtra("price",price);
아쉽게도 putExtra에 담을수 있는 파라미터를 보면 이미지뷰가 없다.
 그래서 String 타입으로 경로를 담아서 보내줘야 한다.
        //이미지를 직접 보내지 못하는 이유 : putExtra에 이미지뷰를 담을 수 없기 때문에
        intent.putExtra("img",img);

액티비티가 아니라 클래스 이므로 startActivity를 단독으로 사용할 수 없다.
context.startActivity 라고 작성을 해줘야 한다.
        context.startActivity(intent);
    }
};
```

## SubActivity에서 파라미터 받아주기
```java
Intent i;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sub);

        //이전페이지에서 넘어온 intent좀 줘봐라
        //ViewModelAdapter에서 넘어온 정보를 현재 액티비티에서 받아온다.

        i = getIntent();
        String s_title = i.getStringExtra("title");
        String s_author = i.getStringExtra("author");
        String s_price = i.getStringExtra("price");
        s_img = i.getStringExtra("img");

받아준 파라미터를 TextView에다가 넣어주자.

        txt_title.setText(s_title);
        txt_author.setText(s_author);
        txt_price.setText(s_price+" 원");

에뮬레이터를 실행해서 확인을 해보면 정보가 잘 넘어온걸 확인할 수 있다.
이미지는 빼고... 제목이 진짜 긴 요소를 클릭해보면 제목이 너무 길어
확인버튼이 짤릴랑 말랑 하거나 진짜 짤리는 수가 있다.. 이거에 대한 해결법을 찾아야 한다.

스크롤을 만들어줄 필요가 있다. 
```
## SubActivity의 레이아웃에 코드 수정하기
```java
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SubActivity"
    android:orientation="vertical"
    android:padding="10dp">
    <!--세로로 화면이 넘칠 때 스크롤을 생성해 주는 객체
       스크롤 뷰의 직계 자식은 반드시 1개여야 한다.-->

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    <ImageView
        android:layout_width="250dp"
        android:layout_height="300dp"
        android:src="@drawable/rabbit"
        android:layout_gravity="center"
        android:layout_marginTop="10dp"
        android:id="@+id/img"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="제목"
        android:textSize="30dp"
        android:id="@+id/txt_title"/>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="저자"
        android:textSize="30dp"
        android:id="@+id/txt_author"/>
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="가격"
        android:textSize="30dp"
        android:id="@+id/txt_price"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="확인"
        android:id="@+id/btn_ok"/>
    </LinearLayout>

</ScrollView>
```
- 이미지를 통신을 통해서 가져와야 한다.
- 상세보기 페이지로 넘어갔으니까 거기에 맞는 이미지를 통신을 통해서 가져오자

## SubActivity로 클래스에 코드 추가하기
- 여기에다가 어댑터에서 했던 것 처럼 이미지의 경로를 알려줘야 한다.
- 이미지의 경로를 알려줘야 하기 때문에 s_img를 특별대우를 해주자.
- s_img를 윗부분에다가 Strimg s_img; 로 멤버변수로 만들어주자.
- Bitmap 객체도 하나만들어주자.
```java
Bitmap bm;
String s_img;

    new ImgAsync().execute(s_img);
}//onCreate()
```
- AsyncTask의 제너릭타입을 의도적으로 String으로 정한 이유가 있다.
- .execute()를 통해 doInBackground를 호출할 때 이미지 경로를 주고 싶어서.
- 이미지 경로는 s_img가 가지고 있다 execute()를 호출할 때 같이 보내주자.
```java
class ImgAsync extends AsyncTask<String, Void, Bitmap> {

        @Override
        protected Bitmap doInBackground(String... strings) {
doInBackground 작업 마무리 되면 밑에 메서드 호출된다.
            try {

나 url로 모델어댑터에서 했던거랑 똑같이 이미지를 가지고올 경로가 필요한데 나한테좀 주라.

                URL img_url = new URL(strings[0]);

그리고 이미지를 비트맵으로 가지고와보자.

                BufferedInputStream bis = new BufferedInputStream(img_url.openStream());

읽어온 스트림을 비트맵에 맞게 재조합을 하자
이미지가 정상적으로 불러와졌다면 bm이 널이 아닐 것이다.

                bm = BitmapFactory.decodeStream(bis);
                bis.close();

            } catch (Exception e) {

            }

이미지를 제대로 가지고 왔는지 확인을 해보자

            if (bm != null) {
                return bm;
            }

그렇지 않다면 비어있는 이미지다.
이미지가 없는 책을 선택했을 때는 기본이미지가 나오게 해야 한다.
이미지 갖고 올거 없으면 토끼 이미지 가져다 쓸꺼지? 반환해

            Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.rabbit);
            return bitmap;
        }

        @Override
        protected void onPostExecute(Bitmap bitmap) {
            img.setImageBitmap(bitmap);
        }
    }
나중에는 '해당 도서는 이미지가 없습니다' 라는 아이콘 하나 만들어서 사용하시면 된다.
에뮬레이터를 켜서 이미지가 잘 들어갔는지 확인을 해보자
```

## NaverActivity 수정하기
- 에뮬레이터 켜고 실행하면 버튼이 돋보기 모양으로 바뀌었지만 눌러도 반응이 없다.
- 강제로 ok 버튼을 클릭을 해야 한다.
```java
//가상키보드에서 검색버튼을 누른경우 실제 ok버튼을 강제로 클릭
    search.setOnEditorActionListener(new TextView.OnEditorActionListener() {
        @Override
        public boolean onEditorAction(TextView textView, int i, KeyEvent keyEvent) {
            if (i == EditorInfo.IME_ACTION_SEARCH) {//search_btn버튼을 강제로 클릭
                search_btn.performClick();
            }
            return true;
        }
    });
}//onCreate()
```
- 앱을 처음에 열면 EditText가 처음에 존재하는데 포커스가 자동으로 잡혀있다.
- 뭔가를 입력할거라고 생각하기 때문에 가상키보드가 강제로 올라온다.
- 시작부터 올라오는게 좋기도 하지만 입력하려고 터치한 순간 올라왔으면 좋겠다 라는 생각이 들수도 있다.
- 가상키보드를 강제로 감추는 방법을 알아보자.

## Manifest에 코드 추가하기
```xml
<activity
    android:name=".NaverActivity"
    android:exported="true"
    android:windowSoftInputMode="stateHidden">
```
## Naver액티비티로 이동하여 코드 수정하기.
```java
search_btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        adapter = null;        
        //서버연결(Async클래스의 doInBackground()메서드를 호출)        
        new NaverAsync().execute(); 
        //Async클래스의 doInBackground()메서드를 호출

        //검색하면 다시 가상키보드가 내려감
        InputMethodManager imm = (InputMethodManager) getSystemService(INPUT_METHOD_SERVICE);
        imm.hideSoftInputFromWindow(search.getWindowToken(), 0);
    }
});
```

# 로딩바 띄우기
- ok버튼을 누르는 시점에 로딩하는 레이아웃을 띄워줄 꺼고 로딩이 끝나는 시점에 숨길 것이다.
- 로딩이라고 하는 이 이아디를 검색해주자.

```java
    public static EditText search;
    ListView myListView;
    Button search_btn;
    Parser parser;
    ViewModelAdapter adapter;
//////////////////////////////////////
    LinearLayout loading;//로딩화면을 표시하는 리니어레이아웃
//////////////////////////////////////

loading = findViewById(R.id.loding);

search_btn.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //서버에 연결(Async클래스의 doInBackgound()메서드를 호출)
            new NaverAsync().execute("홍","길","동");
/////////////////////////////////////////////////////////////////////
            //로딩시작
            loading.setVisibility(View.VISIBLE);
/////////////////////////////////////////////////////////////////////
검색하면 다시 가상키보드가 내려감
        InputMethodManager imm = (InputMethodManager) getSystemService(INPUT_METHOD_SERVICE);
        imm.hideSoftInputFromWindow(search.getWindowToken(),0);
        }
    });

@Override
        protected void onPostExecute(ArrayList<BookVO> bookVOS) {
            //doInBackground에서 return된 최종 작업 결과를 bookVOS가 받게 된다.
            Log.i("my",""+bookVOS.size());
            //bookVOS를 ListView를 그리기 위해 존재하는 adapter클래스에게 넘겨줘야 한다.
            adapter = new ViewModelAdapter(NaverActivity.this,R.layout.book_item,bookVOS, myListView);

            //준비된 어댑터를 ListView에 탑재
            myListView.setAdapter(adapter);
//////////////////////////////////////////////////////////////
            //로딩종료
            loading.setVisibility(View.GONE);
/////////////////////////////////////////////////////////////

        }
```
