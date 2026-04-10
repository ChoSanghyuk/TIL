# git spec kit



https://github.com/github/spec-kit?tab=readme-ov-file



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

   - `git` 사용이 권장되면, git을 사용하지 않을 경우 `--no-git` 옵션을 추가해서 `init`



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

