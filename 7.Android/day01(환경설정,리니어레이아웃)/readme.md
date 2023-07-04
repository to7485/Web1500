# Android
- 안드로이드는 세계에서 가장 대표적인 오픈소스 플랫폼이며 세계 최다 사용자를 보유한 모바일 운영체제이다.

## Android 버전
- 2009년 4월부터 안드로이드의 버전은 디저트 이름을 바탕으로 이름이 붙여 개발되고 있다.
- A와 B의 공식 명칭은 존재하지 않고 C부터 시작한다.
  - 컵케이크(Cupcake)- Android 1.5
  - 도넛(Donut)- Android 1.6
  - 이클레어(프랑스어: Eclair 에클레르[*])- android 2.0~2.1
  - 프로요(Froyo, 프로즌 요구르트)- Android 2.2
  - 진저브레드(Gingerbread, 생강빵)-Android 2.3
  - 허니콤(Honeycomb, 허니콤 토피)-Android 3.0
  - 아이스크림 샌드위치(Ice Cream Sandwich)-Android 4.0
  - 젤리빈(Jelly Bean)-Android 4.1~4.3
  - 킷캣(KitKat)-Android 4.4
  - 롤리팝(Lollipop)-Android 5.0~5.1
  - 마시멜로(Marshmallow)-Android 6.0
  - 누가(Nougat)-Android 7.0~7.1
  - 오레오(Oreo)-Android 8.0~8.1
  - 파이(Pie)-Android 9.0
  - 퀸 케이크(Queen Cake)-Android 10
  - 레드 벨벳 케이크(Red Velvet Cake)-Android 11
  - 스노우 콘(Snow cone)-Android 12~12.1
  - 티라미수(Tiramisu)-Android 13

- 롤리팝 버전 이전까지는 이클립스로 개발했지만 롤리팝 이후 버전은 이클립스로 개발하지 않고 안드로이드 스튜디오로 개발을 한다.
- 안드로이드 스튜디오도 jdk가 설치되어있어야 작동을 한다.

