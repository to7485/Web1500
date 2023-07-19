# 지문인식 기능 만들기

## Ex_날짜 프로젝트 생성하기

## FingerprintActivity로 이름 변경하고 레이아웃 디자인하기
- 지문인식 관련 라이브러리가 있는데 jar파일로 존재하는게 아니라 깃헙 같은곳에서 경로를 가져와야 한다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FingerprintActivity">

    <ImageView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@drawable/img_finger"
        android:layout_centerInParent="true"
        android:id="@+id/finger"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="지문을 인증하세요"
        android:textSize="25dp"
        android:id="@+id/text"
        android:layout_centerHorizontal="true"
        android:layout_below="@id/finger"
        android:layout_marginTop="10dp"/>
</RelativeLayout>
```

## 지문인식 라이브러리 추가하기
- 지문인식은 이라는거 자체는 안드로이드에서 지원해주지 않기 때문에 라이브러리를 사용해야 한다.
- <a href="github.com/ajalt/reprint">지문인식 링크</a>로 들어가보자. 내리다보면 installation을 만난다.

![image](https://github.com/to7485/Web1500/assets/54658614/58f2542b-bd85-4a1f-8721-75a42f8ae03f)

- 깃허브에서 라이브러리를 가져와야 한다면 만약에 dependencies라는 공간만 있으면 거기 있는거만 복사하면 된다.
- jitpack.io라는걸 표기하고 있으면 이 라이브러리들의 버전을 jitpack.io라는데서 관리를 하면서 우리 안드로이드까지 전달을 해주는데가 있다.
- 곧장 사용할 수 있도록 해놓은 개발자도 있고 jitpack을 거쳐야 사용할 수 있게끔 만들어 놓은 개발자가 있다.

### build.gradle에 추가하기
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

    implementation 'com.github.ajalt.reprint:core:3.3.2@aar'
    // required: supports marshmallow devices
    implementation 'com.github.ajalt.reprint:reprint_spass:3.3.2@aar' 
    // optional: deprecated support for pre-marshmallow Samsung devices
    implementation 'com.github.ajalt.reprint:rxjava:3.3.2@aar' 
    // optional: the RxJava 1 interface
    implementation 'com.github.ajalt.reprint:rxjava2:3.3.2@aar'
    // optional: the RxJava 
}
```

- jitpack로 우회하려고 하는 라이브러리를 쓰려고 한다.
- 깃헙으로 돌아와서 jitpack 링크를 복사하자. repositories라는 곳에 넣으라고 한다.














