# Jenkins



## 개요





### CICD 개요

- CI : Countinous Integration
  - integrate their code changes into a shared repository
  - Each integration is verified by an **automated build and automated tests** to detect and address issues as early as possible
- CD
  - Continous Delivery
    - construct software environment that it can be deployed at any time, 
    - but deploy the image manually in the production environment
  - Continous Deployment
    - deployed automatically

:bulb: git 자체는 CI가 아님. 단지, CI를 수행하기 위한 main tool로 사용됨



### Jenkins 장점

- For a very large ecosystem, Jenkins is known to scale to a good extent. 
  - It has the capacity to run some jobs in parallel, and this can easily be configured. 
  - While designing a highly available system, it’s essential to make sure CI/CD is also available as this will be a hindrance in the development workflow. 

- Another great place to use CI/CD is when you’re building images for multiple operating systems
  - it’s essential to create Docker images separately for both of these. 
  - Jenkins provides an interface where you can build the image on a certain operating system and environment.











## 설치 및 설정

처음 설치될 때, 로그로 비밀번호 찍힘. 



install some plugins from Manage Plugins. Go ahead and install Go, Docker, and the Docker plugin from the Available tab in Manage Plugins.

configure these plugins globally by going to `Manage Jenkins -> Global Tool Configuration.`

https://mattermost.com/blog/how-to-set-up-a-jenkins-ci-cd-pipeline-for-your-golang-app/







## 배포 자동화 구축



### 종류

- a freestyle project
  - the multibranch pipeline is used for a repository with multiple branches





### 개인 프로젝트에 jenkins 미도입 사유

- git push 시, 배포 자동화는 github webhook으로 충분히 가능
- jenkins를 사용하고자 한다면, 
  - free style project - 실행 후 session을 죽여서 프로젝트도 같이 따운 됨(`nohup`, `BUILD_ID=dontKillMe` 로도 해결 X)
  - pipeline - 기본적으로 github 읽어서 실행. config만 별도로 읽도록 설정하기 귀찮고, 배포 자동화 때문만을 위해 docker 사용할 이유 없음

