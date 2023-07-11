# Dialog
- dialog는 화면에 보여지는 작은 윈도우이다.
- 화면을 채우지 않고 사용자에게 어떤 정보를 전달하거나 추가적인 정보를 입력받을 수 있다.
- Dialog 클래스가 있지만, 직접 사용하기 보다는 Sub Class인 AlertDialog사용을 권장한다.
- AlertDialog에는 세 개의 버튼만 들어갈 수 있다.

## Ex_날짜 프로젝트 생성하기
- AlertDialogActivity 생성하기
- 버튼을 눌러서 팝업창을 띄워보자.

## layout_alert_dialog에 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".AlertDialogActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="show alert dialog"
        android:id="@+id/btn_show"/>

</LinearLayout>
```

## AlertDialogActivity에 코드 버튼 등록하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.app.AlertDialog;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class AlertDialogActivity extends AppCompatActivity {

    Button btn_show;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_alert_dialog);

        btn_show = findViewById(R.id.btn_show);

        btn_show.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //버튼을 눌렀을 때 다이얼로그 띄우기
                AlertDialog.Builder dialog = new AlertDialog.Builder(AlertDialogActivity.this);

                //메뉴,토스트와같이 위에 뭐가 있던지 무시하고 뚫고 나오는 애들은 팝업 또는 위젯 이라고 해서
                //show()메서드가 없으면 보이지 않는다.
                dialog.show();
            }
        });
    }
}
```
- AlertDialog 클래스를 내부에 Builder라는 클래스가 static으로 정의되어 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/7b99c3b6-41ac-4ec6-9e80-d19a8f588c51)

- 이는 AlertDialog를 builder 패턴으로 만든것이다.
- 실행을 해도 넣어준 내용이 없기때문에 아무것도 뜨지 않는다.

### Builder 패턴
- 별도의 Builder 객체에서 복잡한 객체의 일부를 만들고 조합하는것을 캡슐화 한다.
- 클래스는 객체를 직접 생성하는 대신 Builder 객체에 객체 생성을 위임한다.

### builder 패턴의 장점
- 제품의 내부 표현을 변경할 수 있다.
- 구성 및 표현을 위한 코드를 캡슐화 한다.
- construction process의 단계에 대한 제어를 제공한다

- AlertDialog의 내부의 Build클래스에서 여러가지 메서드를 제공한다.

