# Visibility 속성
- 사용자와의 상호작용을 통해 View를 숨기거나 표시하고 싶을 때가 있다.
- Visibility 속성으로 이를 해결할 수 있다.
- visibility는 View 클래스에 정의되어 있으며 모든 View를 대상으로 사용할 수 있다.

## Visibility 속성의 종류
- visible : 해당 View가 보이는 상태이다.
- invisible : 해당 View가 보이지 않는 상태이다.
  - 숨겨진 상태이지만, 해당 공간을 차지하고 있다.
- gone : 해당 View가 보이지 않는 상태이다.
  - View가 차지하던 공간도 같이 숨겨진다.
 
## Ex_날짜 프로젝트 생성하기

## 이름변경 VisibleActivity
- layout_visible 디자인하기
- 안드로이드 리소스 폴더에서 이미지 두 개를 복사해서 drawable 폴더에 붙혀넣기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".VisibleActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn_back1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="back1" />

        <Button
            android:id="@+id/btn_back2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="back2" />

        <Button
            android:id="@+id/btn_bottom"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="bottom" />
    </LinearLayout>
</LinearLayout>
```

- 이미지를 일부러 겹쳐놓고 지금 당장 보여질 필요가 없는애를 숨겨놓고 시작을 할 것이다.
- 이미지를 겹쳐야 하기 때문에 Frame 레이아웃이나 Relative 레이아웃을 사용할 것이다.

```xml
... 중략

<FrameLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:background="#aaf">

이미지를 drawable 폴더에서 가져올 때 참조할 폴더앞에 @를 붙힌다.

  <ImageView
            android:id="@+id/img_back1"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scaleType="fitXY"
            android:src="@drawable/gunship" />

현재 건쉽의 이미지는 정사각형 이지만 레이아웃은 직사각형이라 억지로 맞춰놓은 상태이다.
scaleType="fitXY"를 사용하면 화면에 딱 맞춰준다. 위아래를 억지로 늘려놓음

여기서 처음에는 비행기 사진이 보이게 하고 싶다. 그래서 로테이션 그림을 숨김처리를 해줘야 한다.

        <ImageView
            android:id="@+id/img_back2"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:src="@drawable/menu_rotate_img"
            android:visibility="invisible" />

</FrameLayout>

프레임레이아웃을 나와서 밑에 버튼을 하나 더 만들어보자
visibility 속성은 기본적으로는 visible로 되어있고 invisible과 gone을 선택하면 사라진다.
하얀색으로 빈공간이 있으면 이쁘지 않기 때문에 gone 속성으로 두자!

    <Button
        android:id="@+id/bot"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:visibility="gone" />
   <!-- android:visibility="invisible"-->

```

## Visible액티비티에 이벤트 처리하기
```java
package com.korea.ex_0709;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.Button;
import android.widget.ImageView;

public class VisibleActivity extends AppCompatActivity {

    Button btn_back1, btn_back2, btn_bottom, bot;
    ImageView img_back1, img_back2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_visible);

        btn_back1 = findViewById(R.id.btn_back1); -> 누르면 비행기 이미지 보이게
        btn_back2 = findViewById(R.id.btn_back2); -> 누르면 동그란 이미지 보이게
        btn_bottom = findViewById(R.id.btn_bottom); -> 밑에 있는 버튼이 보였다 안보였다 하게

        bot = findViewById(R.id.bot);
        img_back1 = findViewById(R.id.img_back1);
        img_back2 = findViewById(R.id.img_back2);

        실제로 클릭을 유도할 버튼은 위 세 개 밖에 없다.
        위 세 개 버튼의 이벤트 처리를 위한 감지자를 하나 만들어 주자.

        btn_back1.setOnClickListener(click);
        btn_back2.setOnClickListener(click);
        btn_bottom.setOnClickListener(click);
    }
}
```
### 감지자로 처리하는법 세가지
1. id로 비교하기
2. view를 자식클래스로 캐스팅 해버리기
3. 인스턴스가 같기 때문에 그냥 비교해버리기

- onCreate 밖에 감지자 만들어주기

```java
View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            if (btn_back1 == view) {
                //비행기를 보여주고 동그라미를 숨긴다.
                img_back1.setVisibility(View.VISIBLE);
                img_back2.setVisibility(View.INVISIBLE);
            } else if (btn_back2 == view) {
                //동그라미를 보여주고 비행기를 숨긴다.
                img_back1.setVisibility(View.INVISIBLE);
                img_back2.setVisibility(View.VISIBLE);
            } else if (btn_bottom == view) {
                //getVisibility()속성으로 현재 객체의 상태를 가져올 수 있다.
                if (bot.getVisibility() != View.VISIBLE) {
                    bot.setVisibility(View.VISIBLE);
                } else {
                    bot.setVisibility(View.GONE);
                }
            }
        }
    };
