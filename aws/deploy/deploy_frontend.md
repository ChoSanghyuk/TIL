# Deploy Front End

<aside> ๐ก Vue ๋ฐฐํฌ ๋ฌธ์

## ์ค๋น์ฌํญ

### Vue project

- ์์ฑ๋ ํ๋ก์ ํธ

## deploy

### netlify

- ์ฌ์ดํธ ๋ก๊ทธ์ธ

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcc65b3c5-aa38-40a9-952d-634182bf7807%2FUntitled.png)

### project

- `Site settings` > `Build & deploy` > `Environment`

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3bb81c71-19fc-4e56-8b89-588c20f5016f%2FUntitled.png)

- build

```bash
npm run build
```

- dist ํด๋ ์์ฑ ํ์ธ
- distํด๋ ์๋ก๋

## ๋๋ฉ์ธ์ฐ๊ฒฐ

- Site settings > Domain management > Domains >Add custom domain

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd4dae6b7-2365-4e22-849f-864120f342dd%2FUntitled.png)

- check DNS configuration

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F64661659-0f8a-43e6-bc83-adcc3e1e0907%2FUntitled.png)

- IP ์ฃผ์ ํ์ธ

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9a0ca071-e82b-4f8e-8b11-5c1ccb50d36c%2FUntitled.png)

- route53 ์ค์ 

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0f7b6e01-80cf-4ef8-aa5d-70fd5605c3d4%2FUntitled.png)

- ์๋ก๊ณ ์นจ ํ check ํ์ธ

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c1f941e-09ac-4c19-bc1f-7edcb35acc96%2FUntitled.png)

- SSL > Verify DNS configuration

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6410ab9a-1bc0-4247-a329-5d9625767d20%2FUntitled.png)

- ์ผ์ ์๊ฐ ํ ์ธ์ฆ ์๋ฃ

  ![img](deploy.assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff7f3c26f-0968-4429-a7eb-4e2a1d8ffdb3%2FUntitled.png)