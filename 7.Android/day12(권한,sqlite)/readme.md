# 권한
- 앱 다운받으면 이 애플리케이션이 무슨 권한을 쓰는데 허용할 거냐 허용하지 않을꺼냐 물어보는 경우가 많다.
- 예전에 전화 거는 페이지로 갈 때 권한이 활성화가 안되어 있어서 설정에 들어가서 앱 들어가서 권한 메뉴 들어가서 직접 권한을 활성화 해야 접근이 가능했다.
- 너무 번거롭고 개발자가 아니면 모르는 사람이 훨씬 많을거고 번거로워서 앱들이 다운을 받으면 물어보는 경우가 많아졌다.
- 앱을 켰을 때 물어봐주는 코드를 만드려면 일일히 작성을 했어야 했는데 권한 관련해서 수락을 해주고 물어봐주는 라이브러리가 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/e5da0ca7-ad77-44ae-95a6-369444edc463)

## Ex_날짜 프로젝트 생성하기

## PermissionActivity로 이름 변경하기
- 마쉬멜로우 이후에 나온 버전은 사용자한테 반드시 사용자 한테 권한이 비활성화 되어있다.
- 사용을 할꺼냐 하고 물어보는걸 필수로 해야 한다.
- 이전 버전에서는 매니패스트에 추가만 해놓으면 따로 허락을 받지 않아도 사용을 할 수 있었다.
- 하지만 권한을 남용하는 일이 생기다 보니 사용자에게도 특정 권한을 쓸거다 라고 알려주는 코드를 작성하도록 보안정책이 바뀌었다.
- 앞으로 우리가 만들 모든 애플리케이션은 사용자가 수락을 해야하는 권한이 존재한다면 수락을 하던지 거절을 하던지 결정할 수 있게 해줘야 한다.
- 사용자가 허락을 해줘야 하는 권한이 있고 안받아도 되는 권한이 있다.
- api연동할 때 썼던 인터넷에 연동하는 권한은 안받아도 되지만
- 전화를 거는 권한, sd카드에 폴더를 만드는 권한, 주소록같은걸로 연결하는 것에 대해 사용자가 수락을 해줘야 한다.
- 사용자 입장에서 수락을 해야되는 권한들 위주로 매니패스트에 추가를 해줄거다.

## Manifest에 권한 추가하기
- 우리가 만들 어플리케이션이 전화를 직접 걸어주는 기능이 있다
- 배달의 민족처럼 직접 전화까지 걸어주려면

```xml
<!--전화걸기 권한-->
<uses-permission android:name="android.permission.CALL_PHONE"/>
```
- intent에서 봤던 Call_phone 이 등록이 되어 있어야 한다.

- 휴대폰 안에 폴더를 생성한다던지mkdirs 해서, outputStream을 이용해서 파일을 만든다던지
- 기록을 할 때 수락을 받아야 하는 권한이 있다.
```xml
<!--내부저장소에 기록을 위한 권한-->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

- 주소록에 접근하는 권한
```xml
<!--주소록 접근 권한-->
<uses-permission android:name="android.permission.READ_CONTACTS"/>
```

- 에뮬레이터 실행 하지만 권한에 대한 내용이 비활성화 되어 있다.
- 기능을 이용할 건지 권한을 자동으로 물어봐주지 않는다.
- 예를들어 배민으로 주문한 가게에 직접 전화를 하려고 하는데 사용자에게 권한을 안물어보면
- 앱이 그냥 종료된다.
- 일반 사용자는 설정 -> 앱&및 알림 -> 앱 선택 -> 권한 에 가서 권한을 켜야겠구나 하고 생각하지 않는다.

## TedPermission
- https://github.com/ParkSangGwon/TedPermission

## build.gradle에 dependencies에 추가하기
- x.y.z를 3.3.0으로 수정해주자.
```xml
dependencies {
    // Normal
    implementation 'io.github.ParkSangGwon:tedpermission-normal:x.y.z'
    
    // Coroutine
    implementation 'io.github.ParkSangGwon:tedpermission-coroutine:x.y.z'

    // RxJava2
    implementation 'io.github.ParkSangGwon:tedpermission-rx2:x.y.z'
    // RxJava3
    implementation 'io.github.ParkSangGwon:tedpermission-rx3:x.y.z'
}
```

## PermissionActivity에 코드 추가하기
```java
package com.lhj.ex_0726;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;

import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.widget.Toast;

import com.gun0912.tedpermission.PermissionListener;
import com.gun0912.tedpermission.normal.TedPermission;

import java.util.List;

