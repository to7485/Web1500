# Intent Flag
- 두가지 액티비티를 이동하는걸 봤는데 액티비티가 겹치기 때문에 다시 돌아올 때는 intent를 사용하지 않았었다.
- Intent Flag를 사용하면 액티비티가 겹치는 문제를 해결할 수 있다.

## IntentFlag를 언제 사용하는가?
- 안드로이드에서 Activity를 호출하다보면 발생하는 Activity의 중복문제나 흐름을 제어해주고 싶을 때 Flag를 사용합니다.

## Task
- Task는 어플리케이션에서 실행되는 액티비티를 기록하는 스택이다.
- 안드로이드에서는 Task를 이용해서 화면의 순서와 흐름을 관리할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/4abb0a4e-6d83-48d3-a451-ce5f3bd3ef73)

## IntentFlag 사용하는법
### AndroidManifest에서 제어

![image](https://github.com/to7485/Web1500/assets/54658614/7f71e540-a0b6-4087-9ee5-6e0651f71277)

  - \<activity android:launchMode= "사용하고자 하는 속성"\></> 
    -  standard : 이 속성은 별도의 Task를 생성하지 않고 해당 Task에 계속 쌓아나갑니다.<br>(Default값은 standard입니다.)
    -  singleTop : 이 Flag를 Activity에 설정하면 Task의 Top에 생성하려는 Activity가 존재하는 경우<br> 새로운 Activity를 Top에 올리지않고 기존의 Activity를 재사용합니다.<br>(여기서 재사용이란 Activity가 재사용되서 활성화가 될때 Activity의 Instance가 생성되는 것이 아니라<br> 단지 하나의 B Instance가 onPause() -> onNewIntent() -> onResume() 생명주기만을 반복하는 것을 의미합니다.)
      -  singleTask : RootActivity로만 존재하며 하나의 인스턴스만 생성 가능합니다.<br>(다른 Task에서도 동일한 Activity 실행 불가) but, 다른 액티비티 실행시 동일 Task내에서 실행 가능합니다.
      -  singleInstance : 루트 액티비티로만 존재하며 하나의 인스턴스만 생성가능하고 태스크내에 해당 액티비티 하나만 속할 수 있어 다른 액티비티를 실행시키면 새로운 Task가 생성되어 (FLAG_ACTIVITY_NEW_TASK와 동일) 그 Task내에 포함됩니다.
   
### 소스코드 Flag 제어
  - Intent.addFlags() : 새로운 flag를 기존 flag에 붙임
  - Intent.setFlags() : 오래된 flag 전체를 대체
### 소스코드 Flag에 들어가는 매개변수
- FLAG_ACTIVITY_BROUGHT_TO_FRONT
  - 시스템 디폴트값, 같은 Task에 Activity가 있을 경우에 Activity 실행모드가 singleTask이면 자동으로 설정됨
- FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET
  - Task가 리셋될 때 플래그가 사용된 Activity부터 위의 Activity가 모두 삭제됨
- FLAG_ACTIVITY_CLEAR_TOP
  - 호출하는 Activity가 스택에 있을 경우, 해당 Activity를 최상위로 올리면서, 그 위에 있던 Activity들을 모두 삭제하는 Flag<br>예) ABCDE가 존재하는 상태에서 C를 호출하게 되면 ABC만 남는다.
- FLAG_ACTIVITY_SINGLE_TOP
  - 호출되는 Activity가 최상위에 존재할 경우에는 해당 Activity를 다시 생성하지 않고, 존재하던 Activity를 다시 사용

## IntentSubActivity 수정하기
```java
@Override
public void onClick(View view) {
     Intent i = new Intent(IntentSubActivity.this,IntentActivity.class);

     //중복된 페이지를 걸러내는 플래그 추가
    i.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP |Intent.FLAG_ACTIVITY_SINGLE_TOP);
     startActivity(i);

}
```