```

## 버튼에 이미지 넣기
- 현재 버튼은 누르면 눌렀다고 티가 난다.
- 하지만 버튼에 이미지를 넣으면 눌렀다고 티가 안난다.

## EffectActivity만들기
- a_alarm,b_alarm, a_search, b_search파일을 복사해서 drawable폴더에 넣어준다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".EffectActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@drawable/b_alarm" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@drawable/b_search"/>

기본테마라서 이미지를 넣어도 제대로 출력이 안되는 걸 볼 수 있어서 테마를 바꿔주자

</LinearLayout>
```

- values -> themes 파일
```xml
<style name="Base.Theme.Ex_0709" parent="Theme.AppCompat.Light.DarkActionBar">
```

버튼에 이미지는 제대로 들어갔는데 이놈이 클릭이 됐는지 안됐는지 알 수가 없는 상황<br>
작은 이미지라면 어차피 손가락에 가려져서 상관이 없는데 큰 이미지라면 효과를 좀 줘야한다.<br>

# selector
- Selector란 View의 각 상태의 drawable을 달리하여 효과를 줄 수 있는 바업ㅂ이다.
- Background와 textColor 등에 적용할 수 있다.

## Background Selector
- android:state_pressed : 뷰가 눌렸을 때 (예, 터치나 클릭이 발생했을 때)

- 효과를 주기 위해서 설정파일을 만들어 줘야 한다.
- android:state_pressed : 뷰가 눌렸을 때 (예, 터치나 클릭이 발생했을 때)
- android:state_focused : 뷰에 포커스가 위치했을 때 (예, EditText를 입력할 수 있을 때)
- android:state_selected : 뷰를 선택했을 때 (예, 방향키로 이동하다가 선택했을 때)
- android:state_checkable : 체크 가능한 상태일 때 (예, 체크 박스를 체크할 수 있는 상태일 때)
- android:state_checked : 체크된 상태일 때 (예, 체크박스가 체크된 상태일 때)
- android:state_enabled : 사용할 수 있는 상태 일때(예, 터치나 클릭 이벤트 등을 받을 수 있는 상태일 때)

### selector 파일 생성하기
- drawable -> new -> Drawable resource File -> alarm_selector (대문자 안됨)

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">

    <!--버튼 클릭 전-->
    <item android:drawable="@drawable/b_alarm" android:state_pressed="false" />

    <!--버튼 클릭 후-->
    <item android:drawable="@drawable/a_alarm" android:state_pressed="true" />

</selector>
```

- 불편한건 요소마다 selector를 만들어줘야 한다는것이다.
- search버튼에 대한 selector파일 만들기

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/b_search" android:state_pressed="false" />
    <item android:drawable="@drawable/a_search" android:state_pressed="true" />
</selector>
```

- 레이아웃에 src를 selector로 바꿔주기

```xml
android:background="@drawable/search_selector"/>
```

- 에뮬레이터 실행해서 결과 확인하기

# 메뉴 만들기
- 보통 오른쪽 위에 점 세 개 클릭하면 나오게 하는 메뉴
- 액션바 위에 메뉴 버튼을 만들거나 팝업형태로 메뉴버튼을 만들어보는 두가지 방법으로 해보자!

## MenuActivity 생성하기
- 작업하는 레이아웃에다가 메뉴라고 하는 팝업을 얹어 놓는 형식이다.
- 팝업 안에 어떤 내용을 집어넣을 건가에 대한 참조파일이 필요하다.
- res -> Directory -> 이름 반드시 menu로 생성
- 참조파일 만들 때 menu리소스 파일을 만들겠냐고 나온다.

