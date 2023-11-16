# python으로 서버 만들기



## 개요

:green_apple: python을 이용해 간단한 서버를 구축하는 방법



## 사용 도구

- python
  - Flask
- ngrok



##  서버 구축하기

### ngrok 실행하기

  우선 개인 PC는 서버용이 아니기 때문에 기본적으로 외부에서의 요청이 막혀있습니다. 외부 요청에 응답하는 서버를 구축하기 위해서는 요청을 대신 받아 오는 역할이 필요하는데 , 이를 수행해 주는 것이 ngrok입니다. 

  ngrok을 다운받아 실행 시킨 후, 다음의 코드를 작성해 줍니다.

``` 
ngrok http 5000
```
  이 코드를 수행해주면 포트 번호 5000번을 통하여 외부의 요청을 받아들일 수 있습니다.

## python

#### 서버 구축 

 서버를 구축하기 위해 Flask 모듈을 사용하였습니다. Flask 모듈을 다운 받은 후 import 해줍니다.

```python
from flask import Flask , request
```

 이후 Flask를 통해 서버를 실행시켜 주면 (아직 아무 기능 없는) 서버가 완성됩니다.

``` python
app = Flask(__name__)
if __name__ == "__main__":
    app.run(debug=True)
```

#### 요청/응답 기능 추가

 `@app.route('<경로>') ` 이란 주석을 통해서 URL과 함수를 mapping 할 수 있습니다. (여기서 app은 Flask의 인스탄스)

기본적으로 `GET` 요청이 기본값이며, `methods` 파라미터를 통해서 설정할 수 있습니다. 

GET과 POST 요청에 대한 코드를 예시로 살펴보겠습니다.

``` python
@app.route('/send')
def send():
    // ~~~
    return "sended"
```

``` python
@app.route("/receive", methods=['POST'])
def receive():
    // ~~
    return "received"
```