![image](https://github.com/to7485/Web1500/assets/54658614/c0013d37-da77-4754-9b70-cbbb63bced55)

- C.T 속성이 지금 액티비티 상에서 가장 위에 있는 액티비티를 지워준다.
- Intent 때문에 위에 메인액티비티가 쌓이는 상황이 올 수 있다.
- S.T 속성이 액티비티가 존재할 경우 만들어진 액티비티를 다시 사용하도록 하게 해준다.
- 화면을 전환하는 곳에서 의식적으로 플래그를 쓰게되면 전환해야 하는 액티비티가 몇개든 간에 겹치는 상황을 막을 수 있다.

# Handler
- 새로운 프로젝트를 만들면 자동으로 생성되는 메인 액티비티는 앱이 실행될 때 하나의 프로세스에서 처리된다.
- 메인 액티비티 내에서 이벤트를 처리하거나 특정 메서드를 정의하여 기능을 구현할 때도 같은 프로세스 안에서 실행된다.
- 기능들이 순서대로 실행이 될 때는 문제가 없지만, 대기 시간이 길어지는 네트워크 요청 등의 기능을 수행할 때는 화면에 보이는 UI도 멈춤 상태로 있게 되는 문제가 발생할 수 있다.
- Handler는 안드로이드에서 비동기적 메세지를 처리하기 위해 사용되며 스레드간 통신을 할 수 있도록 하는 방법을 제공한다.
- 예를들어 스톱워치를 켜고 홈버튼을 눌러서 바탕화면으로 나갔다가 다시 돌아와도 시간은 흐르고 있어야 한다.

## Ex_오늘날짜 프로젝트 생성하기
- HandlerActivity로 이름 변경하기

## layout_handler 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".HandlerActivity">

    <TextView
        android:id="@+id/txt_count"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:gravity="center"
        android:text="0"
        android:textSize="50dp" />

    <Button
        android:id="@+id/btn_start"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="start" />

    <Button
        android:id="@+id/btn_stop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="stop" />

</LinearLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/9c165b7f-8330-4d8c-9477-b407837681ce)


- start버튼을 누르면 1초 간격으로 숫자가 증가 하게 할 것이다.
-  홈버튼을 눌러서 나갔다와도 시간이 흘러갈 수 있도록 만들고
-  stop을 누르면 시간을 멈추게 만들 것이다.

