# SDD + CLAUDE CODE 가이드 문서



## 소개



### SDD (Spec Driven Development)

- **요구 사항 정의서(Specification)**를 중심으로 개발을 진행하는 방식 (명세(Spec)가 구현을 **선도**)

- 개발 시작 전, 모든 팀 구성원(개발자, 디자이너, QA 등)이 **명세(Spec)에 대한 동일한 이해**를 공유하고, 이에 따라 코드가 작성되므로 **오류와 재작업을 최소화**하여 **개발 효율성을 극대화**한다.



### SDD + AI Agent

- 개요

  - 명세(Spec)를 개발의 **단일 진실 공급원(Single Source of Truth)**으로 세우고, AI가 모든 단계를 명세에 **강제적으로 일치**시킴

- 기존 바이브 코딩의 한계

  - AI는 개발자의 요구사항을 완전히 이해하기 전에 즉시 코드를 작성하여 프로젝트의 목적을 벗어난 코드 작성
  - 실제 의도에 맞추기 위해 반복적으로 코드 수정 요청

- SDD의 해결책

  - 프로젝트의 모든 요구사항과 설계를 문서(`spec`)로 명확히 정리
  - AI는 해당 문서(산출물)을 지속적으로 참고해 일관성있는 개발 진행
  - 프로젝트 변경 사항을 문서에 반영, AI는 변경된 문서를 바탕으로 코드 업데이트

  =>  큰 프로젝트에서 AI 에이전트를 효과적으로 활용할 수 있는 관점



## Claude Code



### 설치

1. **Node.js 및 npm** 설치

   - 윈도우 : https://studyhard24.tistory.com/1426
     - 설치 완료 후 cmd 창에서 `npm -v` 로 설치 확인
   - mac : https://favonius.tistory.com/30

2. 클로드 코드 설치 (터미널 수행)

  ```bash
  npm install -g @anthropic-ai/claude-code
  ```

3. 실행 (터미널 수행)

  ```bash
  cd {project path}
  claude
  ```

  - `claude --dangerously-skip-permissions` : claude가 모든 작업에 대해서 수행 여부(y/n)를 물어보지 않고 작업 수행

4. 권한 부여 (optional)

   - 권한이 주어진 작업에 대해서는 수행 전 y/n 여부 질의 생략
   - `/permissions` 명령어 수행 후 허용해 줄 권한 등록
     - ex) `"ReadFile(**)"` ,  `Find(**)` , `Grep(**)` , ...

   



### `CLAUDE.md`

- 개요

  - CLAUDE.md 파일은 Claude Code가 프로젝트를 이해하는 데 사용하는 "메모리" 역할 수행
  - 프로젝트 루트에 CLAUDE.md가 있으면 매번 컨텍스트를 설명할 필요가 없이, 기본적으로 CLAUDE가 읽어 기본 명령으로 인식
  - 팀원들과 공유해서 모든 사람이 같은 컨텍스트로 Claude Code를 사용 가능
- 포함 내용
  - **WHAT**: 프로젝트에 사용된 **핵심 기술**과 **기술 스택**을 설명
    - 백엔드 - Java/Spring Boot, FastAPI, PostgreSQL
    - 프론트엔드 - React, TypeScript, Tailwind CSS
  - **WHY**: 프로젝트가 **해결하고자 하는 문제** 또는 **달성하고자 하는 주요 비즈니스 목표**
  - **HOW**: **어떻게** 코드를 작성하고 프로젝트에 기여해야 하는지에 대한 **지침과 제약 사항**
    - **코딩 표준** (팀의 **코딩 스타일** 및 **컨벤션**) 제시
    - 역할 제시 : 신규 기능 구현, 버그 수정, 테스트 코드 작성
- 작성 규칙
  - 프로젝트 전체 룰과 관련된 내용으로만 작성
    - 프로젝트와 연관 없는 내용이 있는 경우, 전체 파일이 무시될 수 있음
  - 파일 길이는 300줄 이내로 작성
    - 특정 길이보다 길어질 경우, 성능 저하 발생
  - 점진적 공개 원칙 (the principle of **Progressive Disclosure** )
    - 지침별로 별도의 파일을 구성 후, 파일 위치(pointer)와 간단한 설명만 CLAUDE.md에 포함
    - CLAUDE가 필요에 따라 지침을 선택적으로 가져다 읽을 수 있게 함



### 주요 명세 문서

```
specs/{feature 1}/
  ├── spec.md                         # Feature specification (user stories, requirements)
  ├── plan.md                         # Implementation plan (architecture, tech context)
  ├── research.md                     # Technical research findings
  ├── data-model.md                   # Entity definitions and relationships
  ├── quickstart.md                   # User setup and usage guide
  ├── tasks.md                        # implementation checklist
  └── checklists/
      └── requirements.md             # Spec quality checklist
```

- **`spec.md`** : **기능 명세**. 사용자 스토리와 구체적인 요구 사항을 정의한 문서

- `plan.md` : 구현 계획. 아키텍처 및 사용될 기술적 맥락(기술 배경)을 정리한 문서
- `research.md` : 기술 조사 결과. 프로젝트 관련 선행 기술 연구 및 검토 결과를 담은 문서
- `tasks.md` : 구현 체크리스트. 완료해야 할 개별 작업 항목들을 목록으로 정리한 문서



### 명령어 실행

