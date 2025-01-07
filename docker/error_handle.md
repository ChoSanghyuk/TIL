



### credential error

```
error getting credentials - err: exit status 1, out: ``
```

- 1안
  -  `docker logout` & `docker login`
- 2안
  - `vi ~/.docker/config.json`
  - `"credsStore": "desktop",` => `"credsStore": "osxkeychain",`





### platform diff error

```
WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
```

- 원인

  - `linux/amd64` architecture 기반의 이미지를 `linux/arm64/v8` architecture 에서 실행할 경우 문제 발생
    - linux/amd64 : x86 processors, such as Intel or AMD
    - `linux/arm64/v8` : Apple Silicon/M1/M2, or other ARM processors

- 해결

  1. install Rosetta2

     - `softwareupdate --install-rosetta`

  2. docker run시, platform flag 지정

     - teminal launch

       - `docker run --platform linux/amd64 <image-name>`

     - docker compose launch

       ```yaml
       services:
         my_service:
           image: <image-name>
           platform: linux/amd64
       ```



:question: Install Docker's `qemu-user-static` to emulate architectures.

```bash
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```

- 필요한지 추후 확인



