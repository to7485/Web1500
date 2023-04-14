## 싱글톤 패턴
- jsp 파일을 생성하고 DB에 접속하려고 할 때마다 객체들을 생성하여 메모리를 점유해야 한다.
- 코드를 매번 작성해야 하고 비효율적이다.

### 싱글톤
- 어플리케이션이 시작될 때 어떤 클래스가 최초 한번만 메모리를 할당하고(static의 개념) 그 메모리에 인스턴스를 만들어 사용하는 디자인 패턴이다.
- DB를 접속할 때 처럼 공통된 객체를 여러개 생성해서 사용해야 하는 상황에 많이 사용된다.

### jdbc_templ.xml 등록하기

1. windows -> Preferences

![image](https://user-images.githubusercontent.com/54658614/231664981-f3da563c-173f-45f9-adc8-4584b230f24a.png)

2. java -> Editor -> Templates

![image](https://user-images.githubusercontent.com/54658614/231665073-d25838c1-9ad6-4b0d-b038-a1034aca3172.png)

3. jdbc_templ.xml 열기

![image](https://user-images.githubusercontent.com/54658614/231665119-e13790d4-4f11-4d55-b8ff-916004635340.png)

4. apply and close

![image](https://user-images.githubusercontent.com/54658614/231665175-f74639cf-3ca4-4273-8714-32feee08430c.png)
