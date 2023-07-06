# Ex_오늘날짜 프로젝트 생성하기
- 이름만 잘 설정하면 나머지는 설정이 되어 있기 때문에 크게 바꿀 것 없다.

## EventActivity로 이름 바꾸기
- 레이아웃 파일은 이름이 안바뀌기 때문에 직접 바꿔주자

## 디자인 만들어보기

![image](https://github.com/to7485/Web1500/assets/54658614/cef6bd40-ddcb-4f2b-b646-26e1d46aadd8)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".EventActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="3dp">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Red" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="green" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="bule" />
    </LinearLayout>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:background="#000000"
        android:gravity="center"
        android:text="결과"
        android:textColor="#fff"
        android:textSize="40dp" />

</LinearLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/62d8b371-3d8f-4003-8aaa-eb8fdc550028)

## 이벤트처리하기
- 각각의 버튼을 누르면 아래 화면을 각각의 색깔로 바꾸고 글씨도 바꿔보자.
- 이벤트 처리에 필요한 객체가 총 몇개인지 확인해야 한다.
- 이벤트 처리할 때 건드려야할 객체가 있다면 거의 id가 필요하다.

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_weight="1"
    android:text="Red"
    android:id="@+id/btn_red"/>

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:layout_weight="1"
    android:text="green"
    android:id="@+id/btn_green"/>

<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:layout_weight="1"
    android:text="bule"
    android:id="@+id/btn_blue"/>
</LinearLayout>

<TextView
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:layout_weight="1"
android:background="#000000"
android:gravity="center"
android:text="결과"
android:textColor="#fff"
android:textSize="40dp"
android:id="@+id/txt"/>
```

## EventActivity클래스에 코드 작성하기

```java
package com.korea.ex_0706;

import androidx.appcompat.app.AppCompatActivity;

import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class EventActivity extends AppCompatActivity {

    //레이아웃에서 사용하는 모든 객체들은 액티비티에서 클래스로 존재한다.
    Button b_red, b_green, b_blue;
    TextView txt;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_event);

        //이벤트에 사용할 객체들'검색'
        //이벤트에 사용할 객체들'검색'
        b_red = findViewById(R.id.btn_red);
        b_green = findViewById(R.id.btn_green);
        b_blue = findViewById(R.id.btn_blue);
        txt = findViewById(R.id.txt);

        b_red.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //버튼클릭시 호출되는 영역
                txt.setBackgroundColor(Color.RED);
                txt.setBackgroundColor(Color.parseColor("#ff0000"));
            }
        });

        b_green.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //버튼클릭시 호출되는 영역
                txt.setBackgroundColor(Color.RED);
                txt.setBackgroundColor(Color.parseColor("#00ff00"));
                txt.setText("초록");
            }
        });

        b_blue.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {    
                //버튼클릭시 호출되는 영역
                txt.setBackgroundColor(Color.RED);
                txt.setBackgroundColor(Color.parseColor("#0000ff"));
                txt.setText("파랑");
            }
        });
    }
}
```

## R 클래스
- 안드로이드에는 R클래스가 객체를 static으로 제공한다.
- R.java 객체는 안드로이드 리소스(레이아웃, 이미지, 문자열 등)를 식별하기 위한 변수들을 관리하는 R 클래스이다.
- 소스파일(java)에서 resource에 접근할 때 R클래스 사용
- XML파일에서는 R클래스의 역할을 @가 대시한다.

### 객체가 id를 부여받는 순간 R.java가 16진수의 정수형태로 값을 저장한다.
- 심지어 레이아웃을 만들때조차 정수로 저장한다.
- 하지만 findViewById(정수값)으로 검색하면 오류가 난다.
- 빌드할때마다 값이 바뀌기 때문이다.

## 이벤트 처리 실습
- EditText에 글을 써서 send를 누르면 화면에 해당 글씨가 뜨고
- Reset버튼을 누르면 지금 화면에 검은색 화면에 결과 라는 글씨가 뜨게 만들어보자.

## 레이아웃에 객체 추가하기
```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

    <EditText
        android:id="@+id/et"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:inputType="text" />

    <Button
        android:id="@+id/send"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="send" />

    <Button
        android:id="@+id/btn_reset"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:text="reset" />
