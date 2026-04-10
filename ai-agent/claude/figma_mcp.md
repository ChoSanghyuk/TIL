# Claude Mcp Figma Setting



## 세팅

### 준비 작업

- claude code 설치
- figma “dev mode” 권한을 가진 구독 요금제 필요
- dev mode 계정이 주인인 figma 프로젝트에서 “Ready for dev” 활성화



### figma mcp 추가

```bash
claude mcp add --transport http figma-remote-mcp https://mcp.figma.com/mcp
```



##주요 작업 과정



### react 프로젝트 생성

```
npx create-react-app . [--template typescript]
```



### figma mcp acll

```bash
figma-remote-mcp - get_design_context (MCP)(fileKey: "5FFKhd761i05QcP2mYX1OS", nodeId: "1:3556", clientLanguages: "javascript,typescript", clientFrameworks: "react")
```

- `fileKey`와 `nodeId`는 figma에서 component 눌렀을 때 나오는 url.
- 각 파라미터들을 넣어주지 않고, component url만 던져주면서 화면 구현하라고 하면 자동으로 위의 함수 사용





## 참고자료

- https://bcho.tistory.com/1492



