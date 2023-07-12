# Intent Flag
- 두가지 액티비티를 이동하는걸 봤는데 액티비티가 겹치기 때문에 다시 돌아올 때는 intent를 사용하지 않았었다.
- Intent Flag를 사용하면 액티비티가 겹치는 문제를 해결할 수 있다.

## IntentFlag를 언제 사용하는가?
- 안드로이드에서 Activity를 호출하다보면 발생하는 Activity의 중복문제나 흐름을 제어해주고 싶을 때 Flag를 사용합니다.

## Task
- Task는 어플리케이션에서 실행되는 액티비티를 기록하는 스택이다.
- 안드로이드에서는 Task를 이용해서 화면의 순서와 흐름을 관리할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/4abb0a4e-6d83-48d3-a451-ce5f3bd3ef73)

## IntentFlag 사용하는법
### AndroidManifest에서 제어

![image](https://github.com/to7485/Web1500/assets/54658614/7f71e540-a0b6-4087-9ee5-6e0651f71277)

  - \<activity android:launchMode= "사용하고자 하는 속성"\></> 
    -  standard : 이 속성은 별도의 Task를 생성하지 않고 해당 Task에 계속 쌓아나갑니다.<br>(Default값은 standard입니다.)
    -  singleTop : 이 Flag를 Activity에 설정하면 Task의 Top에 생성하려는 Activity가 존재하는 경우<br> 새로운 Activity를 Top에 올리지않고 기존의 Activity를 재사용합니다.<br>(여기서 재사용이란 Activity가 재사용되서 활성화가 될때 Activity의 Instance가 생성되는 것이 아니라<br> 단지 하나의 B Instance가 onPause() -> onNewIntent() -> onResume() 생명주기만을 반복하는 것을 의미합니다.)
      -  singleTask : RootActivity로만 존재하며 하나의 인스턴스만 생성 가능합니다.<br>(다른 Task에서도 동일한 Activity 실행 불가) but, 다른 액티비티 실행시 동일 Task내에서 실행 가능합니다.
      -  singleInstance : 루트 액티비티로만 존재하며 하나의 인스턴스만 생성가능하고 태스크내에 해당 액티비티 하나만 속할 수 있어 다른 액티비티를 실행시키면 새로운 Task가 생성되어 (FLAG_ACTIVITY_NEW_TASK와 동일) 그 Task내에 포함됩니다.
   
### 소스코드 Flag 제어
  - Intent.addFlags() : 새로운 flag를 기존 flag에 붙임
  - Intent.setFlags() : 오래된 flag 전체를 대체
### 소스코드 Flag에 들어가는 매개변수
- FLAG_ACTIVITY_BROUGHT_TO_FRONT
  - 시스템 디폴트값, 같은 Task에 Activity가 있을 경우에 Activity 실행모드가 singleTask이면 자동으로 설정됨
- FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET
  - Task가 리셋될 때 플래그가 사용된 Activity부터 위의 Activity가 모두 삭제됨
- FLAG_ACTIVITY_CLEAR_TOP
  - 호출하는 Activity가 스택에 있을 경우, 해당 Activity를 최상위로 올리면서, 그 위에 있던 Activity들을 모두 삭제하는 Flag<br>예) ABCDE가 존재하는 상태에서 C를 호출하게 되면 ABC만 남는다.
- FLAG_ACTIVITY_SINGLE_TOP
  - 호출되는 Activity가 최상위에 존재할 경우에는 해당 Activity를 다시 생성하지 않고, 존재하던 Activity를 다시 사용

## IntentSubActivity 수정하기
```java
@Override
public void onClick(View view) {
     Intent i = new Intent(IntentSubActivity.this,IntentActivity.class);

     //중복된 페이지를 걸러내는 플래그 추가
    i.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP |Intent.FLAG_ACTIVITY_SINGLE_TOP);
     startActivity(i);

}
```

![image](https://github.com/to7485/Web1500/assets/54658614/c0013d37-da77-4754-9b70-cbbb63bced55)

- C.T 속성이 지금 액티비티 상에서 가장 위에 있는 액티비티를 지워준다.
- Intent 때문에 위에 메인액티비티가 쌓이는 상황이 올 수 있다.
- S.T 속성이 액티비티가 존재할 경우 만들어진 액티비티를 다시 사용하도록 하게 해준다.
- 화면을 전환하는 곳에서 의식적으로 플래그를 쓰게되면 전환해야 하는 액티비티가 몇개든 간에 겹치는 상황을 막을 수 있다.

# Handler

