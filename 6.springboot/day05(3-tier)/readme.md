# 3-tier
- Presentation Tier,Business Tier,Presistence Tier를 각각 물리적으로 독립된 모듈로 개발하고 유지하는 구조를 말한다.
- 스프링 프로젝트는 3-tier방식으로 구성한다.

![image](https://github.com/to7485/Web1500/assets/54658614/aef7e187-ffdb-4ab8-a287-b27b6ef368a9)

## tier의 종류

### Presentation Tier - 화면계층
- 화면에 보여주는 기술을 사용하는 영역
- Controller에서 사용자의 요청에 맞는 응답처리를 진행한다
- HTML엔진(Thymeleaf), HTML등이 담당하는 영역이다.
- 화면 구성이 이에 속한다.

### Business Tier - 비즈니스 계층
- 순수한 Business Logic을 담고있는 영역
- Presentation tier와 presistence tier의 bridge 역할을 한다.
- 고객이 원하는 요구사항을 반영하는 계층이다.
- 서비스에 있어서 가장 중요한 영역이다.
- 이 영역의 설계는 고객의 요구사항과 정확히 일치해야 한다.
- Service영역이다.(로그인, 마이페이지, 회원가입)
- 예를들어 쇼핑몰에서는 상품 구매시 포인트 적립을 가정한다면, Presistence tier의 설계는 '상품','회원'으로 나눠 설계하지만<br> Business tier는 상품 영역과 회원 영역을 동시에 사용해서 하나의 로직을 처리하게 된다.이 때 하나의 서비스에 필요한 여러 개의 쿼리 메서드를 하나로 묶어주는 역할이 필요한데, 이를 Service 객체로 사용한다.

### Presistence Tier - 영속 계층
- 데이터를 어떤 방식으로 보관하고, 사용하는 가에 대한 설계가 들어가는 계층
- 일반적으로 DBMS를 많이 사용한다.
- Presistence tier는 DBMS를 기준으로, Business tier는 Logic을 기준으로 처리한다.

각 영역은 독립적으로 설계되어 나중에 특정한 기술이 변하더라도 필요한 부분을 전자제품의 부품처럼 쉽게 교환할 수 있게 하자는 방식이다.<br> 각 연결부위는 인터페이스를 이용해서 설계하는 것이 일반적인 구성방식이다.

![image](https://github.com/to7485/Web1500/assets/54658614/c9fb3ccf-8cc0-4600-a0f8-9cce7d1b1cd4)

# 3-tier vs MVC

## 디자인 패턴
- 물릭적으로 구분할 수 없는 설계를 문서화 하면서 고안된 방법
- 프로그램의 규모가 커질수록 다양한 문제를 겪게 되는데, 이 문제들을 효율적으로 해결할 수 있게 유형별로 패턴화 한 것

## 차이점
- **3-tier 아키텍처** 는 물리적인 공간을 기준으로 역할을 구분
- **MVC 패턴**은 소프트웨어 디자인패턴 중 하나로 하나의 컴포넌트간 역할 분담

우리는 'MVC 패턴을 적용하였고 3-tier 구조로 되어있는 프레임워크를 갖추고 있다.'라고 할 수 있다.

![image](https://github.com/to7485/Web1500/assets/54658614/ce86031e-de2c-46c5-a575-1337e92108dc)