![image](https://github.com/to7485/Web1500/assets/54658614/b679dc6c-6fbc-4c1e-87ab-b6fe0c7e4769)

- menu 폴더 우클릭 -> Menu resource File -> my_menu 생성

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/menu1"
        android:title="앱 소개" />
    <item
        android:id="@+id/menu2"
        android:title="이메일" />
    <item
        android:id="@+id/menu3"
        android:title="종료" />
</menu>
```

- 참조파일을 만들었다고 에뮬레이터에 메뉴버튼이 자동으로 생성되지는 않는다.
- 오른쪽 위에 메뉴를 호출할 것이고, 어떤 메뉴를 호출할 것인지에 대한 오버라이딩 메서드가 필요하다.
- xml 형태의 참조파일은 view 형태로 전환되지 않으면 우리 눈으로 볼수가 없다.
- xml파일을 눈으로 보이게 만들어주는 inflate가 있다.

# inflate
- 사전적 정의로는 부풀리다,올리다의 의미를 가지고 있다.
- 안드로이드에서 inflate는 xml에 표기된 레이아웃들을 메모리에 로딩된 후 객체화 시키는 과정이다.
- layout에 다른 layout을 집어넣을수 있다는 이야기다.
- 즉, 각기 다른 화면들을 한 화면에 동적으로 띄우고 싶은 경우에 사용된다.

- Activity에서 setContentView()가 xml을 객체화시키는 inflate 동작이다.

![image](https://github.com/to7485/Web1500/assets/54658614/aee27cf8-6950-4d3a-a86f-9d2f7f477855)

- Activity가 onCreate될 때 setContentView()를 하기 때문에 바로 메모리에 객체가 올라가 있게 된다.
- 그래서 setContentView()함수 실행 후 해당 xml에 있는 UI적 요소들을 findViewById()를 가지고 검색을 할 수 있게 되는것이다.


## MenuActivity에 코드 작성하기
```java
package com.korea.ex_0709;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.Menu;

public class MenuActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);
    }


  onCreateOptionsMenu 메서드가 없으면 오른쪽 위에 ...이 표시되지 않는다.
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        
        //사용을 위한 메뉴리소스 파일(my_menu.xml)을 등록
        getMenuInflater().inflate(R.menu.my_menu,menu);

        return super.onCreateOptionsMenu(menu);
    }
}
```

- 각각의 메뉴에 이벤트 처리하기

## Toast
- Toast는 사용자 화면에 띄우는 간결한 메세지이다.
```java
Toast.makeText(Context, 메세지, 노출시간).show();
```

- context
  - 어플리케이션의 현재 상태를 갖고 있음
  - 시스템이 관리하고 있는 액티비티, 어플리케이션의 정보를 얻기 위해 사용
 
- show()
  - 실제로 화면에 띄워주는 메서드

```java

... 중략

//각 메뉴의 이벤트 처리를 위한 메서드
//매개변수 item이 어떤 메뉴가 눌러졌는지 알고 있다.
@Override
public boolean onOptionsItemSelected(@NonNull MenuItem item) {
    int itemId = item.getItemId();
    if (itemId == R.id.menu1) {
        Toast.makeText(MenuActivity.this, "앱 소개 누름", Toast.LENGTH_SHORT).show();
    } else if (itemId == R.id.menu2) {
        Toast.makeText(MenuActivity.this, "이메일 누름", Toast.LENGTH_SHORT).show();
    } else if (itemId == R.id.menu3) {
        //현재 띄워져 있는 액티비티 한 개를 종료
        finish();
    }
    return true;
};
```
- 안드로이드는 화면 이동을 할 때마다 액티비티가 아래에 쌓이는 형태이다.

![image](https://github.com/to7485/Web1500/assets/54658614/082e8b47-b555-4833-ad4a-1369c3432252)

# Popup메뉴
액션바가 사라지면 우리가 기껏 만들어 놓은 메뉴바가 의미가 없어진다
액션바가 없을 경우 메뉴를 띄울수 있는 팝업메뉴를 보자.

## themes에 액션바 없애주기
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.Ex_0709" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->

        <item name="windowNoTitle">true</item>

    </style>

    <style name="Theme.Ex_0709" parent="Base.Theme.Ex_0709" />
</resources>
```

## PopMenu 액티비티 생성하기
- activity_popup_menu.xml 디자인하기

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".PopupMenuActivity"
    android:orientation="horizontal">

   <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#aaf">
        
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="menu"
            android:layout_gravity="right"
            android:id="@+id/btn_show"/>
    </FrameLayout>

겹쳐서 표현한다기 보다는 메뉴 위치를 마음대로 조절하기 위해서 프레임 레이아웃을 쓴다.
사실 높이를 dp 값으로 주면 액션바처럼 쓸수 있지만 지금은 그렇게 까지 할 필요는 없다.

