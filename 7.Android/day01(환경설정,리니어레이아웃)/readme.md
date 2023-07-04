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