## HandlerActivity에 객체 등록하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class HandlerActivity extends AppCompatActivity {

    TextView txt_count;
    Button btn_start, btn_stop;
    int count = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_handler);

        txt_count = findViewById(R.id.txt_count);
        btn_start = findViewById(R.id.btn_start);
        btn_stop = findViewById(R.id.btn_stop);

        btn_start.setOnClickListener(click);
        btn_stop.setOnClickListener(click);

        //이벤트 감지자를 밖에다 만드는 이유, 안에다 만들면 더 위에있는 버튼들이 참조를 못하기 때문
    }//onCreate

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            int id = view.getId();
            if (id == R.id.btn_start) {
                //백그라운드에서txt_count값을1씩 증가시키는 핸들러 호출
            } else if (id == R.id.btn_stop) {
                //핸들러 정지			
            }
        }
    };

    //핸들러 준비
    //OS패키지에 있는 핸들러 클래스를 import 해야 한다.
    //duplicate 앞으로 업데이트의 가능성은 없으니까 이걸 대체할 수 있는 다른데 생겼을 수 있거나 
    //아니면 다른걸 조합해서 이걸 대체할 수 있는지 찾아보세요 라는 것.
    //줄 지우기 : alt + enter -> inspection -> Disable inspection
    Handler handler = new Handler(){
        
    };
}
```

- handleMessage메서드 오버라이딩 하기
- 핸들러만 썼다고 해서 백그라운드에서 동작을 하는게 아니라 handleMessage가 오버라이딩 되어 있어야한다.
- 백그라운드에서 코드를 실행하는 영역이기 때문이다.

```java
Handler handler = new Handler() {
@Override
public void handleMessage(@NonNull Message msg) {
    //백그라운드에서 코드를 실행하는 영역
    count++;
    txt_count.setText(count);
    }
};
```

- setText 메서드를 사용하는 곳은 참 많다.(TextView, EditText, Button)
- setText에 들어가는 파라미터 타입이 한번도 본적이 없는 캐릭터 시퀀스 라고 하는 타입이다.
- 문자열 형태의 데이터가 들어가면 캐릭터배열로 바꿔서 문제가 없다. 그런데 정수가 들어가면 오류가 난다.
- count를 문자열로 바꿔주자.

```java
txt_count.setText(String.valueOf(count));
```

## HandleMessage메서드 호출하기
- 만들어 놓은 handler.handleMessage() 이렇게 호출을 하면 백그라운드에서는 동작하지 않고 일반메서드처럼 동작을 한다.
- 돌아가는 동안에는 클릭도 터치도 아무것도 안된다. 백그라운드에서 돌아가게 호출을 해야한다.

```java
@Override
public void onClick(View view) {
    int id = view.getId();
    if (id == R.id.btn_start) {
        //백그라운드에서txt_count값을1씩 증가시키는 핸들러 호출

        //핸들러의 handleMessage()메서드를 호출하는 방법
        handler.sendEmptyMessage(0);

    } else if (id == R.id.btn_stop) {
        //핸들러 정지
    }
}
```

- 에뮬레이터를 켜고 실행을 해보면 한번 증가하고 끝나는걸 볼 수 있다.
- while문을 썼다가는 while문이 도는 동안 아무것도 못할 수 있다.
- 호출을 했을 때 여러번 실행되게 해야한다.

## handler에 코드 추가하기
```java
Handler handler = new Handler() {
    @Override
    public void handleMessage(@NonNull Message msg) {
        //백그라운드에서 코드를 실행하는 영역
        //handler.sendEmptyMessage() //본인을 다시 실행하는데 1초에 1000번씩 돌아버린다.
        handler.sendEmptyMessageDelayed(0,1000);

        //1초 쉬고 메시지를 호출하는 코드인데 아래의 코드는 언제 동작하는가?
        //1초 쉬었다가 메시지를 호출할껀데 1초 쉬는동안 아래 코드를 작동한다.

        //버튼을 연타하면 속도가 점점 빨라진다 핸들러가 쉬지말라는걸로 생각해서 속도가 빨라진다
        // 그래서 start를 누르면 연타를 하지 못하도록 비활성화 시키는 방법이 있다.

        count++;
        txt_count.setText(String.valueOf(count));
    }
};
```

## start버튼을 누르면 다시 못누르게 만들자
```java
@Override
  public void onClick(View view) {
      int id = view.getId();
      if (id == R.id.btn_start) {
          //백그라운드에서txt_count값을1씩 증가시키는 핸들러 호출

          //핸들러의 handleMessage()메서드를 호출하는 방법
          handler.sendEmptyMessage(0);

          //버튼 비활성화
          btn_start.setEnabled(false);
      } else if (id == R.id.btn_stop) {
          //핸들러 정지

          handler.removeMessages(0);
          //버튼 활성화
          btn_start.setEnabled(true);
      }
  }
```

- what에 대해서 알아보자

## HandlerWhatActivity 생성하고 xml 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".HandlerWhatActivity">

    <Button
        android:id="@+id/btn0"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="what 0" />

    <Button
        android:id="@+id/btn1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="what1" />

</LinearLayout>
```
- 위에꺼 누르면 what을 0으로 보내고 아래버튼을 누르면 what 1로 보낼 것이다.

## HandlerWhatActivity 코드 작성하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class HandlerWhatActivity extends AppCompatActivity {

    Button btn0, btn1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_handler_what);

        btn0 = findViewById(R.id.btn0);
        btn1 = findViewById(R.id.btn1);

        btn0.setOnClickListener(click);
        btn1.setOnClickListener(click);
    }//onCreate

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            int id = view.getId();
            if (id == R.id.btn0) {
                handler.sendEmptyMessage(0);
            } else if (id == R.id.btn1) {
                handler.sendEmptyMessage(1);
            }
        }
    };

    //핸들러
    Handler handler = new Handler() {
        @Override
        public void handleMessage(Message msg) { msg가 what이라는걸 가지고 있다.
            if (msg.what == 0) {
                Toast.makeText(HandlerWhatActivity.this, "0으로 호출됨", Toast.LENGTH_SHORT).show();
            } else if (msg.what == 1) {
                Toast.makeText(HandlerWhatActivity.this, "1으로 호출됨", Toast.LENGTH_SHORT).show();
            }
        }
    };

}
```
# 파라미터 넘기기
- Intent를 이용해서 액티비티간 이동을 하면서 파라미터를 보내보자

## IntentMainActivity 생성하고 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".IntentMainActivity">

    <EditText
        android:id="@+id/edit_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="input name" />

    <EditText
        android:id="@+id/edit_age"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="age"
        android:inputType="number" />

  
    <EditText
        android:id="@+id/edit_tel"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="-없이입력"
        android:inputType="number" />-> 숫자만 입력할 수 있게 하자

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <EditText
            android:id="@+id/edit_b_day"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:enabled="false"    -> editText 비활성화, 키보드로 검색 못하게 하자
            android:hint="1980-01-01" /> -> 사용자가 형식을 안맞춰줄수도 있기 때문에 

        <Button
            android:id="@+id/btn_date"    -> 달력을 호출할 수 있게 하는 버튼
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="select" />

    </LinearLayout>

    <Button
        android:id="@+id/btn_send"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="send" />
</LinearLayout>
```
- 값을 입력하고 send버튼을 누르면 파라미터를 넘기듯이 정보를 넘겨보자

