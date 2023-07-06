# Manifest
- Manifest.xml 파일은 프로젝트의 src/main에 위치하고 있습니다.
- 응용프로그램은 구성 요소를 시작하기전에 Manifest를 읽어서 생성이 되는데, 각종 구성요소의 정보들 및 선언들이 담겨 있다.

## Manifest의 역할
1. 앱에서 요구할 사용자 권한을 정의한다.
2. 앱이 실행될 수 있는 최소 API레벨을 정의한다.
3. 앱에서 사용하는 하드웨어/소프트웨어 기능을 정의한다. ex)카메라, 블루투스
4. 앱에 링크되어야 하는 라이브러리를 선언한다.

## manifest.xml의 구성

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Ex_0705"
        tools:targetApi="31">
        <activity
            android:name=".LinearActivity"
            android:exported="true"> -> intent-filter 속성을 갖고 있으면 True로 바꿔줘야 함
            <intent-filter> -> 첫 번째로 실행할 가치가 있는, 가능성이 있는 애플리케이션 이다 라는 속성
                            여러 가지 액티비티가 intent-filter를 가지고 있다면 가장 먼저 만들어진 요소에서 실행이 된다.
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            
        </activity>  -> intent-filter 속성을 넣었으면 액티비티를 닫아주는 태그를 작성해주자
        <activity
            android:name=".FirstActivity"
            android:exported="true"> true로 유지되고 있어도 상관 없다.
        </activity>
    </application>

</manifest>
```

# FrameLayout
- FrameActivity 생성하기
- manifest.xml에 intent-filter 옮기기
```xml
<activity
    android:name=".FrameActivity"
    android:exported="true" >
<intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
</activity>
```

## activity_frame.xml 로 이동해서 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="20dp"
    tools:context=".FrameActivity">

    <TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:background="#aaf" />

    <!--프레임 레이아웃에서는 자식 객체가 아무런 속성도 추가하지 않는 다면 무조건 왼쪽 위에 가서 겹친다.-->

    <TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_margin="20dp"
        android:background="#000" />


    <TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_gravity="right"
        android:background="#f00" />

    <!--나중에 만든 요소가 먼저 만든 요소를 잡아먹는다더 위에 올라온다.
        완전히 겹칠게 아니라면 마진이라는 속성을 넣어보자
        html이 아니기 때문에 동시에 margin 20dp 20dp 이렇게줄 수는 없다.
        팀장님이 오른쪽 끝에 가로 세로 100dp 짜리 네모를 만들라고 했어요.
        android:layout_marginLeft="310dp" 값이 100을 넘어가 버리는 순간 오차가 생긴다.
        margin이 아닌 layout_gravity라는 속성을 사용해보자.-->

    <!-- layout_gravitiy : 현재 객체 위치를 통째로 변경하는 속성
        어떤 휴대폰에서 봐도 오른쪽 끝에 붙어있다.-->

    오른쪽 아래에 있는 네모를 하나 만들어보자

    <TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_gravity="bottom|right"
        android:background="#185e5e"
        android:padding="15dp"
        android:text="hi"
        android:textColor="#fff"
        android:textSize="25dp" />

    <!--최대 두개의 속성을 한 영역에 줄 수 있다.layout_gravity 자체를 두 번 쓰는 것은 불가능하다.-->
    <!--html에서 padding을 주면 요소의 크기가 실제 크기보다 늘어나지만 안드로이드는 그렇지 않다.-->
    <!--html에서 margin값이 겹치면 큰 값을 기준으로 사용한다. 안드로이드에서는 두개를 합한 값으로 사용한다.-->

    <TextView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_gravity="bottom"
        android:background="#000"
        android:gravity="center"
        android:text="hi"
        android:textColor="#fff" />

   <!-- gravity는 객체 내부에 있는 요소들의 위치를 변경-->

</FrameLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/f61a1b76-8308-4ab4-8d22-d3115604355d)

## 실습문제
- 화면가운데에 다음과 같은 요소 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/e9bc98b9-9e3a-4925-9a46-039d70b9b96d)

```xml
    <TextView
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:layout_gravity="center"
        android:background="#f00" />

    <TextView
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_gravity="center"
        android:background="#fff"
        android:gravity="center"
        android:text="A"
        android:textSize="30dp" />
