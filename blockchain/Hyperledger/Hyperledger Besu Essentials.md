# Hyperledger Besu Essentials



## Installing Besu (Linux)



java 설치

`sudo apt install openjdk-17-jdk-headless`



ubuntu에서

```sh
cd /mnt
```

할 경우, 내 윈도우 디스크 접근 O

아래 주소에서 zip 다운로드

https://github.com/hyperledger/besu/releases



압축풀고 ubuntu에서 해당 파일로 이동한뒤 설치 여부 확인

```sh
cd besu-24.1.0/
./bin/besu --help
```







## Installing Besu (Windows)



### 사전 세팅

- [Have a 64-bit version of Windows installed](https://support.microsoft.com/en-us/windows/which-version-of-windows-operating-system-am-i-running-628bec99-476a-2c13-5296-9dd081cdd808#:~:text=Select the Start button > Settings > System > About .&text=Under Device specifications > System type,Windows your device is running) 
- [Download a 64-bit version of JDK/JRE](https://www.oracle.com/java/technologies/javase-downloads.html). We recommend that you also remove any 32-bit JDK/JRE installations.
- Download [Git for Windows](https://git-scm.com/downloads) if you do not already have it installed.



### Besu 설치

- Install Besu

  :bulb: 사전에 git bash 사전 설정 필요 (파일 경로 제한 해제)

  ```sh
  git config --system core.longpaths true
  ```

  - 설치

  ```sh
  git clone --recursive https://github.com/hyperledger/besu
  ```

- Build Besu

  ```sh
  export JAVA_HOME="<java경로>"
  cd besu
  ./gradlew build -x test
  cd build\distributions
  tar -xzf besu-<version>.tar.gz
  cd besu-<version>
  ```

  - `<java경로>` :  java가 설치된 경로 기입 (ex. `C:\Program Files\Java\jdk-17`)
  - `<version>`  : Besu 버전 설치

- Confirm Installation
  
  ```sh
  bin\besu --help
  ```
  
  