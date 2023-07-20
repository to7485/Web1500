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
