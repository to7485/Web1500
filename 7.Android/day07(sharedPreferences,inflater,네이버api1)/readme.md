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
