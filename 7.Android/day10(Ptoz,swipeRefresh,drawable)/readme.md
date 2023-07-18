# Pinch Zoom
- 라이브러리를 활용해보자.
- gradle에 추가하는법과 jar파일을 넣어서 사용하는 방법이 있다.

## Ex_날짜 프로젝트 생성하기
- PtoZActivity로 이름 수정하기
- PinchtoZoom 이라는 기능인데 두손으로 줌인 줌아웃을 하는 기능이다.
- 예전에는 사진으로 줌인 줌아웃만 해도 200~300줄 짜리 코드를 짜야 했다.
- 그거에 대해 불편함을 느낀 개발자가 라이브러리로 만들어 놓았어요.
- 사진을 조정하는거기 때문에 사진이 하나 필요하기 때문에 사진 하나를 drawable 폴더에다가 넣어주자.

## activity_ptoz 디자인하기
```xml
어차피 사진 하나만 넣을꺼기 때문에 부모 레이아웃의 orientation은 크게 중요하지 않다.
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".PtoZActivity">

ImageView 하나를 만들어서 꽉 채워주고 drawable 폴더에서 우리가 넣어준 사진을 지정해준다
그리고 id를 지정해주자.

    <ImageView
        android:id="@+id/iv_photo"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:src="@drawable/rabbit" />

</LinearLayout>
```
- 핀치투줌 두손가락으로 사진을 크게했다 작게했다 하는 기능을 사용하려면 라이브러리가 있어야 한다.
- 핀치투줌 하고 관련된 라이브러리가 photoview.jar 이다.
- 파일을 복사해서 libs 폴더에다가 붙혀넣자.