```

# RelativeLayout
- 방향성을 가지고 있지는 않다.
- 위치를 결정할 때 상대적인 방법으로 결정한다.
- ex) xx를 기준으로 오른쪽에 요소를 두고 싶다.
- 요소를 배치할 때 기준을 둘 무엇인가가 필요하다.

## RelativeActivity생성하기
- AndroidManifest.xml 수정하기

```xml
    <activity
        android:name=".RelativeActivity"
        android:exported="true">

        <intent-filter>

            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
```

## layout_relative.xml에 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".RelativeActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" 방향성이 없기 때문에 왼쪽 위에 생성된다.
        android:text="URL"
        android:textSize="40dp" />

    <EditText
        android:layout_width="200dp"
        android:layout_height="wrap_content" />

    너비를 wrap_content로 잡으면 글씨를 쓰기 전까지 딱 붙어서 보기 좋지 않다.

    relative는 반드시 상대적으로 기준이 되는객체가 반드시 필요합니다.
    그런데 TextView가 여러개 있을 수 있다.
    기준이 되는 객체의 존재를 인식할 수 있도록 이름을 붙혀주자. 

</RelativeLayout>
```

## id만들어주기
- layout_relative.xml 수정하기
```xml
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="URL"
        android:textSize="40dp"
        android:id="@+id/tv"/> 하나의 레이아웃에서는 id가 중복되게 만들 수 없다.

    <EditText
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:id="@+id/et"/>
```
EditText가 tv라고 하는 객체의 오른쪽에 가고싶다고 요청을 할 수 있다.<br>
아쉽게도 Relative레이아웃에는 layout_gravity 속성이 없다.

layout_toRightOf을 사용한다.

```xml
<EditText
    android:layout_width="200dp"
    android:layout_height="wrap_content"
    android:id="@+id/et"
    android:layout_toRightOf="@id/tv"/>
```
@+id 는 없는 객체에 새로 이름을 붙힐 때<br>
@id 는 이미 있는 객체의 이름을 참조할 때 사용한다.<br>

