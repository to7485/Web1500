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
- github.com/ajalt/reprint 지문인식 링크 로 들어가보자. 내리다보면 installation을 만난다.

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

![image](https://github.com/to7485/Web1500/assets/54658614/002e8668-a3c6-4775-a80a-ee7822eda9ac)

### setting.gradle에 추가하기
```xml
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url "https://jitpack.io" } -> 추가하기
    }
}
```

- sync now 누르기

### manifest에 권한 추가하기
- 다시 깃헙 홈페이지로 돌아가서 권한을 추가해주자.

![image](https://github.com/to7485/Web1500/assets/54658614/47d35e38-7d58-4a83-837f-a53ba7811812)

```xml
<!-- Marshmallow fingerprint permission-->
<uses-permission android:name="android.permission.USE_FINGERPRINT"/>

<!-- Samsung fingerprint permission, only required if you include the Spass module -->
<uses-permission android:name="com.samsung.android.providers.context.permission.WRITE_USE_APP_FEATURE_SURVEY"/>
```

- 마시멜로에서의 접근권한은 듀플리케이트가 되었기 때문에 사실상 지워줘도 된다.
- 액티비티로 돌아와서 지문인식 기능을 사용하게 코드를 작성해보자.
- 다음과 같이 뜨면 성공

![image](https://github.com/to7485/Web1500/assets/54658614/36f83c6e-d792-4eb4-9fca-767a55dedf65)

- 지문인식을 위해서 초기화(initialize())를 해줘야 한다. 휴대폰의 환경을 알아야 한다.
- 휴대폰에 지문이 등록이 되어있는지, 휴대폰에 지문인식 센서는 있는지 확인을 해줘야 한다.

## FingerprintActivity에 코드 작성하기
```java
package com.lhj.ex_0725;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;
import android.widget.Toast;

import com.github.ajalt.reprint.core.AuthenticationFailureReason;
import com.github.ajalt.reprint.core.AuthenticationListener;
import com.github.ajalt.reprint.core.Reprint;

public class FingerprintActivity extends AppCompatActivity {

    TextView text;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_fingerprint);

        text = findViewById(R.id.text);

        //1. 지문사용을 위한 초기화
        Reprint.initialize(FingerprintActivity.this);

        //3. 메서드 작성후 조건문 작성하기
        if(checkSpec()){
            //지문인식이 가능한 경우
            Reprint.authenticate(new AuthenticationListener() {
                @Override
                public void onSuccess(int moduleTag) {
                    //보통은 텍스트만 바꾸는데 끝나지않고 화면을 전환한다.
                    text.setText("인증 성공");

                }

                @Override
                public void onFailure(AuthenticationFailureReason failureReason, boolean fatal, CharSequence errorMessage, int moduleTag, int errorCode) {
                    text.setText("인증 실패. 다시 시도하세요.");
                }
            //4. 지문등록하기
            });
        }

    }//onCreate()

    //2. 지문인식이 가능한지 판단 우리 휴대폰의 스펙을 확인해보자!
    public boolean checkSpec(){
        //참이면 지문인식 센서가 존재한다는 것
        boolean hardware = Reprint.isHardwarePresent();

        //센서가 있어도 지문이 등록이 안되어있으면 사용을 할수가 없기 때문에 등록되어 있는 지문을 확인해보자.
        boolean register = Reprint.hasFingerprintRegistered();

        if(!hardware){
            Toast.makeText(FingerprintActivity.this, "지문인식을 지원하지 않는 기기입니다.", Toast.LENGTH_SHORT).show();
        } else {
            if(!register){
                Toast.makeText(FingerprintActivity.this, "등록된 지문이 없습니다.\n 지문등록을 먼저 해주세요.", Toast.LENGTH_SHORT).show();
            }
        }
        return hardware && register;

    }
}
```
- 지문인식이 가능한 경우 hardware와 register가 참인경우만 가능하다.
- 코드 작성을 마무리 하고 에뮬레이터를 실행해보면 지문을 등록하라는 토스트가 뜰 것이다.
- 모니터에 지문을 찍을 수 없는 노릇이니 설정에가서 등록을 해줘야 한다.

## 지문 등록하기
- 설정 -> 보안&위치 -> 지문 -> 패턴등록 -> 알림 표시 안함 -> 스튜디오 에뮬레이터 위에 … -> fingerprint -> finger1 Touch Senser
- 가상의 10개의 손가락이 있음 가상으로 할 수 있는게 은근히 많다.
- 위치정보, 센서는 좀 구리고 나머지는 괜찮음

![image](https://github.com/to7485/Web1500/assets/54658614/5e48f8fb-2da1-4d8a-af8a-47fbc2c01306)

- 창 계속 켜놔야 한다. 에뮬레이터 재실행하고 Touch Sensor 누르면 인증성공으로 바뀜

# 패턴 만들기
- PatternLockView 라이브러리

## PatternActivity 생성하고 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    tools:context=".PatternActivity">

    <TextView
        android:id="@+id/password_text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:gravity="center"
        android:textSize="20sp" />

    <com.andrognito.patternlockview.PatternLockView
        android:id="@+id/pattern_lock_view"
        android:layout_width="280dp"
        android:layout_height="280dp"
        android:layout_marginTop="30dp"
        app:normalStateColor="#000000" />
</LinearLayout>
```

## build.gradle에 추가하기
```xml
implementation 'com.andrognito.patternlockview:patternlockview:1.0.0'
```
## setting.gradle에 추가하기
```xml
repositories {
        google()
        mavenCentral()
        maven { url "https://jitpack.io" }
        jcenter(); -> 추가하기
    }
```

## gradle.properties 밑에 코드 추가하기
- 남이 만든 라이브러리를 내가 만든 프로젝트에서 쓸 수 있게 속성을 주는 코드
```xml
android.enableJetifier=true
```

## PatternActivity에 코드 작성하기
```java
package com.korea.ex_0718;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;

import com.andrognito.patternlockview.PatternLockView;
import com.andrognito.patternlockview.listener.PatternLockViewListener;
import com.andrognito.patternlockview.utils.PatternLockUtils;

import java.util.List;

public class PatternActivity extends AppCompatActivity {

    PatternLockView patternLockView;
    TextView passwordTextView;
    //패턴상태
    String restorePassword;

    //저장된 패스워드
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_pattern);
        passwordTextView = findViewById(R.id.password_text_view);
        patternLockView = findViewById(R.id.pattern_lock_view);
        patternLockView.addPatternLockListener(new PatternLockViewListener() {
            @Override
            public void onStarted() {
                Log.i("my", "패턴그리기시작");
            }

            @Override
            public void onProgress(List<PatternLockView.Dot> progressPattern) {
                //패턴그리는중
                Log.i("my", "패턴 그리는중: " + PatternLockUtils.patternToString(patternLockView, progressPattern));
            }

            @Override
            public void onComplete(List<PatternLockView.Dot> pattern) {
                Log.i("my", "패턴완성: " + PatternLockUtils.patternToString(patternLockView, pattern));
            }

            @Override
            public void onCleared() {
                Log.i("my", "패턴이 지워짐");
            }
        });
    }
}
```

![image](https://github.com/to7485/Web1500/assets/54658614/9d445eca-6bd5-487a-ba55-aa7c9bca5721)

- 에뮬레이터를 켜서 패턴을 그려보자 그리고 logcat에서 호출되는 메서드를 알아보자

## PatternActivity에 코드 추가하기
- 패스워드가 일치하면 패턴성공 이라고 띄우기

```
package com.korea.ex_0718;

import androidx.appcompat.app.AppCompatActivity;

import android.app.Activity;
import android.content.SharedPreferences;
import android.graphics.Color;
import android.os.Bundle;
import android.util.Log;
import android.widget.TextView;

import com.andrognito.patternlockview.PatternLockView;
import com.andrognito.patternlockview.listener.PatternLockViewListener;
import com.andrognito.patternlockview.utils.PatternLockUtils;

import java.util.List;

public class PatternActivity extends AppCompatActivity {

    PatternLockView patternLockView;
    TextView passwordTextView;
    //패턴상태
    String restorePassword;

    //저장된 패스워드
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_pattern);
        passwordTextView = findViewById(R.id.password_text_view);
        patternLockView = findViewById(R.id.pattern_lock_view);
        patternLockView.addPatternLockListener(new PatternLockViewListener() {
            @Override
            public void onStarted() {
            }

            @Override
            public void onProgress(List<PatternLockView.Dot> progressPattern) {
            }

            @Override
            public void onComplete(List<PatternLockView.Dot> pattern) {
                //입력한 패스워드
                String password = PatternLockUtils.patternToString(patternLockView, pattern);
                //1-1 저장된 패스워드가 있다면
                if (!restorePassword.equals("")) {
                    //2-1 입력과 저장된 패스워드가 같다면
                    if (password.equals(restorePassword)) {
                        passwordTextView.setText("패턴 성공");
                        //패턴 색상 변경
                        patternLockView.setCorrectStateColor(Color.parseColor("#0000FF"));
                        //파랑
                    }
                    //2-2 입력과 저장된 패스워드가 다르다면
                    else {
                        passwordTextView.setText("잘못된 패턴");
                        //패턴 색상 변경
                        patternLockView.setCorrectStateColor(Color.parseColor("#FF0000"));
                        //빨강
                    }
                }
                //1-2 저장된 패스워드가 없다면
                else {
                    //현재 패스워드 저장
                    SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
                    SharedPreferences.Editor editor = pref.edit();
                    editor.putString("password", password);
                    editor.commit();
                    passwordTextView.setText("저장되었습니다.");
                    patternLockView.clearPattern();
                    //패턴지우기
                }
            }

            @Override
            public void onCleared() {
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        restorePassword();
    }

    private void restorePassword() {
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        //저장된 패스워드 변수에 담기
        restorePassword = pref.getString("password", "");
        //등록한 비밀번호가 있을 때
        if (pref != null && !restorePassword.equals("")) {
            passwordTextView.setText("등록한 패턴을 입력해주세요");
        }
    //등록한 비밀번호가 없을 때
    else{
        passwordTextView.setText("저장된 패턴이 없습니다.");
    }
    }
}
```

# sweet 다이얼로그
- 다이얼로그의 업그레이드 버전인 sweet alert dialog 에 대해서 알아보도록 하자
- 기존 다이얼로그에서 애니메이션이 조금 추가된 버전이고 라이브러리를 사용을 해야 한다.

## SweetAlertActivity 생성하고 디자인하기
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SweetAlertActivity"
    android:orientation="vertical">

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="warning"
        android:id="@+id/btn_warning"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="error"
        android:id="@+id/btn_error"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="success"
        android:id="@+id/btn_success"/>

</LinearLayout>
```
## build.gradle에 라이브러리 추가하기
```xml
implementation 'com.github.thomper:sweet-alert-dialog:1.4.0'
```
## sweet다이얼로그가 참조할 문구를 만들어주자.
- value -> strings.xml에 작성하기
```
<resources>
    <string name="app_name">Testssssss</string>
    <string name="alert_warning">Warning!!</string>
    <string name="alert_error">Error!!</string>
    <string name="alert_success">Success!!</string>
</resources>
```
## SweetAlertActivity 코드 작성하기
```java
package com.lhj.ex_0725;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import com.ontbee.legacyforks.cn.pedant.SweetAlert.SweetAlertDialog;

public class SweetAlertActivity extends AppCompatActivity {

    SweetAlertDialog sweetAlertDialog;
    Button btn_warning, btn_error, btn_success;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sweet_alert);

        btn_warning = findViewById(R.id.btn_warning);
        btn_error = findViewById(R.id.btn_error);
        btn_success = findViewById(R.id.btn_success);

        btn_warning.setOnClickListener(click);
        btn_error.setOnClickListener(click);
        btn_success.setOnClickListener(click);
    }//onCreate()

    View.OnClickListener click = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            int id = view.getId();
            if(id == R.id.btn_warning) {
                sweetAlert("warning", getString(R.string.alert_warning), SweetAlertDialog.WARNING_TYPE, SweetAlertActivity.this);
            } else if (id == R.id.btn_error) {
                sweetAlert("error",getString(R.string.alert_error),SweetAlertDialog.ERROR_TYPE,SweetAlertActivity.this);
            }else if (id == R.id.btn_success) {
                sweetAlert("success",getString(R.string.alert_success),SweetAlertDialog.SUCCESS_TYPE,SweetAlertActivity.this);
            }
            }
        }

    //타입별 다이얼로그 호출 메서드
    public void sweetAlert(String msg, String title, int type, Context context){
        sweetAlertDialog = new SweetAlertDialog(context, type);
        sweetAlertDialog.setTitleText(title);
        sweetAlertDialog.setConfirmText(msg);
        sweetAlertDialog.setConfirmText("ok");

        sweetAlertDialog.show();
    }

}
```
- 각각의 버튼을 눌러서 다이얼로그를 확인해볼수 있다.
- 애니메이션 처리가 되면서 나오고 있다.