</LinearLayout>
```

## EventActivity에 코드 추가하기
```java
Button b_red, b_green, b_blue, btn_send, btn_reset;
EditText et;
TextView txt;

btn_send = findViewById(R.id.send);
btn_reset = findViewById(R.id.btn_reset);
et = findViewById(R.id.et);

btn_send.setOnClickListener(click);
btn_reset.setOnClickListener(click);

//OnClickListener 인터페이스 안드로이드에서는new로 생성이 가능하다. click 감지자 라고 함
```

![image](https://github.com/to7485/Web1500/assets/54658614/f85cb615-7849-426e-bb44-082d13168744)

```java
// 버튼은 두 개인데 감지자는 하나이다. 버튼마다 다른 이벤트를 만드려고 하면 애매하게 되버린다.
//그렇기 때문에 감지자 안에 switch문을 적어준다.
View.OnClickListener click = new View.OnClickListener() {
    @Override
    public void onClick(View view) { //View클래스가 모든 클래스의 부모이다.
        Toast.makeText(EventActivity.this, "클릭완료", Toast.LENGTH_SHORT).show();

            int id = view.getId(); //view : 현재 클릭된 객체
            if (id == R.id.btn_send) {    //EditText에 쓰여진 문장을txt에 적용
                String str = et.getText().toString(); // getText()의 반환형이String이 아니기 때문에 문자열로 변환을 해주자
                txt.setText(str);
            } else if (id == R.id.btn_reset) {
                txt.setBackgroundColor(Color.BLACK);
                txt.setText("결과");
            }//switch
    }
};
```

# 회문수 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/9b0af789-6d71-4c00-9b58-dd04a2ce6008)

EditText에 값을 넣고, OK버튼을 누르면<br>
TextView에서 회문수인지 아닌지를 판단하시오<br>
(회문수 : 앞에서 읽어도 뒤에서 읽어도 똑같이 읽히는 값<br>
예 – 12321, aba, aabbaa...)<br>


## WorkActivity클래스 생성하기
- layout_work.xml 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".WorkActivity"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <EditText
            android:id="@+id/et"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:inputType="text"/>
        <Button
            android:id="@+id/btn_ok"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="ok"/>
    </LinearLayout>

    <TextView
        android:id="@+id/txt_result"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="결과"
        android:gravity="center"
        android:textSize="50dp"/>

</LinearLayout>
```

- WorkActivity클래스에 코드 작성하기
```java
package com.korea.ex_0706;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class WorkActivity extends AppCompatActivity {

    EditText et;
    Button start_btn;
    TextView txt_result;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_work);

        et = (EditText)findViewById(R.id.et);
        start_btn = (Button)findViewById(R.id.btn_ok);
        txt_result = (TextView)findViewById(R.id.txt_result);

        start_btn.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {

                //et에 작성된 원본 내용을 가져온다.
                String str = et.getText().toString().trim();
                String str2 = "";

                //원본을 뒤집어서 저장하는 코드
                for(int i = str.length()-1; i >= 0; i--){
                    str2 += str.charAt(i);
                }

                //혹은 스트링 버퍼를 이용하여
//                StringBuffer sbuf = new StringBuffer();
//                String str = et.getText().toString().trim();
//                sbuf.append(str);
//
//                StringBuffer sbuf2 = sbuf.reverse();
//                String str2 = sbuf2.toString();
//
                if(str.equals(str2)){
                    txt_result.setText("회문수");
                }else{
                    txt_result.setText("안회문수");
                }
            }
        });
    }
}
```