public class PermissionActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_permission);

        //현재 수락이 필요한 권한들에 대한 상태를 확인
        //전화걸기, 기록하기, 주소록 접근 에 대한 권한이 비활성화 되어있는지 확인

        //ActivityCompat : 권한 확인하라고 제공하는 클래스 (원래 있음)
        if(ActivityCompat.checkSelfPermission(
                PermissionActivity.this, Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED){

            //전화 걸기 권한을 수락해줘야 합니다. 수락 안했을 시 밑으로 내려가면 안되기 때문에 return을 넣자.
            setPermission();
            return;
        }
        if(ActivityCompat.checkSelfPermission(
                PermissionActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED){
            //저장 권한을 수락해줘야 합니다.
            setPermission();
            return;
        }
        if(ActivityCompat.checkSelfPermission(
                PermissionActivity.this, Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED){
            //주소록 접근 권한을 수락해줘야 합니다.
            setPermission();
            return;
        }
    }//onCreate();
```
- 만들어 놓은 권한중에 하나라도 수락이 안되어있는애가 있으면 if문 중 하는 반드시 걸릴것이다.
- 현재 권한 설정에 대해서 다 수락이 됐는지, 하나라도 수락이 안된게 있는지 판단하는 권한 수락과 관련된 감지자가 필요하다.

![image](https://github.com/to7485/Web1500/assets/54658614/b5f9f969-398a-4025-817d-d411dab00d23)


```java
    //앱 권한설정에 관한 감시자
    PermissionListener permissionListener = new PermissionListener() {
        @Override
        public void onPermissionGranted() {
            //모든 권한의 수락이 완료되었을 경우
            //가장 마지막에 작성하기
            Intent i = new Intent(PermissionActivity.this,PermissionActivity.class);
            startActivity(i);
            finish(); //액티비티가 겹치기 때문에 하나를 꺼주자.

에뮬레이터를 실행해서 권한을 다 수락하고 앱을 껐다켜도 수락창이 뜨지 않는걸 볼 수 있다.

        }

        @Override
        public void onPermissionDenied(List<String> deniedPermissions) {
            //한가지라도 허용이 되지 않은 권한이 있다면 호출

            //모든 권한이 수락되지 않았다면 강제로 종료
            Toast.makeText(PermissionActivity.this, "모든 권한을 수락해야 합니다.", Toast.LENGTH_SHORT).show();
            finish();
        }
    };

    //권한 수락에 관한 여부를 묻는 메서드
    public void setPermission(){
```

![image](https://github.com/to7485/Web1500/assets/54658614/f6922ee3-6693-4a25-ac19-002e3ce77b8f)


```java
        TedPermission.create().setPermissionListener(permissionListener).setDeniedMessage(
                "이 앱에서 요구하는 권한이 있습니다.\n [설정] > [권한]에서 해당 권한을 활성화 해주세요")
                .setPermissions(Manifest.permission.WRITE_EXTERNAL_STORAGE,Manifest.permission.CALL_PHONE,
                        Manifest.permission.READ_CONTACTS).check(); //수락을 해줘야 하는 세가지 권한을 명시해줘야 한다. 자동으로 해줄까? 라고 물어보는 역할
    }
}
```

# Sqlite
- 안드로이드 기본 데이터베이스를 만들어서 서버 없이 앱에다가 저장을 해볼것이다.
- DB를 사용하려면 외장 메모리에 어디 폴더에 우리 앱 이름 해서 폴더를 하나 생성을 해줘야 한다.
- 내부 저장소에 기록을 하도록 권한을 허락을 해놨다.
- 내부 데이터베이스를 휴대폰 내부에 저장을 하고 데이터들을 관리하는 액티비티를 만들어보자.
- 내장 데이터베이스 사용하려면 우리 휴대폰 내부에 데이터베이스를 복사해서 OUTPUT으로 넣어야 하기 때문에 내부저장소 기록 권한이 꼭 있어야 한다.

## SqliteActivity 액티비티 생성
- https://sourceforge.net/projects/sqlitebrowser -> github주소 클릭

![image](https://github.com/to7485/Web1500/assets/54658614/1335b713-d3c1-4731-88f1-1efb715e55ce)

- 아래로 스크롤 하여 윈도우 버전 다운받기

![image](https://github.com/to7485/Web1500/assets/54658614/a30f374a-06dc-4039-9ef9-a9405b8c9d14)

- DB Browser for SQLite - Standard installer for 64-bit Windows 다운로드

![image](https://github.com/to7485/Web1500/assets/54658614/40eb7314-21c5-442f-94a1-739b55146ac3)

- 바로가기 체크박스 체크하고 next -> 설치

![image](https://github.com/to7485/Web1500/assets/54658614/8a38efda-1252-49c7-81a3-7142e5210eaa)

- 새 데이터 베이스 -> myDB.db라고 바탕화면에 저장하기

![image](https://github.com/to7485/Web1500/assets/54658614/b1539b9c-a99e-4cb1-a3f5-ed83ef85d9ab)

- db가 만들어지는데 테이블도 없고 아무것도 없다.
- 테이블 이름은 friend로 설정하고
- 추가 눌러서 컬럼을 추가할 수 있다. name,phone,age 세개를 추가해보자
- text는 varchar2()이고 Integer는 number라고 보면 된다.

![image](https://github.com/to7485/Web1500/assets/54658614/28a792bb-0820-4244-8fef-023d058df9fb)

- 확인을 눌러서 완성을 한다. 데이터 보기를 눌러서 예시 데이터를 넣어주자.

![image](https://github.com/to7485/Web1500/assets/54658614/02c37db1-01e7-44d6-8179-c47e9e18f909)

![image](https://github.com/to7485/Web1500/assets/54658614/435fb8ba-a960-4676-8707-d9f0e7c95f72)

![image](https://github.com/to7485/Web1500/assets/54658614/8b883ccd-568e-491c-81a3-804b4e997e03)

- 두명의 데이터를 추가해보자. 그리고 위에 변경사항 저장하기를 누르자.

![image](https://github.com/to7485/Web1500/assets/54658614/fa8ce51b-6723-47ca-a6bb-355a7ec5aa39)

## build.gradle에 권한 추가하기
- 수락을 받아야 하는 권한이 아니기 때문에 라이브러리로 띄워줄 필요도 없고 그냥 작성만 해놓으면 된다.
- outputStream을 쓸 수 있는 권한이기도 하다.
```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/> 
로 하게 되는데 내용을 읽어오려면 READ_EXTERNAL_STORAGE 권한이 필요하다.
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

## DB 저장하기
- 아무데다 저장할 수 없다. Main -> assets폴더 만들기

![image](https://github.com/to7485/Web1500/assets/54658614/0e029921-19ad-4d6d-a6b4-966983df4cad)

- 아까 만들어놨던 myDB.db 복사해서 넣기

![image](https://github.com/to7485/Web1500/assets/54658614/d9d4a44c-f2ea-4b09-8bb1-8cbdb762fb54)

## SqliteActivity 액티비티 코드 작성하기
```java
package com.lhj.ex_0726;

import androidx.appcompat.app.AppCompatActivity;

import android.content.res.AssetManager;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.os.Environment;

import java.io.File;
import java.io.InputStream;
import java.io.OutputStream;

public class SqliteActivity extends AppCompatActivity {

    //안드로이드에서 기본적으로 제공을 해준다.
    SQLiteDatabase mDatabase;

    //처음에 한번 복사를 하면 그다음은 복사를 해줄필요 없어서 막아줄 변수를 하나 만든다.
    boolean isFirst = true;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sqlite);

        //애플리케이션이 최초에 실행 되었을 때 assets폴더의 DB를 휴대폰 내부저장소에 저장
        copyAssets();
    }//onCreate();

    //assets폴더의 DB를 휴대폰 내부장소에 저장 하기 위한 메서드
    public void copyAssets(){

inputStream으로 읽어서 outputStream으로 휴대폰 내부에 저장을 할 것이다.

        // getAssets() 를 통해 assets 폴더 밑의 file을 불러 올수 있다
        AssetManager assetManager = getAssets();
        String[] files = null;
        String mkdir = "";
        try {
            //files[0] --> "images" 보통 images폴더가 숨어있어서 1번으로 해야한다.
            //files[1] --> "myDB.db" 파일 이름이 문자열로 들어온다.
            files = assetManager.list("");

            InputStream in = null;
            OutputStream out = null;

inputStream은 io클래스 이기 때문에 open할 때 ioException이 발생할 수 있기 때문에 try문 안에서 작성을 해주자.
            //files[1]의 값인 "myDB.db"와 같은 이름의 파일을 찾아서
            //inputStream으로 읽어온다.
            in = assetManager.open(files[1]);

            //내부저장소에 폴더 생성
            //휴대폰 내부(기본) 저장소의 root(최상위) 경로로 접근

sd카드로는 함부로 접근을 하면 안되는게, sd카드가 안들어가는 휴대폰도 있음

            //내부저장소에 폴더 생성
            //휴대폰 내부(기본)저장소의 root(최상위) 경로로 접근
            String str = "" + Environment.getExternalStorageDirectory();
            mkdir = str + "/databse"; //databse라고 하는 이름의 폴더를 생성할 예정

            File mpath = new File(mkdir);
                if(!mpath.exists()){
                    isFirst = true;
                }
                if(isFirst) {
                    mpath.mkdirs(); //databse폴더를 실질적으로 생성
                     //databse이름의 폴더까지 접근해서 myDB.db라는 이름으로 output할 준비해
                    //root/database/myDB.db
                    out = new FileOutputStream(mkdir + "/" + files[1]);

sqlite는 처음 복사할 때 용량이 많지 않아서 1~2mb를 잡아주면 되는데 in에서 읽어온걸 기록을 다 할것이다.

                    byte[] buffer = new byte[2048];
                    int read = 0;

반복문 만들어서 1바이트씩 기록좀 해라 inputStream으로 담아온걸 buffer에 담아서 (한글자씩 읽어서)read한테 준다. 문서의 끝 -1을 만나면 while문이 정지가 된다.
                    while((read = in.read(buffer))!= -1){
                    out.write(buffer,0,read);
                    }
                    out.close();
                    in.close();

                    isFirst = false;
                }

        }catch (Exception e){


            }
        }
    }

```
- 에뮬레이터를 켜고 홈버튼을 누르고 설정으로 들어간다. -> 저장용량으로 들어간다.
- 우리가 데이터베이스라고 하는 폴더를 강제로 만들고 있는 중이다.
- 내부공유 저장용량 클릭 -> 파일 클릭 -> 내부저장소에 만들어져있는 저장소의 이름들을 볼 수 있다
- database라는 이름의 폴더가 생성이 되어 있어야 한다.
- mpath.mkdirs() 때문에 database라는 폴더가 만들어졌다.
- ouputStream이 정상적으로 작동을 해야 이 안에 myDB.db라는 녀석이 8.19kb의 용량으로 생겨있다.
- assets폴더에서 휴대폰 내부로 DB를 옮겨 넣은 것이고  이제 우리는 여기로 접근해서 insert,delete,update 같은 작업들을 수행할 것이다.
- 껏다 켜면 isFirst가 true로 다시 바뀌기 때문에 onCreate()에서 copyAsset을 호출하는데 매번 껏다 켤때마다 isFirst가 true여서 assets에 있는 내용을 다시 복사한다.
-  내용을 100개를 추가해놔도 다시 김길동,홍길동 두개만 남는다.

## SqliteActivity 액티비티 코드 추가하기
- 껐다 켯을 때도 isFirst가 false로 유지가 되야 한다. sharedPreference를 사용해보자.

```java
///////////////////////////////////////
 SharedPreferences pref;
///////////////////////////////////////

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sqlite);

///////////////////////////////////////
        pref = PreferenceManager.getDefaultSharedPreferences(SqliteActivity.this);

        load();
        //애플리케이션이 최초에 실행 되었을 때 assets폴더의 DB를 휴대폰 내부저장소에 저장
        copyAssets();
        save();
///////////////////////////////////////
    }//onCreate();

public void save() {
    SharedPreferences.Editor edit = pref.edit();
    edit.putBoolean("save", isFirst); //save라는 이름으로 isFirst를 저장한다.
    edit.commit();
    //카피에셋을 실행시키고 저장을 한거니까 isFirst값이 false가 저장이 되어야 한다.
}

//isFirst의 값을 로드
public void load() {
    isFirst = pref.getBoolean("save", true); //저장되어 있는 값이 없다면 기본값은 true로 해라

}
```

## layout_sqlite.xml 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".SqliteActivity">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1">

        <TextView
            android:id="@+id/result_txt"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="hong/20/010-111-1111\nkim/25/011" /> <!--예시로 보여주고 지우기-->

    </ScrollView>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn_all"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="전체보기" /> <!--select * from table-->

        <Button
            android:id="@+id/btn_search"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="검색" /> <!--select * from table where 조건식-->

        <Button
            android:id="@+id/btn_insert"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="추가" /> <!--insert into 테이블 values();-->

        <Button
            android:id="@+id/btn_del"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="5dp"
            android:layout_weight="1"
            android:text="삭제" /> <!--delete from 테이블 where 조건-->
    </LinearLayout>
    
    <!--추가할 때 이름과 전화번호 나이를 넣을 EditText-->
    <EditText
        android:id="@+id/et_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="input name" />

    <EditText
        android:id="@+id/et_phone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="input phone" />

    <EditText
        android:id="@+id/et_age"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="input age"
        android:inputType="number" />
</LinearLayout>
```

## SqliteActivity에 등록하기
```java
EditText et_name, et_phone, et_age;
Button btn_all, btn_search, btn_insert, btn_del;
TextView result_txt;

    et_name = findViewById(R.id.et_name);
    et_phone = findViewById(R.id.et_phone);
    et_age = findViewById(R.id.et_age);
    result_txt = findViewById(R.id.result_txt);
    btn_all = findViewById(R.id.btn_all);
    btn_search = findViewById(R.id.btn_search);
    btn_insert = findViewById(R.id.btn_insert);
    btn_del = findViewById(R.id.btn_del);
    
    btn_all.setOnClickListener(myClick);
    btn_search.setOnClickListener(myClick);
    btn_insert.setOnClickListener(myClick);
    btn_del.setOnClickListener(myClick);
   }//onCreate();

View.OnClickListener myClick = new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        int id = view.getId();
        if(id == R.id.btn_all){
            //전체조회 
        } else if(id == R.id.btn_search){
            //상세조회 
        }else if(id == R.id.btn_insert){
            //정보추가
        }else if(id == R.id.btn_del){
            //정보삭제
        }
    }
};

