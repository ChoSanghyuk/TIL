# Web Framework



## framework란

- 틀 안에서 일하는 것

- 자유도가 제한되지만 핵심 기능들이 이미 구현되어 있어, 다시 생성할 필요 없음

  => 재사용할 수 있는 수많은 코드를 프레임워크로 통합함으로써, 개발자가 새로운 애플리케이션을 위해 기능을 구현할 필요 없음



## Framework Architecture

- MVC Design Pattern (model-view-controller)
- 사용자 인터페이스로부터 프로그램 로직을 분리하여 애플리케이션의 시각적 요소나 이면에서 실행되는 부분을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음

 @ Design Pattern

​	: 소프트웨어 설계에서 가장 효과적이라 생각되는 작업이 정형된 것

### MVC

- Model
  - 데이터 구조를 정의하고, db의 기록을 관리
- View
  - 실제 내용을 보여주는데 사용
- Controller
  - HTTP 요청을 수신하고 HTTP 응답을 반환
  - Model을 통해 요청을 충족시키는데 필요한 데이터에 접근