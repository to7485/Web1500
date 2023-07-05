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
