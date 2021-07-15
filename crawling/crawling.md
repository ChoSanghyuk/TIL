# Python을 통한 Crawling

## 개요

:green_apple: python을 이용하여 web page의 data를 crawling 하는 법을 살펴봄

- URL 주소로 html 문서 크롤링하기
- API로 Json data 크롤링하기



## URL 주소로 html 문서 크롤링하기

```python
import requests
from bs4 import BeautifulSoup


url = "https://finance.naver.com/marketindex/"

res = requests.get(url)
data = BeautifulSoup(res.text, 'html.parser')
selector ="#exchangeList > li.on > a.head.usd > div > span.value"
kopsi = data.select_one(selector)



print(kopsi.text)
```

  `requests.get(<url>)` 를 통해서 해당 **url의 페이지 정보**를 받아옵니다.

  BeautifulSoup는 텍스트 전처리에 사용되며, 여기에서는 **html 번역기의 역할**을 했습니다.

  selector 변수에는 url에서 원하는 부분의 selector의 값을 넣어줍니다.

  data는 BeautifulSoup의 객체임으로, BeautifulSoup의 `select_one() `메소드를 활용하여 selector 부분의 정보만 가져옵니다.

 

  ## API로 Json data 크롤링하기

  우선 API를 활용하기 위해서는 해당 사이트의 API를 사용할 수 있는지 확인하여야 한다.

  API가 공공으로 열려있는 경우에는 바로 사용할 수 있지만, 인증이 필요한 경우 따로 신청하여 ServiceKey 등을 제공받아야 

사용할 수 있다. 

  API에 접근하는 기본 형식으론 `<API주소>?<파라미터>=<값>&<파라미터>=<값> ... `형식으로 입력하면 된다. 

``` python
import requests


url = "http://apis.data.go.kr/B552584/ArpltnInforInqireSvc/getCtprvnRltmMesureDnsty?serviceKey=<서비스키>&numOfRows=10&pageNo=1&sidoName=%EC%84%9C%EC%9A%B8&ver=1.0&returnType=json"
res = requests.get(url)
data = res.json()

print(data)
```

  html 크롤링과 마찬가지로 get 요청을 통해서 데이터를 받아온다. 이 데이터는 String 형식이기 때문에, json() 메소드를 통해 Sting을 dictionary type으로 변환해 준다.