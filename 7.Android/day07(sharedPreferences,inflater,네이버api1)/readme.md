# sharedPreferences
- 안드로이드 앱 개발을 진행하다 보면, 앱의 데이터들을 저장하여 관리해야 할 상황이 존재한다.
- 간단한 설정 값이나 문자열 같은 데이터를 저장하기 위해 DB를 사용하기는 부담스럽기 때문에 SharedPreferences를 사용하는 것이 적합하다.

## sharedPreferences의 특징
- 보통 초기 설정값이나 자동 로그인 여부 등 간단한 값을 저장하기 위해 사용
- Application에 파일 형태로 데이터를 저장한다.
- Application이 삭제되기 전까지 저장한 데이터가 보존된다.
- Key-value 방식

### 객체 생성법
```java
SharedPreferences pref = getSharedPreferences("SHARE", MODE_PRIVATE);
```
### MODE의 종류
- MODE_PRIVATE : 생성한 Application에서만 사용 가능하다.
- MODE_WORLD_READABLE : 외부 App에서 사용 가능, But 읽기만 가능
- MODE_WORLD_WRITEABLE : 외부 App에서 사용 가능, 읽기/쓰기 가능