- `claude` 모드를 실행한 상태에서, 작성한 명세 파일에 대한 위치와 설명을 명시하면서 코드 작성을 지시

  ```
  specs/auto-onramp/ 하위 파일들을 참고하여, auto-onramp 기능을 작성해라. 기능에 대한 명세는 spec.md, 완료해야할 작업들은 tasks.md에 명시되어 있다. tasks.md에서 완료한 작업들에 대해서는 완료 mark로 update한다.
  ```

  



## Git Spec kit (optional)



### 개요

- SDD 방식을 돕기 위해 GitHub에서 공개한 툴킷
- 내장된 `.sh` 실행 파일과 Skills 기능을 통해, 연결된 AI agent를 사용하여 명세 문서를 체계적으로 작성



### 설치

1. `uv` 명령어 설치
   - window (**PowerShell**) : `irm https://astral.sh/uv/install.ps1 | iex` 
   - mac : `brew install uv`
     - warning으로 환경 변수 추가해야한다고 띄면, 그대로 수행 (혹은 터미널 환경 변수 설정)

2. `specify` 명령어 설치
   
   - `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git`
   
3. 실행

   ```bash
   # 신규 프로젝트
   specify init <PROJECT_NAME> --ai claude
   
   # 기존 프로젝트
   specify init . --ai claude
   ```

   - `git` 사용이 권장.
     - git을 사용하지 않을 경우 `--no-git` 옵션을 추가해서 `init`



### 사용

- 개발 과정을 기본 원칙 설정 + 4단계로 나누어서 실행

0. `/speckit.constitution {프로젝트 개요}` 
   - 기본 원칙 설정 단계. CLUADE.md` 와 같은 프로젝트의 전체적인 룰에 대해 작성한다.
   - 프로젝트 생성 후 한번만 수행
   
   - 생성 파일 : `.specify/memory/constitution.md` 
   
1. `/speckit.specify {기능 설명}`
   - 프로젝트의 **'무엇(What)'**과 **'왜(Why)'**에 초점을 맞춰 사용자 스토리 및 요구 사항을 상세히 정의
   - 생성 파일 : 
     - `specs/{feature}/spec.md`
     - ``specs/{feature}/checklists.md` : 다음 단계 수행 전, 개발자가 확인해야할 체크리스트
   
2. `/speckit.plan {기술 스택 및 아키텍처}`
- 사양을 기반으로 **기술 스택, 아키텍처** 등 **'어떻게(How)'**를 구체화하는 계획을 수립
   - 생성 파일 : 
     - `specs/{feature}/plan.md`
     - `specs/{feature}/data-model.md` : entity 설계도
     - `specs/{feature}/research.md` : 프로젝트에 필요한 내용에 대해서 AI-agent가 자체적으로 인터넷에서 조사한 결과

3. `/speckit.tasks`

   - 큰 계획을 **독립적으로 구현, 검토, 테스트**할 수 있는 **작고 구체적인 작업 단위**로 분해

   - `specs/{feature}/tasks.md` 

4. `/speckit.implement`

   - 소스 생성 명령어.
   - `specs/{feature}/tasks.md` 에 정의된 작업 목록 **전부 실행**



### Git Spec Kit vs 자체 명세 문서 작성 

| **Git Spec Kit 사용**                                        | **자체 명세 문서 작성**                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| AI 에이전트와 전용 명령어로 자동화된 작성. 정형화된 파일로 분리 | 수동으로 문서를 작성. 비정형화된 문서 형식                   |
| 기능 설명을 토대로 필요한 내용을 자체적으로 생성해서 작성    | 수행자가 인식하는 요구 사항 내용에 국한되어 작성             |
| AI 에이전트가 작성하는 명세 파일이 때로는 개발자가 인식하는 **프로젝트 범위를 넘어가거나** 불필요한 내용을 포함하는 경우 존재 | 명세 작성자가 **직접 범위를 통제**하므로, 문서가 프로젝트의 의도된 범위 내에 머무른다. |
| 방대한 명세 파일을 단기간 내 작성. 극심한 토큰 사용          | 시간 소요. 토큰 소모 비용 없음                               |



### 권장 환경

- Git Spec Kit 사용 권장
  - 기능 명세의 큰 방향성은 나왔지만 세밀한 제약 조건과 요구사항은 없는 경우 (자유도가 높은 환경)
  - 선제적으로 AI agent가 작성한 문서를 수행자가 요구 조건에 맞게 중간중간 직접 수정이 가능한 경우
  - 크고 복잡한 기능을 AI가 end-to-end까지 독자적으로 개발이 가능한 경우
- 자체 명세 문서 작성 사용 권장
  - 세밀한 제약 조건과 요구사항을 모두 명시 해야하는 경우
  - 인식 가능한 범위 내로 AI 사용을 제한하고 싶은 경우
  - 작은 기능을 개발하는 경우



:bulb: 현재 프로젝트 환경에서는 자체 명세 문서 작성을 기본으로 하고, 경우에 따라서 git spec-kit 사용을 권장

- 기획자 : user story, business rule 작성( `spec.md` 문서 )
- 개발자 : 함수 시그니처, input/output 제약, error cases, test cases 작성 (`spec.md`문서 혹은 `data-model.md`, `api.md` 등 별도 작성)
  - 별도 파일로 작성할 경우, spec.md 파일 내에 파일 위치(pointer) 명시
- 품질 : `spec.md` 등 명세 문서 검토

