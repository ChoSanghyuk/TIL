# Claude Code



## 설치



1. **Node.js 및 npm** 설치
2. 클로드 코드 설치

```bash
npm install -g @anthropic-ai/claude-code
```

3. 인증

```bash
cd {project path}
claude
```



## 세팅



### `CLAUDE.md`

- 개요

  - CLAUDE.md 파일은 Claude Code가 프로젝트를 이해하는 데 사용하는 "메모리" 역할 수행
  - 프로젝트 루트에 CLAUDE.md가 있으면 매번 컨텍스트를 설명할 필요가 없이, 기본적으로 CLAUDE가 읽어 기본 명령으로 인식
  - 팀원들과 공유해서 모든 사람이 같은 컨텍스트로 Claude Code를 사용 가능

- 주의 사항

  - **Less (instructions) is more**
    - 작은 LLM 모델일 수록 일정 수준 instruction이 넘어갔을 때 극격한 성능 저하 발견. 큰 모델도 성능 저하는 존재
    - instruction을 일정 수준 넘게 입력했을 경우엔, 단순히 신규 명령을 무시하는 것이 아니라 기존 명령도 똑같이 무시
    - general consensus is that **< 300 lines is best**, and shorter is even better.

  - LLM will perform better on a task when its' context window **is full of focused, relevant context**
    - including  examples, related files, tool calls, and tool results
  - Bloated CLAUDE.md files cause Claude to ignore your actual instructions!
    - 매 라인에 대해서 ""정말 필요한 줄인가?" 물어보고 아니라면, 과감하게 삭제.
  - You can tune instructions by adding emphasis to improve adherence.
    - ex. “IMPORTANT” or “YOU MUST”

- the principle of **Progressive Disclosure** 
  - keep task-specific instructions in *separate markdown files* with self-descriptive names
  - you can include a list of these files with a brief description of each, and instruct Claude to decide which (if any) are relevant and to read them before it starts working.
  - **include `file:line` references** (Prefer pointers to copies)
  - 예시

    ```
    agent_docs/
      |- building_the_project.md
      |- running_tests.md 
      |- code_conventions.md
      |- service_architecture.md
      |- database_schema.md
      |- service_communication_patterns.md
    ```

- 포함 & 제외시킬 내용

  | ✅ Include                                            | ❌ Exclude                                          |
  | :--------------------------------------------------- | :------------------------------------------------- |
  | Bash commands Claude can’t guess                     | Anything Claude can figure out by reading code     |
  | Code style rules that differ from defaults           | Standard language conventions Claude already knows |
  | Testing instructions and preferred test runners      | Detailed API documentation (link to docs instead)  |
  | Repository etiquette (branch naming, PR conventions) | Information that changes frequently                |
  | Architectural decisions specific to your project     | Long explanations or tutorials                     |
  | Developer environment quirks (required env vars)     | File-by-file descriptions of the codebase          |
  | Common gotchas or non-obvious behaviors              | Self-evident practices like “write clean code”     |



### 주요 명세 문서

- `spec.md` : Feature specification (user stories, requirements)

- `plan.md` : Implementation plan (architecture, tech context)
- `research.md` : Technical research findings
- `tasks.md` : implementation checklist

```
specs/001-liquidity-repositioning-agent/
  ├── spec.md                         # Feature specification (user stories, requirements)
  ├── plan.md                         # Implementation plan (architecture, tech context)
  ├── research.md                     # Technical research findings
  ├── data-model.md                   # Entity definitions and relationships
  ├── quickstart.md                   # User setup and usage guide
  ├── tasks.md                        # implementation checklist
  └── checklists/
      └── requirements.md             # Spec quality checklist
```



### 권한

- 권한이 주어진 작업에 대해서는 수행 전 y/n 여부 질의 생략

- 설정 방법

  - `/permissions` 명령어 사용
    - 이후 안내에 따라서 권한 설정

  - `claude/settings.local.json` 파일 수정

    - 룰 예시 (`settings.local.json` 예시)

      ```json
      {
        "permissions": {
          "allow": [
            "ReadFile(**)", 
            "LS(**)",
            "Glob(**)",
            "Find(**)",
            "Search(**)",
            "Grep(**)",
            "Bash(cat:*)",
            "Bash(head:*)",
            "Bash(tail:*)",
            "Bash(grep:*)"
          ],
          "deny": []
        }
      }
      
      ```

  - `claude allow` - 도구 허용 목록 (Allowlisting Tools)

    - clalude가 사용 할 수 있는 명령어들을 사전에 허용해준다.
      - 이후 권한 요청 없이 claude가 자체적으로 사용 가능
    - 사용 예시
      - claude allow "git commit” : `git commit` 허용
      - claude allow edit : 문서 편집 허용

- 전체 권한 허용

  - `claude --dangerously-skip-permissions` (claude 기동할 때 옵션 부여) 



### 명령어 등록

- `.claude/commands`  파일 아래에 {명령어}.md 로 파일 생성 후 파일 안에 명령어에 대한 설명 작성

- 클로드 스킬 활성화 되어있어야 함
  - 설정 > 기능 > 스킬 > skill-creator 활성





## 사용



###  Git 작업

Git 작업 흐름 관리는 시간이 많이 소요될 수 있지만, 클로드 코드는 커밋 만들기, 병합 충돌 해결 및 풀 리퀘스트(프리퀀스) 생성과 같은 **Git 작업을 자동화**할 수 있습니다.

```bash
commit
```



### 자연어 명령

**자연어 명령**을 사용하여 작업을 수행

```bash
> explain what the function in file.js does
```



### context

- 개요
  - 현재 대화에서 '기억'하고 참조할 수 있는 모든 정보를 의미
  - 이전 대화 기록, 업로드한 파일, 시스템 지시사항 등이 모두 포함
- 특징
  - Claude 3.5 Sonnet 등 최신 모델은 최대 **200,000 토큰**의 컨텍스트 윈도우를 제공
    - 수백 페이지 분량의 책이나 대규모 코드베이스를 한 번에 입력할 수 있는 크기
  - **성능의 스윗 스팟(Sweet Spot)은 2만 토큰 미만**
    - **토큰 환산:** 약 **5,000 ~ 20,000 토큰** 정도일 때 Claude가 가장 기민하고 정확하게 반응 (A4 10~30페이지)
  - LLM performance degrades as context fills
    - Claude may start “forgetting” earlier instructions or making more mistakes. 
    - The context window is the most important resource to manage

- 사용
  - 반복적인 작업은 메시지가 아닌 skills 혹은 파일로 별로 관리
  - 작업에 필요한 파일 및 명령만 들어가도록 관리 필요
  - 신규 작업 시작 시 `/clear`로 context 초기화 필요
