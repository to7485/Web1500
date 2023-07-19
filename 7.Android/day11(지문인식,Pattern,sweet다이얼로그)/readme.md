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

## FingerprintActivity 코드 작성하기
- 지문인식은 이라는거 자체는 안드로이드에서 지원해주지 않기 때문에 라이브러리를 사용해야 한다.
- github.com/ajalt/reprint주소로 들어가보자. 내리다보면 installation을 만난다.


