![image](https://github.com/to7485/Web1500/assets/54658614/a8a0950e-942e-4de0-a880-9bdd819a6364)


## AlertDialog에 여러가지 요소와 이벤트 추가하기
```java
 //버튼을 눌렀을 때 다이얼로그 띄우기
AlertDialog.Builder dialog = new AlertDialog.Builder(AlertDialogActivity.this);

dialog.setTitle("카카오톡");
dialog.setMessage("평가점... ㅋㅋㅋ"); //->여기까지 작성하고 다시한번 확인하기

//다이얼로그에 버튼 추가
dialog.setPositiveButton("네", new DialogInterface.OnClickListener() {
                        //ㄴ버튼에 쓰여질텍스트와 이벤트 감지자를 같이 넣는다 .
    // new On 하고 자동완성 중에 DialogInterface... 눌러서 완성

    @Override  //버튼 눌렀을때 이 안에서 이벤트처리 해줄게
    public void onClick(DialogInterface dialogInterface, int i) {
        Toast.makeText(AlertDialogActivity.this, "평가예정임", Toast.LENGTH_SHORT).show();
    }
});

//메뉴,토스트와같이 위에 뭐가 있던지 무시하고 뚫고 나오는 애들은 팝업 또는 위젯 이라고 해서
//show()메서드가 없으면 보이지 않는다.
dialog.show();
```

- 에뮬레이터 켜고 확인하기

## 버튼 추가하기
- AlertDialog에는 버튼을 세 개 까지밖에 만들 수 없다.
- positive버튼을 만들었기 때문에 다시 positive로 만들수 없다.
- 다시 만들게 되면 마지막에 만든걸로 갱신이 되버린다.
- negative버튼을 만들어보자

```java
// dialog.setNegativeButton("아니오", new DialogInterface.OnClickListener() {
//   @Override //int i : 클릭한 버튼에 대한 정보
//   public void onClick(DialogInterface dialogInterface, int i) {
//
//   }
// });

아니오를 누르면 다시 앱으로 돌아오게 만들것이다.
할 일이 없기 때문에 다이얼로그로 돌아오면 된다.
이벤트 처리를 할 필요가 없다는 얘기입니다.
new 해서 감지자 만들지 말고 그냥 null 이라고 넣으면 된다. 

dialog.setNegativeButton("아니오",null);

dialog.setNeutralButton("나중에",null);

//다이얼로그가 뒤로가기나 빈공간 터치로 사라지는것을 방지
dialog.setCancelable(false);
```

![image](https://github.com/to7485/Web1500/assets/54658614/67e3a3ad-c8aa-4150-a1f5-8b7ff85f22e4)

- 아니오 예를 바꾸고 싶으면 positive와 negative에 들어가는 문구를 바꾸면 된다.
- 어차피 실제 긍정과 부정의 의미를 지니는게 아니다.

## onBackPressed() 메서드
- 뒤로가기 버튼을 눌렀을 때 호출되는 메서드
- AlertDialog액티비티의 onCreate 영역 밖에다 작성하기

```java
@Override
public void onBackPressed() {
    //뒤로가기 버튼을 클릭했을 때 호출되는 메서드
    //super.onBackPressed(); -> 없으면 뒤로가기 눌러도 종료가 안된다.
    Toast.makeText(AlertDialogActivity.this, "뒤로가기 누름", Toast.LENGTH_SHORT).show();
}
```

## 실습
- 뒤로가기 버튼을 눌렀을 때 종료하시겠습니까? 물어보는 다이얼로그 만들고 "네"버튼 누르면 종료하기

```java
@Override
public void onBackPressed() {
    //뒤로가기 버튼을 클릭했을 때 호출되는 메서드
    //super.onBackPressed(); -> 없으면 뒤로가기 눌러도 종료가 안된다.

    AlertDialog.Builder dialog = new AlertDialog.Builder(AlertDialogActivity.this);

    dialog.setTitle("앱 제목");
    dialog.setMessage("종료하시겠습니까?");

    dialog.setPositiveButton("넵", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            finish();//전체 액티비티 종료
        }
    });
```

# Intent
- 안드로이드의 애플리케이션 구성은 4대 컴포넌트로 이루어져있다.
    - 액티비티
    - 서비스
    - 브로드캐스트 리시버
    - 컨텐트 프로바이더
- Intent는 각가의 컴포넌트간의 통신을 맡고 있습니다.

## Intent의 통신방법
- Intent의 통신 방법은 두가지가 있습니다.
    1. 명시적 Intent
    2. 암시적 Intent
 
## 명시적 Intent
- 명시적 Intent는 가장 많이 볼 수 있는 방법이다. 바로 앱의 화면전환을 하는 방법입니다.
- 하나의 액티비티에서 다른 액티비티로의 화면 전환시 사용하는 것
```java
Intent intent = new Intent(A_Activity.this, B_Activity.class);
startActivityForResult(intent, 100);
```

## 암시적 인텐트
- 암시적 Intent는 Intent의 Action에 따라 해당하는 적합한 어플리케이션의 클래스를 호출한다.
- 웹브라우저 호출, 이메일 전송 전화앱으로의 통화 등이 해당한다.
```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://m.naver.com"));
startActivity(intent);
```

## layout_intent디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".IntentActivity">

    <!--전화 거는 페이지로 이동-->
    <Button
        android:id="@+id/btn_call"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="call" />

    <!--문자 보내는 페이지로 이동-->
    <Button
        android:id="@+id/btn_sms"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="sms" />

    <!--내장 카메라로 연결-->
    <Button
        android:id="@+id/btn_camera"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="camera" />


    <!--갤러리로 이동 근데 잘 안될 수도 있음-->
    <Button
        android:id="@+id/btn_gallery"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="gallery" />
  

    <Button
        android:id="@+id/btn_link"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="link" />

    <Button
        android:id="@+id/btn_next"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="next" />

</LinearLayout>
```

## IntentActivity 버튼 등록하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class IntentActivity extends AppCompatActivity {

    Button btn_next, btn_gallery, btn_camera, btn_sms, btn_call, btn_link;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent);

        btn_call = findViewById(R.id.btn_call);
        btn_camera = findViewById(R.id.btn_camera);
        btn_gallery = findViewById(R.id.btn_gallery);
        btn_link = findViewById(R.id.btn_link);
        btn_next = findViewById(R.id.btn_next);
        btn_sms = findViewById(R.id.btn_sms);

        btn_call.setOnClickListener(click);
        btn_camera.setOnClickListener(click);
        btn_gallery.setOnClickListener(click);
        btn_link.setOnClickListener(click);
        btn_next.setOnClickListener(click);
        btn_sms.setOnClickListener(click);
    }//onCreate

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //화면 전환을 위해 반드시 필요한 클래스
            Intent i = null;

            int id = view.getId();
            if (id == R.id.btn_call) {
                //다이얼 화면으로 전환
                //Intent(Intent.ACTION_DIAL) 목적지를 정해준다.
                //i = new Intent(Intent.ACTION_DIAL);
                //i.setData(Uri.parse("tel:010-1111-1111"));
                //uri 라고 하는 걸 파라미터로 받는데 android.net 패키지에 있는 Uri를 써준다.
                //tel:를 보고 전화번호구나를 판단한다.
                // formatter 같이 특정한 키워드를 찾아서 분석을 해서 전화받는 화면으로 가야되는지 문자 받는 화면으로 가야되는지 인식을 해준다.
                //startActivity(i);//화면 전환을 위한 메서드

                위 코드를 실행해보면 현재 액티비티위에 다이얼액티비티가 얹혀 있는 상태이다.
                뒤로가기를 누르면 다이얼 액티비티만 종료되고 원래 페이지로 돌아온다.
                그리고 화면만 바꿔주는 것도 있지만 전화를 아예 걸어주는 것도 있다.
                이거는 권한이 있어야 한다 보안적으로 중요하기 때문에..
                지금은 번호를 보고 내가 모르는 번호라면 거를 수 있는데 전화를 자동으로 걸어버리면 앱이 나도 모르는 사이에 국제전화 같은거 얼어서 통화료가 어마무시하게 나올 수 있다.

                //전화를 즉시 걸어주는 작업
                i = new Intent(Intent.ACTION_CALL);
                i.setData(Uri.parse("tel:010-111-1111"));
                startActivity(i);

                //위 코드 실행시 권한이 없기 때문에 전화가 걸리지 않는걸 볼 수 있다.
                // CALL_PHONE이라는 권한이 등록이 되어야 한다. 권한은 매니패스트에서 등록할 수 있다.

                
            } else if (id == R.id.btn_camera) {
            } else if (id == R.id.btn_gallery) {
            } else if (id == R.id.btn_link) {
            } else if (id == R.id.btn_next) {
            } else if (id == R.id.btn_sms) {
            } else if (id == R.id.btn_call) {
            }
        }

    };
}

```

## Manifest에 권한 추가하기
```xml
<uses-feature android:name="android.permission.CALL_PHONE"  android:required="false"/>
```
