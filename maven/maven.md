# Maven



## Introduction



## Build

- 소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정 / 결과물
- 소스코드(java) 및 프로젝트 파일과 자원 등(xml, jpg, jar, properties) 을 JVM or WAS가 인식할 수 있는 구조로 패키징



### What is maven

- Build Tool
  - 라이프 사이클 관리

- Dependency Management
  - 필요 라이브러리 및 해당 라이브러리의 의존 라이브러리까지 관리

- Repository system
- Plugin framework
- Best Practices



## Lifecycle



### Maven Building Blocks

- Goals

  - Tasks that do the work
    - 최소한의 실행 단위
  - Defined By plugins
    - 플러그인에서 실행할 수 있는 각각의 기능
  - Plugin Goal 실행
    - `mvn groupId:artifactId:version:goal`
    - `mvn plugin:goal`
- Phases

  - one or many goals
  - Part of lifecycle
    - Phase는 의존 관계를 가짐 => 해당 Phase 수행 전 이전 단계 Phase 모두 수행
- Lifecycle

  - start to finish collection of phases
  - different based on packaging type
  - executes all phases up to specific phase




### Phase 종류

- Default(Build)
  - 일반적인 빌드 프로세스 위한 모델
- Clean
  - 빌드 시 생성 파일 삭제
- Validate
  - 프로젝트가 올바른지 확인하고 필요한 모든 정보 사용 가능 여부 확인
- Compile
  - 프로젝트의 소스 코드를 컴파일
- Test
  - 유닛 테스트 수행
- Package
  - 실제 컴파일된 소스 코드 및 리소스 => jar, war 등에 배포를 위한 패키지로 만드는 단계
- Verify
  - 통합 테스트 결과에 대한 검사 실행 => 품질 기준 충족하는지 확인
- Install
  - 패키지를 로컬 저장소에 설치
- Site
  - 프로젝트 문서와 사이트 작성 / 생성
- Deploy
  - 만들어진 package를 원격 저장소에 release



### 최종 빌드 순서

- compile => test => package





## Maven 설징 파일



### settings.xml

- 메이븐 빌드 툴과 관련한 설정 파일
- MAVEN_HOME/conf 디렉토리에 위치 



### pom.xml

- POM
  - Project Object Model
  - Object-based representation of a build
- 역할
  - Project meta data
    - modelVersion : POM model 버전
    - parent : 프로젝트 계층 정보
    - groupId : 프로젝트 생성하는 조직의 고유 ID (일반적으로 도메인 이름 거꾸로)
    - artifactId
      - 파일 대표 이름
      - groupId내 유
    - version
      - 프로젝트의 현재 버전
      - 개발 中에는 SNAPSHOT을 접미사
    -  packaging
      - 패키징 유형
      - jar, war, ear 등
  - dependencies
    - Declare primary dependencies
    - Transitive dependencies

      - dependency of your dependency 까지 download

    - Downloading from repository

    - Caching locally

    - Scopes (compile time vs runtime)

      - dependency가 언제 필요한지 정의
  - Build settings
  - Inheritance / Super POM





## Maven Command



### command

- `mvn -version`
- `mvn --help`
- `mvn clean`
- `mvn package`
- `mvn clean package`
- `mvn verify`
  - run all test

- `mvn install`
  - build project & installs the result (JAR)