```

- DB라고 하면 전체 조회를 할 때 JSP로 치면 resultSet같이 움직일 수 있는 커서가 필요하다.
- myClick리스터 밑에 추가하기
```java
//쿼리문 수행을 위한 메서드
public void searchQuery(String query){
    Cursor c = mDatabase.rawQuery(query, null);
    //지금 DB를 복사만 해놓은 상태이고 실제 어디로 연결해야 되는지 지정이 되어있지 않다
    //mDatabase를 통해서 rawQuery라는 메서드를 가지고 쿼리문을 요청해야 한다.
}
```

- copyAssets()을 통해 복사된 DB를 읽기, mDatabase가 읽어와댜 한다.
```java
    //개발자가 완벽히 알맞은 코드나 충돌 가능성이 있는 코드를 사용할 때 사용하는 어노테이션
    @SuppressLint("WrongConstant")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sqlite);

        pref = PreferenceManager.getDefaultSharedPreferences(SqliteActivity.this);

        load();
        //애플리케이션이 최초에 실행 되었을 때 assets폴더의 DB를 휴대폰 내부저장소에 저장
        copyAssets();
        save();

        //openOrCreateDatabase(db명, 데이터베이스모드,쿼리가 호출되는 커서를 선택(커서객체를 만들어 사용할지에 입력 그렇지 않으면 null)
        //만들어져있는 DB를 열거나 없으면 새로 만들어주는 작업을 해주는 메서드
        mDatabase = openOrCreateDatabase(
                Environment.getExternalStorageDirectory()+"/database/myDB.db",
                SQLiteDatabase.CREATE_IF_NECESSARY, null);

        ... 중략


    }//onCreate();