## IntentMainActivity 코드 작성하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;

public class IntentMainActivity extends AppCompatActivity {

    EditText edit_name, edit_age, edit_tel, edit_b_day;
    Button btn_date, btn_send;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_main);

        edit_name = findViewById(R.id.edit_name);
        edit_age = findViewById(R.id.edit_age);
        edit_tel = findViewById(R.id.edit_tel);
        edit_b_day = findViewById(R.id.edit_b_day);
        btn_date = findViewById(R.id.btn_date);
        btn_send = findViewById(R.id.btn_send);
    }
}
```

## IntentSubActivity 생성하고 디자인하기
- 파라미터를 받아줄 레이아웃 생성하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".IntentSubActivity">

    <TextView
        android:id="@+id/txt_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="이름: "
        android:textSize="30dp" />

    <TextView
        android:id="@+id/txt_age"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="나이:"
        android:textSize="30dp" />

    <TextView
        android:id="@+id/txt_tel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="전화:"
        android:textSize="30dp" />

    <TextView
        android:id="@+id/txt_birth"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="생일:"
        android:textSize="30dp" />

</LinearLayout>
```

## IntentSubActivity 코드작성하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class IntentSubActivity extends AppCompatActivity {

    TextView txt_name, txt_age, txt_tel, txt_birth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_sub);

        txt_name = findViewById(R.id.txt_name);
        txt_age = findViewById(R.id.txt_age);
        txt_tel = findViewById(R.id.txt_tel);
        txt_birth = findViewById(R.id.txt_birth);
    }
}
```

## IntentMainActivity로 돌아와 이벤트 감지자 만들기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class IntentMainActivity extends AppCompatActivity {

    EditText edit_name, edit_age, edit_tel, edit_b_day;
    Button btn_date, btn_send;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_main);

        edit_name = findViewById(R.id.edit_name);
        edit_age = findViewById(R.id.edit_age);
        edit_tel = findViewById(R.id.edit_tel);
        edit_b_day = findViewById(R.id.edit_b_day);
        btn_date = findViewById(R.id.btn_date);
        btn_send = findViewById(R.id.btn_send);

        btn_send.setOnClickListener(send_click);
    }

    View.OnClickListener send_click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //화면전환시 파라미터로 전달할 값들들
            String name = edit_name.getText().toString();
            if (name.trim().length() == 0) {
                Toast.makeText(IntentMainActivity.this, "이름을 입력하세요", Toast.LENGTH_SHORT).show();
            }
            String age = edit_age.getText().toString();
            String tel = edit_tel.getText().toString();

            

        }
    };
}
        
```
- 위 name, age, tel을 갖고 서브액티비티로 넘어가야 한다.
- 액티비티는 new 생성자()를 이용해서 객체로 생성할 수 없다.
- 생성이 된다고 해도 우리가 만든 서브 액티비티랑은 다른 메모리 영역에 생성된다.
- 어떤 레이아웃을 참조하고 있는지도 모를뿐더러 어떤객체를 갖고 있는지도 알 방법이 없다.

