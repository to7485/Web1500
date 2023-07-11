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



