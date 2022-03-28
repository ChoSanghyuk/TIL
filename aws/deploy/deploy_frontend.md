# Deploy Front End

<aside> 💡 Vue 배포 문서

## 준비사항

### Vue project

- 완성된 프로젝트

## deploy

### netlify

- 사이트 로그인

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcc65b3c5-aa38-40a9-952d-634182bf7807%2FUntitled.png)

### project

- `Site settings` > `Build & deploy` > `Environment`

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3bb81c71-19fc-4e56-8b89-588c20f5016f%2FUntitled.png)

- build

```bash
npm run build
```

- dist 폴더 생성 확인
- dist폴더 업로드

## 도메인연결

- Site settings > Domain management > Domains >Add custom domain

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd4dae6b7-2365-4e22-849f-864120f342dd%2FUntitled.png)

- check DNS configuration

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F64661659-0f8a-43e6-bc83-adcc3e1e0907%2FUntitled.png)

- IP 주소 확인

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9a0ca071-e82b-4f8e-8b11-5c1ccb50d36c%2FUntitled.png)

- route53 설정

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0f7b6e01-80cf-4ef8-aa5d-70fd5605c3d4%2FUntitled.png)

- 새로고침 후 check 확인

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c1f941e-09ac-4c19-bc1f-7edcb35acc96%2FUntitled.png)

- SSL > Verify DNS configuration

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6410ab9a-1bc0-4247-a329-5d9625767d20%2FUntitled.png)

- 일정시간 후 인증 완료

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff7f3c26f-0968-4429-a7eb-4e2a1d8ffdb3%2FUntitled.png)