```java

... 중략

View.OnClickListener send_click = new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        //화면전환시 파라미터로 전달할 값들들
        String name = edit_name.getText().toString();
        if (name.trim().length() == 0) {
            Toast.makeText(IntentMainActivity.this, "이름을 입력하세요", Toast.LENGTH_SHORT).show();
        }
        String age = edit_age.getText().toString();
        String tel = edit_tel.getText().toString();

        Intent i = new Intent(IntentMainActivity.this, IntentSubActivity.class);

        //값 전달을 위해 intent객체에 저장
        i.putExtra("m_name", name);
        i.putExtra("m_age", age);
        i.putExtra("m_tel", tel);

        //화면 전
        startActivity(i);
    }
};

```

## IntentSubActivity에서 파라미터 받아주기

```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class IntentSubActivity extends AppCompatActivity {

    TextView txt_name, txt_age, txt_tel, txt_birth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_sub);

        Intent i = getIntent();//main에서 넘어온 intent

        //넘겨받은 intent로부터 저장된 값을 추출
        String name = i.getStringExtra("m_name");
        String age = i.getStringExtra("m_age");
        String tel = i.getStringExtra("m_tel");

        txt_name = findViewById(R.id.txt_name);
        txt_age = findViewById(R.id.txt_age);
        txt_tel = findViewById(R.id.txt_tel);
        txt_birth = findViewById(R.id.txt_birth);

        txt_name.setText("이름: " + name);
        txt_age.setText("나이: " + age);
        txt_tel.setText("전화: " + tel);
    }
}
```

- 메인에서 select 버튼을 눌렀을 때 달력이 뜨도록 해보자

## IntentMainActivity에 코드 추가하기
```java
package com.korea.ex_0711;

import androidx.appcompat.app.AppCompatActivity;

import android.app.Dialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import java.util.Calendar;

public class IntentMainActivity extends AppCompatActivity {

    EditText edit_name, edit_age, edit_tel, edit_b_day;
    Button btn_date, btn_send;
///////////////////////////
    Dialog dialog;
///////////////////////////

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_intent_main);

        edit_name = findViewById(R.id.edit_name);
        edit_age = findViewById(R.id.edit_age);
        edit_tel = findViewById(R.id.edit_tel);
        edit_b_day = findViewById(R.id.edit_b_day);
        btn_date = findViewById(R.id.btn_date);
        btn_send = findViewById(R.id.btn_send);

        btn_send.setOnClickListener(send_click);
////////////////////////////////////////////////////////
        btn_date.setOnClickListener(date_click);
///////////////////////////////////////////////////////
    }

    View.OnClickListener send_click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //화면전환시 파라미터로 전달할 값들들
            String name = edit_name.getText().toString();
            if (name.trim().length() == 0) {
                Toast.makeText(IntentMainActivity.this, "이름을 입력하세요", Toast.LENGTH_SHORT).show();
            }
            String age = edit_age.getText().toString();
            String tel = edit_tel.getText().toString();

            Intent i = new Intent(IntentMainActivity.this, IntentSubActivity.class);

            //값 전달을 위해 intent객체에 저장
            i.putExtra("m_name", name);
            i.putExtra("m_age", age);
            i.putExtra("m_tel", tel);

            //화면 전
            startActivity(i);
        }
    };
//////////////////////////////////////////////////////////////////////////////
    View.OnClickListener date_click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //달력에 최초로 표기될 오늘 날짜를 구한다.
            Calendar now = Calendar.getInstance();
            int y = now.get(Calendar.YEAR);
            int m = now.get(Calendar.MONTH);
            실제로는 1월인데 값이 0으로 넘어온다. 12월인 경우 11로 넘어온다.
            int d = now.get(Calendar.DAY_OF_MONTH)//일
            날짜는 그냥 day가 아니라 day_of_month로 표시해줘야 한다.
        }
    };
    //달력의 변경사항을 감지하는 이벤트 감지자
    DatePickerDialog.OnDateSetListener dateSetListener = new DatePickerDialog.OnDateSetListener() {
        @Override //i가 연도, i1가 월, i2가 일 -> y,m,d로 바꿔주자.
        public void onDateSet(DatePicker datePicker, int y, int m, int d) {

        }
    };



//////////////////////////////////////////////////////////////////////////////
}

```

- dialog 객체 생성하기