## 안드로이드 스튜디오 설치하기
- [안드로이드 스튜디오 사이트](https://developer.android.com/?hl=ko)

- 안드로이드 스튜디오를 설치하면서 개발하려는 OS버전도 같이 골라서 설치를 해야 하는데 OS별로 2~3GB이기 때문에 설치에 주의하자(같이 진행하기)

![image](https://github.com/to7485/Web1500/assets/54658614/e99555e0-4110-41e2-841a-aa5050cdada2)


![image](https://github.com/to7485/Web1500/assets/54658614/17cdaff8-ead9-4db6-9ed4-ec2319fefef1)

- 체크하고 다운로드하기

## 7.android 폴더 생성하고 util,work폴더 생성하기
- 같은 드라이브에 SDK폴더 만들기(Software Development Kit)
- SDK : 휴대폰을 연결하는데 필요한 라이브러리, 드라이버, OS가 저장되게 할 폴더
- 안드로이드 스튜디오는 여러개 설치가 불가능하기 때문에 위의 요소들이 흩어져서 설치되어있으면 서로 참조하다가 충돌이 나서 실행이 잘 안된다.

![image](https://github.com/to7485/Web1500/assets/54658614/fd23cdb1-0edb-4279-9c0c-cb884e7e7cfe)


### 제거하는법
- 안드로이드 스튜디오는 제어판에서 제거가 가능하지만 지워지지 않는 폴더가 있다.
- SDK, .android, .m2 폴더를 직접 지워줘야 한다.(안드로이드를 완전히 제거해야 하는 상황이라면)

안드로이드 스튜디오는 .apk파일까지 만드는것이 가능하다.

## 안드로이드의 특징
- 자바 기반이기 때문에 자바에 대한 지식이 있으면 jsp나 스프링에 비해서는 배우기 쉽다.(하지만 후반가면 어려운건 똑같다)
- 자바와 코틀린을 골라서 쓸 수 있도록 바뀌었다. 원래는 only 자바였음

## 설치하기
설치파일 util폴더로 옮기기

2. 다운로드 된 설치파일 누르고 경로 util\Android Studio로 바꿔주기

3. 설치가 되면 Do not import settings 선택 하고 ok

![image](https://github.com/to7485/Web1500/assets/54658614/db965d4d-d032-4c2c-8ea3-bfb266b3c18a)


4. next -> custom (standard로 설치하면 쓸데없는 것 까지 다 설치함, 잘못 설치하면 지웠 다가 다시 깔아야 됨)

![image](https://github.com/to7485/Web1500/assets/54658614/a5706f21-5a5e-478b-b740-bcea2112513b)


5. 테마 선택

6. Anroid Virtual Device 체크가 된다면 체크하기

7. sdk 경로 설정해주기

![image](https://github.com/to7485/Web1500/assets/54658614/de4eaaf5-5b40-46e8-8915-e9abd956035a)


8. 가속기가 사용할 메모리 설정해주기(안나와도 default 값이 2gb)

9. 권한 수락

![image](https://github.com/to7485/Web1500/assets/54658614/ba3e693a-5fc6-4c0b-b7b1-c02b0d651977)


※ AMD CPU의 경우 가상화 기능이 안켜져 있는 경우가 많다.<br>컴퓨터 켤 때 delete, f10버튼 연타해서 BIOS 화면으로 들어가서 가상화 기능을 켜야 한다.<br>아니면 안드로이드폰을 직접 연결해서 결과를 확인해야 하는데 적어도 갤럭시노트5 이상

- 프로젝트를 만들기 위해 에뮬레이터도 설치해야 하고 에뮬레이터에 os도 설치해야 하고 라이브러리 들을 설치해야 한다.

10. more Actions -> sdk manager

![image](https://github.com/to7485/Web1500/assets/54658614/4a5efdde-2392-4616-8323-7c2f033c7851)

11. Android 11 (R) 설치
  - 오른쪽 아래 show Package Details 클릭
  - Android SDK Platform 30 만 클릭하고 설치
  -  sdk 경로 맞는지 물어보고 ok 눌러서 설치

![image](https://github.com/to7485/Web1500/assets/54658614/74070fdd-f506-44f7-9ff2-40f7efbba428)

12. more Actions -> sdk manager- > SDK Tools
  - build tools 전체 선택 하고 설치 원래는 27만 설치하면 되지만 그러면 안되는 기능이 있을 수 있기 때문에 전체 다 설치 해주자

![image](https://github.com/to7485/Web1500/assets/54658614/13054acf-707a-4191-880a-d3d2cb453774)

# 프로젝트 생성하기
- new project
- activity : 앱의 전반적이 활동을 담당하는것이 액티비티 사용자가 여러가지 액션을 취할 수 있게 해줍니다.
- 많은 종류의 액티비티가 존재한다. 안드로이드에서는 Activity 클래스를 상속받는 클래스들이다.
- 액티비티가 하나의 화면이라고 생각하면 된다. 여러가지 템플릿들이 많이 존재한다.

![image](https://github.com/to7485/Web1500/assets/54658614/e17d97fb-f388-4d43-af27-afe9db48d907)

- Empty Views 액티비티를 선택하고 next를 누른다.

## Ex_날짜 프로젝트 생성하기

![image](https://github.com/to7485/Web1500/assets/54658614/619c6e0a-172a-427e-961d-410790204d24)


- Name : 프로젝트 이름
- Package name : com.korea (example은 사용하지 말것 마켓에 올릴때도 연습용으로 만들었다고 판단하여 안될 때가 있음)
- Save location : work폴더까지 잡는데 프로젝트 이름으로 된 폴더를 하나 더 만들지 않으면 work폴더에다가 만들어버려서 다른 프로젝트를 만들 수 없게 된다
- Language : java
- Minimum SDK : API 27: Android 8.1(Oreo)

방화벽 같은거 물어볼 수 있으니 수락하자.<br>
오른쪽 아래 프로젝트 생성이 완료가 되어야 한다.<br>

- 왼쪽 위에 안드로이드로 되어 있다. 프로젝트로 바꾸면 이클립스랑 더 유사하다.
- app -> src -> main -> java -> 패키지 -> 액티비티 클래스 존재

![image](https://github.com/to7485/Web1500/assets/54658614/080a98f8-65e1-4f50-b452-c6061ab4de24)

### 글자 크기 바꾸기
- file -> Settings -> font 검색 -> 글씨체는 consolas

![image](https://github.com/to7485/Web1500/assets/54658614/73eaa3ec-5dfe-4025-a9b0-880df0e85fa8)

- 디자인은 html 처럼 태그를 가지고 한다.
- 자바 gui 처럼 이벤트 처리는 클래스에서 한다.

```java
package com.korea.ex_0705;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); -> 내가 어떤 화면을 점유하고 있는지 표시를 해줌
    }
}
```

- 액티비티를 만들면 화면을 구성하는 레이아웃이 세트로 만들어진다.
- xml을 눌러 화면 디자인을 볼 수 있다.
- 요소를 드래그앤 드랍으로 만들 수 있지만 휴대폰마다 크기가 달라 요소들이 위치가 제멋대로 바뀔 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/9e37b7dc-ef89-47d8-a9af-e1b3fc09cce5)

- 오른쪽 split을 누르면 화면을 구성하는 코드를 볼 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/4d2e39e1-3ab8-46ff-a794-3423a6e4114d)

- 내용을 바꿔도 자동으로 저장이 된다.
- 레이아웃은 res -> layout폴더 안에 존재한다.
- drawable에 이미지를 넣는다.
- mipmap에 아이콘을 넣는다.

![image](https://github.com/to7485/Web1500/assets/54658614/60aea47b-7c38-4d55-96ff-bc1485c7ffc3)

## 에뮬레이터 생성하기
- 프로젝트를 진행하면서 확인을 해야 하는데 안드로이드폰이 없으니 에뮬레이터를 활용해야 한다.

- 오른쪽위 핸드폰 모양 누르기

![image](https://github.com/to7485/Web1500/assets/54658614/9a6112ad-44d5-4c4a-9eff-05a017e97f03)

-  Create device

![image](https://github.com/to7485/Web1500/assets/54658614/e689494e-11cf-4a9f-88ea-73f83d0551c8)

- 안드로이드폰 마다 해상도가 다르다보니 기준이 되는 해상도가 필요하다.
- pixel2 와 같이 1080x1920이 표준으로 잡기에는 괜찮다.

![image](https://github.com/to7485/Web1500/assets/54658614/84c4a523-5086-4f4b-8a80-ed3315edf4f3)

-  next -> R 이미지 다운로드 (에뮬레이터에도 OS가 필요하다)

![image](https://github.com/to7485/Web1500/assets/54658614/60087134-6351-439c-b409-4a96b3b2eb3d)

- 오른쪽 위에 디바이스가 표시되고 ▶ 버튼 누르면 실행됨

![image](https://github.com/to7485/Web1500/assets/54658614/0cbca288-e41c-4280-9cf9-4ed0fa4dc178)

### 액티비티 이름 바꾸기
- shift + f6 누르면 이름을 변경할 수 있다.
- 메인 액티비티를 FirstActivity로 바꿔보자

![image](https://github.com/to7485/Web1500/assets/54658614/e9978d20-dfd2-46e8-9118-dd985e5c722e)

- 해당하는 레이아웃의 이름도 맞춰줘야 한다.
- 레이아웃의 이름에는 대문자가 들어가면 안된다.

![image](https://github.com/to7485/Web1500/assets/54658614/83f16935-21cc-4c25-b770-f42affaa6e96)

# 레이아웃의 종류
- 가장 바깥쪽을 담당하는 body 태그의 역할을 하는게 레이아웃이다.
- 레이아웃마다 요소를 배치하고 위치를 정하는게 다 다르다.
- ConstraintLayout이 디폴트로 되어있는데 가장 안좋은 것 같다. 

- 패키지에서 new -> activity -> Empty Activity 생성하기 액티비티도 클래스다

![image](https://github.com/to7485/Web1500/assets/54658614/50739421-fc2c-4dbe-b006-741d09472f3e)

- 코드 자동정렬 : ctrl + alt + L

## LinearLayout
- 방향성을 갖고 있는 레이아웃
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".LinearActivity">
    <!--LinearLayout은 방향성을 가지고 있다.
    orientation : vertical -> 수직정렬
                horizontal -> 수평정렬 -->

    텍스트뷰는 세로와 가로 속성을 반드시 갖는다.
    wrap_contet : 컨텐츠의 길이만큼 공간을 차지하겠다.
    match_parent : 부모의 길이와 똑같은 공간을 차지하겠다.

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView1"/>

    밑에있는 요소의 크기를 math_parent로 바꿔도 위에있는 객체의 범위까지 침범하지 못한다.

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#aaf"
        android:text="안녕하세요"
        android:textSize="10mm" />
    <!--mm과 같은 절대값을 사용해버리면 휴대폰 크기가 바뀌면 크기가 다르게 나올 수 있다. 그래서 dp와 같은 단위를 사용하는게좋다-->
    dp : device independens pixel
    사용중인 디바이스의 해상도에 맞는 pixel값을 계산하여 크기를 결정해주는 단위
    완전히 해결되는건 아니지만 mm 보다는 훨씬더 편리한 단위기 때문에 모든 단위는 dp값으로 통일한다.

    <SeekBar
      android:layout_width="match_parent"
      android:layout_height="wrap_content" /> <!-- -> 음량 조절하는 바-->

      <ProgressBar
        android:layout_width="100dp"
        android:layout_height="100dp" />
    <!-- -> 로딩할 때 뜨는 요소-->

        <RatingBar
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:numStars="3" /> -> 별점 요소 별점을 3개로 조절할 수도 있다.

    <CheckBox
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="체크박스" /> -> 간혹 크기를 변경하지 못하는 요소들도 있음

        <RadioGroup
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn1" /> -> 라디오 그룹안에 있는 속성은 하나만 선택 가능

        <RadioButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn2" />
    </RadioGroup>

    현재 버티컬 레이아웃인데 레이아웃 안에 가로로 요소를 배치하고 싶을 때가 있다.
    리니어 레이아웃을 한 개 더 집어넣어서 영역을 확보할 수 있다.

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#afa"
        android:orientation="horizontal">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn1" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="btn2" />
    </LinearLayout>

</LinearLayout>
```

![image](https://github.com/to7485/Web1500/assets/54658614/431c4ed0-0626-4fa9-9bba-e95a2988bed8)