</LinearLayout>
```
- res -> menu 폴더 만들기 -> my_menu.xml 만들기

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/menu1"
        android:title="메뉴1" />
    <item
        android:id="@+id/menu2"
        android:title="메뉴2" />
    <item
        android:id="@+id/menu3"
        android:title="메뉴3" />
</menu>
```
- 자동으로 메뉴가 붙는게 아니기 때문에 액티비티로 돌아와서 처리를 해주자

## PopupMenu액티비티에 코드 작성하기
```java
package com.korea.ex_0709;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.PopupMenu;

public class PopupMenuActivity extends AppCompatActivity {

    Button btn_show;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_popup_menu);

        btn_show = findViewById(R.id.btn_show);

        btn_show.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //팝업메뉴 생성 android.widget 패키지
                PopupMenu popup = new PopupMenu(PopupMenuActivity.this,view);//anchor : 메뉴를 띄워줄 기준객체
               //PopupMenu() 괄호 안에 ctrl+spacebar를 눌러도 어떤 파라미터가 들어가는 지 알수 없다 ctrl+ p를 눌러서 보자!

                //팝업메뉴에 띄워줄 메뉴xml파일을 등록
                //inflate() xml을 view 형태로 바꿔주는 메서드
                //popup.getMenu() -> 메뉴가 들어갈 공간을 만들어주는 메서드
                getMenuInflater().inflate(R.menu.my_menu, popup.getMenu());

                popup.show();
            }
        });
    }
}
```

## 메뉴가 왼쪽 위에 나오게 해보자.
- 레이아웃에 왼쪽 위에 버튼 하나 만들기
```xml
<Button
    android:id="@+id/anchor"
    android:layout_width="1dp"
    android:layout_height="1dp"
    android:visibility="invisible" />
```

- 버튼을 눌렀을 때 메뉴가 나오기위한 anchor를 방금 만든 버튼으로 설정하면 된다.
```java
@Override
public void onClick(View view) {
    //팝업메뉴 생성 android.widget 패키지
    PopupMenu popup = new PopupMenu(PopupMenuActivity.this,anchor);//anchor : 메뉴를 띄워줄 기준객체
   //PopupMenu() 괄호 안에 ctrl+spacebar를 눌러도 어떤 파라미터가 들어가는 지 알수 없다 ctrl+ p를 눌러서 보자!

    //팝업메뉴에 띄워줄 메뉴xml파일을 등록
    //inflate() xml을 view 형태로 바꿔주는 메서드
    //popup.getMenu() -> 메뉴가 들어갈 공간을 만들어주는 메서드
    getMenuInflater().inflate(R.menu.my_menu, popup.getMenu());

    popup.show();
}
```

![image](https://github.com/to7485/Web1500/assets/54658614/5ec3742f-a39d-454d-ba5a-8ec03ebcdf21)

## 이벤트 처리하기
- 메뉴가 이미 보여진 다음에 이벤트 처리를 하면 작동을 잘 안할수도 있다.

```java
btn_show.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //팝업메뉴 생성 android.widget 패키지
                PopupMenu popup = new PopupMenu(PopupMenuActivity.this, anchor);//anchor : 메뉴를 띄워줄 기준객체
                //PopupMenu() 괄호 안에 ctrl+spacebar를 눌러도 어떤 파라미터가 들어가는 지 알수 없다 ctrl+ p를 눌러서 보자!

                //팝업메뉴에 띄워줄 메뉴xml파일을 등록
                //inflate() xml을 view 형태로 바꿔주는 메서드
                //popup.getMenu() -> 메뉴가 들어갈 공간을 만들어주는 메서드
                getMenuInflater().inflate(R.menu.my_menu, popup.getMenu());

                //팝업메뉴에 클릭 이벤트 감지자 등록
                popup.setOnMenuItemClickListener(menu_click);


                popup.show();
            }
        });
    }//onCreate()

    PopupMenu.OnMenuItemClickListener menu_click = new PopupMenu.OnMenuItemClickListener() {
        @Override
        public boolean onMenuItemClick(MenuItem menuItem) {
            int itemId = menuItem.getItemId();
            if (itemId == R.id.menu1) {
                Toast.makeText(PopupMenuActivity.this, "앱 소개 누름", Toast.LENGTH_SHORT).show();
            } else if (itemId == R.id.menu2) {
                Toast.makeText(PopupMenuActivity.this, "이메일 누름", Toast.LENGTH_SHORT).show();
            } else if (itemId == R.id.menu3) {
                //현재 띄워져 있는 액티비티 한 개를 종료
                finish();
            }
            return true;
        }
    };
```