```java
View.OnClickListener date_click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            //달력에 최초로 표기될 오늘 날짜를 구한다.
            Calendar now = Calendar.getInstance();
            int y = now.get(Calendar.YEAR);
            int m = now.get(Calendar.MONTH);
            int d = now.get(Calendar.DAY_OF_MONTH);//일
                                                                    //감지자      연,월,일
            dialog = new DatePickerDialog(IntentMainActivity.this,dateSetListener,y,m,d);

            dialog.show();

        }
    };
```

- 달력을 켜서 확인하면 오늘 날짜가 나오는데 7월 이라고 나오지만 값은 6으로 저장되어 있다.
- 나중에 내가 원하는 달로 사용하려면 +1을 해줘야 한다.

```java
DatePickerDialog.OnDateSetListener dateSetListener = new DatePickerDialog.OnDateSetListener() {
        @Override //i가 연도, i1가 월, i2가 일 -> y,m,d로 바꿔주자.
        public void onDateSet(DatePicker datePicker, int y, int m, int d) {
            //파라미터 값중 월을 의미하는m은	
            // 1월-> 0 2월 0 -> 1....	
            // 날짜형식 지정1980-01-01
            String result = String.format("%d-%02d-%02d",y,m+1,d);
          	edit_b_day.setText(result);
        }
    };
```
- 생일도 파라미터로 묶어서 보내기

```java
String age = edit_age.getText().toString();
String tel = edit_tel.getText().toString();
String birth = edit_b_day.getText().toString();

Intent i = new Intent(IntentMainActivity.this, IntentSubActivity.class);

//값 전달을 위해 intent객체에 저장
i.putExtra("m_name", name);
i.putExtra("m_age", age);
i.putExtra("m_tel", tel);
i.putExtra("m_birth",birth);
```
- sub에서 받아주기
```java
Intent i = getIntent();//main에서 넘어온 intent
String name = i.getStringExtra("m_name");
String age = i.getStringExtra("m_age");
String tel = i.getStringExtra("m_tel");
String birth = i.getStringExtra("m_birth");

txt_name = findViewById(R.id.txt_name);
txt_age = findViewById(R.id.txt_age);
txt_tel = findViewById(R.id.txt_tel);
txt_birth = findViewById(R.id.txt_birth);

txt_name.setText("이름: " + name);
txt_age.setText("나이: " + age);
txt_tel.setText("전화: " + tel);
txt_birth.setText("생일: " + birth);
```


- 날짜를 선택하면 바뀌는걸 볼수 있는데 텍스트 색깔이 좀 아쉽다
- 레아이웃으로 이동해서 날짜 EditText에 textColor를 검은색으로 바꿔주면 날짜를 선택하면 검은색으로 바뀐다.

![image](https://github.com/to7485/Web1500/assets/54658614/7a3274e4-79be-40ac-99f6-45403a004072)

# 핸들러 실습
- 앱을 하나 켜고 뒤로가기를 눌렀을 때 '뒤로가기를 한번 더 눌러야 종료가 됩니다.' 하고 토스트가 뜬다.
- 토스트가 떴을 때 3초정도 있다가 다시 누르면 두 번 누른걸로 인정하지 않아서 토스트가 다시 뜬다.
- 핸들러를 이용해서 만들어보자.

```
    @Override
    public void onBackPressed(){
        //super.onBackPressed() -> super 가 있으면 강제로 종료된다.
        if(num != 2){
            finish();
        } else{
            Toast.makeText(IntentMainActivity.this,"한번 더 누르면 종료됩니다.",Toast.LENGTH_SHORT).show();
            //핸들러 호출
            handler.sendEmptyMessage(0);
        }

    }

    Handler handler = new Handler(){
        @Override
        public void handleMessage(Message msg){
            //2초 카운팅을 위한 핸들러 현재 num = 2 즉 2초에서 시작을 하고 1초에 1씩 감소하는 핸들러, 감소하다가 0이 되면 else로 들어가서 다시 2로 바꿈
            handler.sendEmptyMessageDelayed(0,1000);
            if(num > 0){
                --num;
            } else {
                num = 2;
                handler.removeMessages(0); // 없으면 2,1,0 2,1,0 계속 반복하게 된다.
            }
        }
    };
}
```