```
- 쿼리문 추가하기
```java
View.OnClickListener myClick = new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        int id = view.getId();
        if(id == R.id.btn_all){
            //전체조회
            searchQuery("select * from friend");
        } else if(id == R.id.btn_search){
            //상세조회
        }else if(id == R.id.btn_insert){
            //정보추가
        }else if(id == R.id.btn_del){
            //정보삭제
        }
    }
};
```

- searchQuery()메서드를 통해서 데이터 조회하기
```java
public void searchQuery(String query) {
    Cursor c = mDatabase.rawQuery(query, null);
    //지금 DB를 복사만 해놓은 상태이고 실제 어디로 연결해야 되는지 지정이 되어있지 않다
    //mDatabase를 통해서 rawQuery라는 메서드를 가지고 쿼리문을 요청해야 한다.

    //c.getColumnCount() : friend테이블의 컬럼 수(name,phone,age)
    //컬럼의 수만큼 배열의 공간을 확보하라는 뜻
    String[] col = new String[c.getColumnCount()];
    col = c.getColumnNames();

    //col[0] : name
    // col[1] : phone
    // col[2] : age

    String[] str = new String[c.getColumnCount()];
    String result = ""; //최종 결과를 저장해둘 변수

    Log.i("MY", col[0] + "/" + col[1] + "/" + col[2]);

    while (c.moveToNext()) {
        for (int i = 0; i < col.length; i++) {
            str[i] = "";
            str[i] += c.getString(i); //각 컬럼별 실제 데이터
            result += col[i] + ":" + str[i] + "\n";
        }//for    result +="\n"; //for문 빠져나와서 엔터값 한번 더주기
    }//while
    result_txt.setText(result);
    //에뮬레이터를 켜고 전체 조회를 누르면 저장해놓은 샘플데이터가 조회된다.
}
```



