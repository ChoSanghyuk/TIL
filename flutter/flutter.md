# Flutter



## 프로젝트 구조



### 기본 폴더

- build
  - `.gitignore`에 포함되어야 함

- lib

  - 소스 코드 포함되는 디렉토리

  - `main.dart`와 같은 진입점 및 다른 Dart 파일들이 위치

- pubspec.yaml

  - 프로젝트 이름, 설명, 버전, **의존성**, 정적 파일을 관리하는 설정 파일

  - vscode 기준 우측 상단에 다운로드 표기 있어서, 누르면 의존성 다운됨

  - `assets` 필드에 이미지 등 정적 파일 위치 표기해야 사용 가능

- pubspec.lock

  - 패키지 매니저가 의존성에 대한 정확한 버전을 관리하는 파일

  - `pubspec.yaml` 파일에 명시된 의존성 버전과 일치하도록 자동으로 생성



### structural pattern

####하위 layer

- data
  - storing
- domain | services
  - app logic
- presentation | pages | views | screens | ui | widgets
- providers | blocs | cubits | controllers

#### simple structure

```
lib/
│── main.dart
│── home_page.dart
│── widgets/
│   ├── custom_button.dart
│   ├── custom_card.dart
```

- main.dart: Entry point
- home_page.dart: Main screen
- widgets/: Reusable UI components

#### Feature-Based Structure

Each feature has its own folder, making it easy to scale

```
lib/
│── main.dart
│── features/
│   ├── auth/
│   │   ├── presentation/
│   │   │   ├── login_screen.dart
│   │   │   ├── signup_screen.dart
│   │   ├── domain/
│   │   │   ├── auth_repository.dart
│   │   ├── data/
│   │   │   ├── auth_api.dart
│   │   │   ├── auth_model.dart
│   ├── dashboard/
│   │   ├── presentation/
│   │   │   ├── dashboard_screen.dart
│   │   ├── domain/
│   │   ├── data/
```

- Each feature (e.g., auth, dashboard) has its own presentation, domain, and data layers.
- Aligns well with Clean Architecture.

#### Layered Architecture (MVC / MVVM / Clean Architecture)

A modular approach for medium-to-large projects:

```
lib/
│── main.dart
│── core/
│   ├── utils/
│   ├── constants.dart
│   ├── network.dart
│── data/
│   ├── models/
│   ├── repositories/
│── domain/
│   ├── entities/
│   ├── use_cases/
│── presentation/
│   ├── screens/
│   ├── widgets/
```

- core/: Global utilities (e.g., constants, network handling)
- data/: Data sources, APIs, and models
- domain/: Business logic, entities, and use cases
- presentation/: UI-related components

#### Modular Approach (for Large Apps)

Used in large-scale apps for better scalability

```
lib/
│── main.dart
│── modules/
│   ├── auth/
│   │   ├── auth_module.dart
│   │   ├── auth_controller.dart
│   ├── home/
│   │   ├── home_module.dart
│   │   ├── home_controller.dart
│── shared/
│   ├── widgets/
│   ├── services/
│── app.dart
```

- Each module has its own controllers and dependencies.

[flutter 프로젝트 구조](https://www.notion.so/flutter-1973d64c3d038009baf7efe405d8e4a8?pvs=21)