![image](https://github.com/to7485/Web1500/assets/54658614/b6e8c8c7-8e61-46b4-b1b0-1461e9d034e6)


### 버튼을 하나 만들어보자 EditText가 끝나는 지점 밑에다 배치를 하려고 한다.

```xml
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="ok" 처음에는 그냥 요소가 겹쳐서 생성된다.
    android:layout_below="@id/et" editText밑에만 있으면 된다고 생각한다.
    android:layout_alignRight="@id/et"/> 라인을 맞추겠다는 속성
```

### 버튼을 만들어서 바닥에 배치를 해보자.
```xml
<Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_alignParentBottom="true"
    android:text="bottom" />
```
화면 밑에 배치를 하고 싶다면 relative 레이아웃은 기준을 잡고 밑으로 보내야 한다.<br>
EditText나 Button 밑으로 보내면 요소 바로 밑에 붙어버릴 것이다.<br>
기준을 크게 잡고 부모요소의 바닥과의 라인을 맞출것이다.<br>
relative레이아웃은 속성을 많이 알고 있어야 유리한 경우가 많다.<br>

### 화면 가운데 이미지 넣기
```xml
<ImageView
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:src="@mipmap/ic_launcher_round"
    android:layout_centerInParent="true"/>
    이미지를 가운데로 보내보자 딱히 기준으로 둘 게 없다. 부모를 기준으로 잡아보자.
```

경로는 똑같이 src 속성으로 관리한다.<br>
이미지는 drawable 폴더에 넣고 관리를 하지만 안에 폴더를 하나 더 만들면 인식을 못한다는 단점이 있다.<br>

@mipmap/ic_launcher_round -> 내 해상도에 맞는 아이콘이 알아서 들어온다.<br>

mipmap 폴더에는 에뮬레이터에서 보여줄 아이콘들이 들어있다.<br>
폴더가 여러개 있는데 해상도 별로 보관이 되어 있다.<br>
우리가 사용하는 해상도 1080*1920은 xhdpi 정도에 보관이 되고 있다.<br>

만약 다른 폴더에 이미지가 없고 현재 해상도에 해당하는 폴더에만 아이콘이 있는데<br>
후진 해상도의 핸드폰으로 작동을 한다면 어쩔수 없이 현재 해상도 폴더에 있는 아이콘을 호출한다.<br>

## Weight 속성
- weight은 view에 가중치를 주는 속성으로 각각의 컴포넌트들의 크기를 조절해줍니다.
- 레이아웃 디자인을 할 때 가장 많이 사용되는 속성이라고 해도 과언이 아니다.

## WeightActivity 생성하기
- manifest 수정하기

## layout_weight.xml 디자인하기
- weight 속성 같은 경우 방향성이 없는 레이아웃에서는 사용하기 곤란하기 때문에 LinearLayout을 사용하자.
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".WeightActivity">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="top" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#aaf"
        android:orientation="horizontal">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn1" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:text="btn2" />

        기본테마의 단점 버튼에 마진이 없음
    </LinearLayout>

</LinearLayout>
```

버튼 두 개만 가운데로 보내기

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="#aaf"
    android:orientation="horizontal"
    android:gravity="center"> -> gravity 속성 사용하기
```

버튼 두개로 공간을 채우고 싶다.

```xml
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn1"
            android:layout_weight="1"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:text="btn2"
            android:layout_weight="1"/>
```

리니어 레이아웃 안에서 weight 속성을 갖고 있는 객체들 끼리 비율을 나눠 갖는다.<br>
바깥에 리니어 레이아웃을 하나 더만들고 weight속성을 준다.<br>

```xml
...중략

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_weight="1"
    android:background="#8ac"
    android:orientation="horizontal">

</LinearLayout>

위에 weight과는 비교 할 수 없다. 같은 선상에 있는 weight과 만 비교할 수 있다.

높이를 match로 잡아버리면 공간을 다 차지하긴 하지만 밑에 요소들이 차지할 자리가 없어진다.
아래에 요소를 배치할 경우가 있을수도 있기 때문에 weight으로 주게 되면 아래에 요소가 들어갈
최소한의 자리를 준다.

<Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

## 액티비티 실습
- FormActivity 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/82a79e61-d19f-41d3-96c0-2815fdfefba8)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".FormActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Name :"
            android:textSize="40dp" />

        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="inputname"
            android:inputType="text" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Marry :
    "
            android:textSize="40dp" />

        <CheckBox
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="결혼했음" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Gender :
    "
            android:textSize="40dp" />

        <RadioGroup
            android:layout_width="wrap_content"
            android:layout_height="wrap_content">

            <RadioButton
                android:id="@+id/r1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:checked="true"
                android:text="female" />

            <RadioButton
                android:id="@+id/r2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="male" />
        </RadioGroup>
    </LinearLayout>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1">

        <ImageView
            android:layout_width="300dp"
            android:layout_height="300dp"
            android:layout_gravity="center"
            android:src="@mipmap/ic_launcher_round" />
    </FrameLayout>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="send"
        android:background="#f00"/> -> 기본테마가 적용이 되어있어서 색깔이 안바뀐다.

</LinearLayout>
```

## 테마 변경하기
- 기본 테마 색깔이 마음에 들지 않아서 변경해보자

![image](https://github.com/to7485/Web1500/assets/54658614/e7a549a4-d0fc-4428-96be-810eb0403a80)

- 테마를 담당하고 있는 코드

![image](https://github.com/to7485/Web1500/assets/54658614/2c9e1c36-b53e-4b16-bd8b-05990cb21815)

```xml
   <resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->           AppCompat.Light.DarkActionBar로 수정하기
    <style name="Base.Theme.Ex_0705" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->

        <!--Status Bar 배경색-->
        <item name="android:statusBarColor" tools:targetApi="l">#F00</item>

        <!--Status Bar 글자색깔 true 블랙 false 화이트-->
        <item name="android:windowLightStatusBar">false</item>

        <!-- ActionBar 숨기기 true, 보이기 false-->
        <item name="windowNoTitle">true</item>

    </style>

    <style name="Theme.Ex_0705" parent="Base.Theme.Ex_0705" />
</resources>
```

![image](https://github.com/to7485/Web1500/assets/54658614/f2eff0c1-8c7c-48fe-8e87-c7a0f102b9c1)

# 액티비티의 생명주기
- 액티비티는 UI와 가장 밀접한 관련을 가지고 있기 때문에 사실상 안드로이드 앱에 있어서 가장 기본이 되는 구성 요소이다.
- 보통 앱은 하나 이상의 액티비티가 서로 연결된 형태로 구성된다.
- 액티비티는 생명주기를 갖는다.

Ex)카카오톡을 사용하다가 유튜브 영상을 보기 위해 유튜브 앱을 실행하면 카카오톡 앱 화면은 더이상 보이지 않고 유튜브 앱 화면이 보인다.
이 때 카카오톡과 유튜브의 액티비티는 각자의 생명주기에 따라 호출되는 함수들이 있다.

## 생명주기 함수
- 생명주기를 쉽게 이해하려면 실제 화면에 표시 유무를 생각하면 된다.
- onCreate() : Activity가 생성되면 가장 먼저 호출됨
    - 최초로 앱을 실행하면 호출된다.
    - 생명주기 통틀어서 단 한 번만 수행되는 메소드
    - Activity 최초 실행에 해야하는 작업을 수행하기에 적합함
    - 화면 Layout 정의, View 생성, Databinding 등은 이곳에 구현함

- onStart() : 이 시점부터 사용자가 액티비티를 볼 수 있다.
    - Activity가 화면에 표시되기 직전에 호출됨
    - 화면에 진입할 때마다 실행되어야 하는 작업을 이곳에 구현함

- onResume() : 현재 Activity가 사용자에게 포커스인 되어있는 상태
    - 현재 Activity가 사용자에게 포커스인 되어있는 상태

- onPause() : 포커스를 잃은 상태가 되면 호출된다.
    - 액티비티가 실행 중인 상태에서 사용자와 상호작용이 불가능한 상태
    - 다른 Activity가 호출되기 전에 실행되기 때문에 무거운 작업을 수행하지 않도록 주의해야함
    - 영구적인 Data는 이곳에 저장

- onStop() : Activity가 다른 Activity에 의해 100% 가려질 때 호출되는 메소드
    - 홈 키를 누르는 경우, 다른 액티비티로의 이동이 있는 경우가 있음
    - 이 상태에서 Activity가 호출되면, onRestart() 메소드가 호출됨
  
- onDestory() : Activity가 완전히 종료되었을 때 호출되는 메소드
    - finish(), onBackPressed() 가 호출되면 호출
    - 메모리부족(프로세스 종료)일때 종료
    - onStop(), onDestroy() 메소드는 메모리 부족이 발생하면 스킵될 수 있음
 
- onRestart() : onStop()이 호출된 이후에 다시 기존 Activity로 돌아오는 경우에 호출되는 메소드
    - onRestart()가 호출된 이후 이어서 onStart()가 호출됨

![image](https://github.com/to7485/Web1500/assets/54658614/7ce434f2-b1c4-4198-aecf-cd372dfb11e4)

## LifeCycleActivity 만들기
```java
package com.korea.ex_0705;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

public class LifeCycleActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_life_cycle);

        //확인하고 싶은 정보를 눈으로 보고싶다면Log를 사용해야한다.
        //자동완성으로 하면import가 되지만 직접 작성하면 알트+엔터로import 해줘야 한다.
        Log.i("MY", "--onCreate--");
        //tag,msg는 작성하면 자동으로 나오는 것으로 직접 쓸 필요 없다.
        //실행을 하고 아래logcat을 누르면 여러가지 정보가 나오는데 볼륨을 조절한다던지,
        //블루투스를 연결한다던지, usb에 연결을 한다든지 하는 모든 정보가 기록이 되는데
        //우리가 원하는 것만 보려면filter에MY를 입력해보자

    }//맨처음에는 반드시oncreate()라고 하는 함수가 실행이 된다.

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.i("MY", "--onDestroy--");
    }

    @Override
    protected
    void onPause() {
        super.onPause();
        Log.i("MY", "--onPause--");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        //context:화면 제어권자
        //Toast.makeText(액티비티.this,"재실행", Toast.LENGTH_SHORT).show();
        Log.i("MY", "--onRestart--");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i("MY", "--onResume--");
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.i("MY","--onStart--");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i("MY","--onStop--");
    }
}
```
