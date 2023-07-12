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

## 전화거는 화면으로 이동해보기
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
                //formatter 같이 특정한 키워드를 찾아서 분석을 해서 전화받는 화면으로 가야되는지 문자 받는 화면으로 가야되는지 인식을 해준다.
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

                
            } else if (id == R.id.btn_sms) {
            } else if (id == R.id.btn_camera) {
            } else if (id == R.id.btn_gallery) {
            } else if (id == R.id.btn_link) {
            } else if (id == R.id.btn_next) {
            }
        }

    };
}

```

## Manifest에 권한 추가하기
```xml
    <uses-feature
        android:name="android.hardware.telephony"
        android:required="false" />
    <uses-permission android:name="android.permission.CALL_PHONE" />
```
- 물론 권한을 쓸수 있다 없다 선택만 가능하고 그대로 실행하면 오류가 난다.

### 한국어로 설정 바꿔보기
- 앱을 나와 메뉴로 들어가고 settings으로 간다.
- system -> Languages & input -> Languages 클릭 -> Add a Language -> 한국어
- 맨 위로 올려주자.

### 권한 허용하기
- 바탕화면으로 나와 세팅 -> 앱 및 알림 선택 -> 오늘 날짜로 된 프로젝트 클릭 -> 전화 권한 온
- 앱을 사용하면서 권한이 필요하면 물어볼때가 있다.(카메라에 접근을 허용하겠습니까?, 주소록에 접근허용을 하시겠습니까?)

- 에뮬레이터를 다시 실행해보면 전화 거는 창으로 이동함 물론 실제로 거는건 아님

![image](https://github.com/to7485/Web1500/assets/54658614/5eff6edd-d96c-47bb-b859-bd54a00d91af)

## 메세지를 보내는 화면으로 이동하기
```java
else if (id == R.id.btn_sms) {
    i = new Intent(Intent.ACTION_SENDTO);
    i.setData(Uri.parse("smsto:010-222-2222"));
    //smsto라고 써야 문자를 보낸다고 인식을 하고 보내준다.

    //putExtra를 사용하면 내용을 지정해줄 수도 있다
    //putExtra를 작성하면 키와 밸류로 저장을 해야되는데 킷값은 sms_body로 고정이다.
    i.putExtra("sms_body","안녕~");
    startActivity(i);
}
```
- 에뮬레이터 실행하고 확인하기
- 전송은 되지만 한글로 입력이 안된다. 언어설정을 한글로 바꿨다고 해서 한글로 입력이 되지는 않는걸 볼 수 있다.
![image](https://github.com/to7485/Web1500/assets/54658614/c26f9737-603a-4859-a59e-3054f40b95aa)

## 카메라 화면으로 이동하기
```java
else if (id == R.id.btn_camera) {
    //내장 카메라로 연결
    //i = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    //Intent 클래스가 아니라 카메라,동영상 같은 경우 MediaStore라고 하는걸 사용해
    //내장카메라로 이동한다. 보낼 내용이 따로 없으니 startActivity(i)로 이동하자.

    shift + 드래그 -> 이동 , shitf + 방향키 -> 위 아래 방향 보기
    wasd 이동 가능

    //startActivity(i);

   //동영상 연결
    i = new Intent(MediaStore.ACTION_VIDEO_CAPTURE);
    startActivity(i);

}
```

![image](https://github.com/to7485/Web1500/assets/54658614/87cb3973-3d5a-433e-ab29-35fb7550f17b)

## 갤러리로 이동하기
```java
else if (id == R.id.btn_gallery) {
    i = new Intent(Intent.ACTION_GET_CONTENT);
    i.setType("*/*");//모든 타입을 호출할 때 사용
    startActivity(i);


}
```

## 링크로 이동하기
```java
else if (id == R.id.btn_link) {
링크를 연결해보자 버튼을 눌렀을 때 네이버로 간다던가,
구글로 간다던가 아니면 어디 홈페이지로 간다거나 웹페이지로 연결해보자.

    i = new Intent(Intent.ACTION_VIEW);
    //Action_View가 없어도 동작을 하는데 낮은 버전에서는 없으면 무조건 구글로 가는 버그가 있다.
    i.setData(Uri.parse("https://www.naver.com"));
    startActivity(i);

    에뮬레이터로 실행해서 확인을 해보자 크롬으로 확인을 할꺼고 크롬을 항상으로 지정해주자.
    로그인은 건너뛰기 해도 되고
    
    홈페이지로 전환을 하고 싶을 때는 이 방법을 쓰지만 마켓같은 곳으로 들어가기 위해서는 바로 들어가는 방법은 없다.
    우리 앱좀 평가해주실래요 했는데 예를 누르면 마켓화면으로 가야한다.
    우리가 마켓에 애플리케이션을 등록을 할 때 패키지 형태로 등록을 한다.
    마켓페이지로 가고싶다 근데 우리는 마켓에 등록을 하지 않았으니까... 진짜 들어가지지는 않을 것.

    //플레이스토어 전환
    i = new Intent(Intent.ACTION_VIEW);
    i.setData(Uri.parse("market://details?id=com.lhj.ex_0718"));
    startActivity(i);

}
```

## 우리가 만든 액티비티로 이동하기
## IntentSubActivity 생성하기
- 처음부터 Sub화면을 볼것이 아니기 때문에 intent-filter를 옮겨줄 필요가 없다.

## layout_intent_sub 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".IntentSubActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="previous"
        android:id="@+id/btn_prev"/>

</LinearLayout>
```

## IntentActivity에 코드 추가하기
```java
else if (id == R.id.btn_next) {
    //다른 액티비티로 전환하기   현재 액티비티 ,  이동할 액티비티
    i = new Intent(IntentActivity.this, IntentSubActivity.class);
    startActivity(i);
}
```

## IntentSubActivity로 이동하기
- 뒤로가기 버튼을 눌렀을 때 다시 이동하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class IntentSubActivity extends AppCompatActivity {

    Button btn_prev;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_sub);

        btn_prev = findViewById(R.id.btn_prev);

        btn_prev.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent i = new Intent(IntentSubActivity.this,IntentActivity.class);
                startActivity(i);
            }
        });
    }
}
```

![image](https://github.com/to7485/Web1500/assets/54658614/d40c7134-f196-434b-968b-b56dba0fe317)

- 서브 갔다가 돌아왔다가 서브 갔다가 다시 돌아오면 화면이 꺼지는게 아니라 쌓이는 형태가 된다
- 뒤로가기를 눌러봤자 맨 위에 화면 하나만 걷어낼 뿐이다
- 사용자 입장에서는 그냥 맨 처음 화면에서 뒤로가기를 눌렀을 뿐인데 갑자기 서브 화면이 나오면 납득을 못할 것이다.

```java
 btn_prev.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //Intent i = new Intent(IntentSubActivity.this,IntentActivity.class);
                //startActivity(i);
                finish();
            }
        });
```
- 하지만 페이지가 많아지게 되면 finish()만 가지고는 만드는데 한계가 있다.