![image](https://github.com/to7485/Web1500/assets/54658614/8d58533d-4812-453a-aaa3-1788922e473c)

- libs폴더에 넣는다고 바로 사용이 가능한건 아니고 라이브러리를 우클릭 해서 add as library 를 눌러줘야 한다. 라이브러리가 등록이 되는 영역이 따로 있어서 등록을 해놔야 사용이 가능하다.

![image](https://github.com/to7485/Web1500/assets/54658614/43d87867-dacb-4525-9111-3686db1bd331)

- 스프링 부트에서 해봤던 버전관리툴인 gradle이 버전관리와 라이브러리를 관리를 따로 해준다.

![image](https://github.com/to7485/Web1500/assets/54658614/09baf98d-5a21-4488-b652-910cce2db11b)

- 스프링처럼 dependencies에 라이브러리들이 관리가 되고 있다 우리가 추가한 라이브러리가 잘 추가 되었는지 확인해보자.

![image](https://github.com/to7485/Web1500/assets/54658614/aa978487-5621-4cbb-be01-652fca544bc6)

## PtoZActivity액티비티에 코드를 작성해주자.

```java
package com.korea.ex_0718;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.ImageView;

import uk.co.senab.photoview.PhotoViewAttacher;

public class PtoZActivity extends AppCompatActivity {

    ImageView iv_photo;
    PhotoViewAttacher attacher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ptoz);

        iv_photo = findViewById(R.id.iv_photo);

        사진을 줌인 줌아웃 하는 기능은 코드가 단 두줄이면 끝난다.

        //attacher 객체 생성하고 이미지뷰를 파라미터로 준다.
        attacher = new PhotoViewAttacher(iv_photo);
        attacher.update();
    }
}
```
- 에뮬레이터를 켜서 확인을 해보자 화면 위에서 ctrl 을 누르고 마우스를 움직이면 두손가락으로 터치를 한것 처럼인식을 한다.
- 사진을 줌인 줌아웃을 해보자.

# Swiperefreshlayout
- 사용자가 수동으로 업데이트 를 요청할 수 있도록 한다.
- Swiperefreshlayout이 적용되어 있는 Activity에 수직으로 pull하면 업데이트가 트리거된다.

## SwipeRefreshActivity액티비티 생성하기
- 스와이프리프레쉬레이아웃을 사용할 수 있어야 하는데 라이브러리가 등록이 되어있지 않아서 지금 당장 사용할 수가 없다.
- build.gradle로 이동해서 라이브러리를 추가해야 한다.

```xml
dependencies {

    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.5.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation files('libs\\photoview.jar')
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
    /*당겨서 새로고침을 위한 라이브러리*/
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.1.0'
}
```
- 이제 레이아웃에서 스와이프리프레쉬레이아웃을 사용할 수 있다.

## layout 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.swiperefreshlayout.widget.SwipeRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/swipe"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SwipeRefreshActivity">

    <!--SwipeRefreshLayout의 직계자식은 ScrollView나 ListView중    한가지만 선택할 수 있다.-->
    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">        <!--ScrollView의 직계자식은 반드시 한개만 존재해야 한다.-->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:gravity="center"
                android:text="여기는 메인페이지 입니다."
                android:textSize="30dp" />
        </LinearLayout>
    </ScrollView>

</androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
```
- 부모 전체가 리프레쉬 레이아웃이 된다.
- Swiperefreshlayout의 직계 자식은 반드시 ScrollView나 ListView 둘중에 한가지만 선택할 수 있다.
- Swiperefreshlayout 영역 안에 있다면 어디에서든지 위에서 아래로 당겼을 때 로딩하는 디스크를 볼 수 있다.
- 에뮬레이터를 켜서 돌아가는걸 보면 계속 도는걸 볼 수 있다.
- 로딩이 언제끝나는지 얘는 모른다. 내가 로딩을 끝난 시점에서 지워줘야 한다.
- 없애는건 조금 이따 하고 여기에서 가능한 추가적인 기능과 디자인데 대한 공부를 해보자.

## SwipeRefreshActivity액티비티에 코드 작성하기
```java
package com.korea.ex_0718;

import androidx.appcompat.app.AppCompatActivity;
import androidx.swiperefreshlayout.widget.SwipeRefreshLayout;

import android.graphics.Color;
import android.os.Bundle;

public class SwipeRefreshActivity extends AppCompatActivity {

    SwipeRefreshLayout swipe;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_swipe_refresh);

        
        swipe = findViewById(R.id.swipe);

        //디스크의 배경색을 변경
        swipe.setProgressBackgroundColorSchemeColor(Color.parseColor("#aaaaff"));

        //디스크의 사이즈 변경(DEFAULT가 기본값)
        swipe.setSize(SwipeRefreshLayout.LARGE);

        //스케일을 바꿀건지 안바꿀껀지 어느시점까지 당겨지게 할건지 결정
        //true 디스크가 끝나는 위치와 디스크를 점점 더 크게 해주는 명령어
        //300 당겨지는 위치
        swipe.setProgressViewEndTarget(true,300);
    }
}
```

## res colors.xml에 색깔 추가해보기
- 우리 테마의 색깔을 관리하고 있기도 하고 우리가 직접 색깔을 추가할수도 있다.

```xml
<color name="color1">#92ff2f</color>
<color name="color2">#ffbc00</color>
<color name="color3">#12ffa0</color>
<color name="color4">#ff6724</color>
```
- 색깔을 4개를 추가해봤다
- Swiperefreshlayout 같은 경우에는 디스크 색깔을 제외하면 리소스 colors.xml에 설정되어 있는 리소스 파일을 가져와야 사용할 수 있다.
- colors에 정의되어 있는 이름들을 골라서 가져와야 한다. 색깔은 꼭 맞출 필요는 없다.

## SwipeRefreshActivity액티비티에 코드 작성하기
- setProgressViewEndTarget 위에 코드 추가하기
```java
 swipe.setColorSchemeResources(
                R.color.color1,R.color.color2,R.color.color3,R.color.color4);

//스케일을 바꿀건지 안바꿀껀지 어느시점까지 당겨지게 할건지 결정
//true 디스크가 끝나는 위치와 디스크를 점점 더 크게 해주는 명령어
//300 당겨지는 위치
swipe.setProgressViewEndTarget(true,300);
```
- 파라미터를 보면 variable argument (...) 라고 되어있다
- 내가 데이터를 5개를 주면 5개 크기의 배열로 만들어준다.
- 내가 데이터를 동적으로 보내주는 만큼 동적으로 할당을 한다.
- 우리는 4개가지의 색깔이 준비되어 있다.

디자인적으로 할 수 있는 부분은 사실상 끝이다.<br>
당겼다가 놓으면 언젠가 없애야 된다.<br>
로딩을 하고싶을 때만 가동을 시키고 로딩이 끝나면 사라지게 만들어야 하는데<br>
지금 우리가 서버 통신을 실제로 하고 있는게 아니기 때문에<br>
핸들러로 약 3초정도 서버랑 통신을 한다 하고 가정을 해서 당겼다가 3초 뒤에 사라지도록 만들어보자.<br>

```java
SwipeRefreshLayout.OnRefreshListener swipeListener = new SwipeRefreshLayout.OnRefreshListener() {
    @Override
    public void onRefresh() {
        //당겼다가 손을 떼는 순간 호출되는 메서드
        //만약에 서버랑 실제로 통신을 한다고 한다면
        // new Async().execute() 이런식으로 서버랑 통신하는 작업이 들어가야 하는데 지금은 서버가 없기 때문에 핸들러를 만들어서 처리해보자.
        handler.sendEmptyMessageDelayed(0,3000);//-> 3초 뒤에 로딩이 완료되었다고 가정.
    }
};

Handler handler = new Handler(){
    @Override
    public void handleMessage(@NonNull Message msg) {
        //로딩이 완료된 시점에서 디스크 제거
        swipe.setRefreshing(false);
    }
};
```
# 앱 서랍 만들기
- 앱을 눌렀을 때 왼쪽이나 오른쪽 바깥에서 안쪽으로 들어오는 메뉴
- 당겨서 들어오는 것도 가능하고 버튼을 눌러서 나오게 하는 것도 가능하다.
- drawerlayout이라고 불리는 서랍기능을 구현해보도록 하자
- 서랍은 알고 있으면 쓸모가 참 많다.

## DrawerLayoutActivity 생성하기
- 서랍이 나오기 전 화면이 필요하고 서랍의 레이아웃을 디자인 하기위한 레이아웃이 필요하다.
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".DrawerLayoutActivity">

    <!-- 서랍이 열리기 전에 보여줄 메인 레이아웃-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="메인화면"
            android:textSize="30dp" />

        <Button
            android:id="@+id/btn_open"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="서랍열기" />
    </LinearLayout>
    <!-- 서랍이 열리기 전에 보여줄 메인 레이아웃-->

    <!--서랍레이아웃을 만들껀다 math로 만들어버리면 꽉 채워 버린다 2/3 정도 차지하게 만들어보자.-->

    <!--서랍 레이아웃-->
    <LinearLayout
        android:id="@+id/drawer"
        android:layout_width="300dp"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:background="#8ac"
        android:orientation="vertical">

       //android:layout_gravity="start" -> 왼쪽에서 오른쪽으로
	   //android:layout_gravity="end" -> 오른쪽 에서 왼쪽으로

drawerlayout은 겹쳐서 표현을 하기 때문에서랍이 안보여서 잘 만들어져있는지 확인하기 위해서 백그라운드 컬러를 넣어볼 수 있다.
그런데 미리보기로 확인해보면 프레임레이아웃이나 릴레이티브 레이아웃처럼 겹쳐서 보인다.
300dp 인것같지도 않다. 드로어 레이아웃 같은 경우 미리보기 화면만 보고 작업하는데는 한계가 있다. 일단 서랍에 걸맞는 디자인을 해보자

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="나는 서랍"
            android:textSize="30dp" />

        <ImageView
            android:layout_width="250dp"
            android:layout_height="250dp"
            android:layout_gravity="center"
            android:src="@drawable/rabbit" />

        <Button
            android:id="@+id/btn_close"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="서랍닫기" />
        
    </LinearLayout>
    <!--서랍 레이아웃-->

</androidx.drawerlayout.widget.DrawerLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/d2286fe9-96d0-4bfb-a17f-de5f3ab40954)

- 지금 레이아웃을 통째로 차지하고 있다.
- 왼쪽이나 오른쪽으로 좀 몰아야 한다 하지만 안타깝게도 gravity를 사용할 수 가 없다. 그런데 적으면 적용이 된다;;;
- 에뮬레이터 켜서 작동하는지 확인을 해보자 빈공간에서 왼쪽에서 오른쪽으로 드래그하면 열리고
- 다시 밀어넣으면 들어가고, 빈공간을 눌러도 들어가고 뒤로가기 버튼을 눌러도 들어간다.
- 이제 열기 버튼을 눌렀을 때 서랍이 나오고 닫기 버튼을 누르면 서랍을 닫게 만들어보자.

## DrawerLayoutActivity 에 코드 추가하기
```java
package com.korea.ex_0718;

import androidx.appcompat.app.AppCompatActivity;
import androidx.drawerlayout.widget.DrawerLayout;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;

public class DrawerLayoutActivity extends AppCompatActivity {

    DrawerLayout drawer_layout;
    Button btn_open, btn_close;
    //버튼을 눌렀을 때 숨겨져있는 레이아웃이 나와야 하기 때문에 서랍 레이아웃도 등록을 해줘야 한다.
    LinearLayout drawer;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_drawer_layout);

        drawer_layout = findViewById(R.id.drawer_layout);

        //열기버튼을 눌렀을 때 열리고 닫기버튼을 눌렀을 때 닫는 기능만 만들면 된다.
        btn_open = findViewById(R.id.btn_open);
        btn_close = findViewById(R.id.btn_close);
        drawer = findViewById(R.id.drawer);

        btn_open.setOnClickListener(click);
        btn_close.setOnClickListener(click);

        //드래그로 서랍을 열지 못하게 할 수 있다!
        drawer_layout.setDrawerLockMode(DrawerLayout.LOCK_MODE_LOCKED_CLOSED);
    }//onCreate

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            int id = view.getId();
            if (id == R.id.btn_open) {
                drawer_layout.openDrawer(drawer);
            } else if (id == R.id.btn_close) {
                //닫고싶은 서랍 지정해서 닫기
                // drawer_layout.closeDrawer( drawer);
                // 모든 서랍닫기
                drawer_layout.closeDrawers();
            }
        }
    };
}
```
