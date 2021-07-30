# Python API 요청 및 정보 가공

​	URL에 API 요청 중 가장 기본적인 GET 요청을 보내고, 받아온 정보를 가공해 봅니다. API는 영화 정보를 얻을 수 있는 TMDB API을 이용합니다.

​	이번 프로젝트에서는 객체 지향 프로그래밍으로 파이썬 객체를 생성하여, 요청을 보내고 정보를 받아오는 작업을 수행합니다. `TMDBHelper` 라는 클래스를 생성해 앞으로 필요한 기능들을 추가해 줍니다.

```python
class TMDBHelper:
```



## step 1. API key신청

​	해당 프로젝트를 진행하기 위해서는, API에 요청을 보낼 수 있는 권한이 필요합니다. 권한을 신청하기 위해, TMDB 로그인 -> 프로필 -> 설정 -> API 에서 API를 신청하고 인증key값을 받습니다.

​	API 값은 앞으로 모든 API 요청에 활용됨으로,  객체의 속성 값으로 저장해 둡니다. 객체가 초기화 될 때, API 키 값을 입력 받음으로 추후 다른 메소드를 호출할때 언제든지 api key 값을 사용할 수 있습니다. 

```python
    def __init__(self, api_key=None):
        self.api_key = api_key
```



## step 2. API URL 선택

​	URL 주소에 따라서 매핑되어 있는 메소드들이 다르며 이에 따라 반환해 주는 데이터가 다 다릅니다. 자신이 필요한 데이터가 무엇인지 정한 다음, 해당 데이터를 반환하는 URL을 찾아야 합니다. 이에 대한 설명은 https://developers.themoviedb.org/3 에 들어가 확인해줍니다.

​	여기에서는 필요한 url을 생성하는 틀을 메소드를 통해 구현해 둡니다. 

```python
    def get_request_url(self, method='/movie/popular', **kwargs):
        base_url = 'https://api.themoviedb.org/3'
        request_url = base_url + method
        request_url += f'?api_key={self.api_key}'
        for k, v in kwargs.items():
            request_url += f'&{k}={v}'

        return request_url
```

​	base_url은 모든 TMDB API url에 공통적으로 들어가는 앞부분을 미리 저장해둡니다. 그 후, 입력받은 method 값과 자신의 api_key 값을 추가하고, 마지막으로 해당 요청에 필요한 parameter 값들을 추가해 줍니다. 이렇게 해서 완성된 url 을 반환합니다.



## step3. Data 받아오기 및 가공

​	해당 url을 request.get() 메소드에 넣어주면 해당 url에서 제공하는 정보를 받아올 수 있습니다. 이를 가공하고 쉬운 json() 형태로 바꾸어 추후 가공 및 편집하기 쉽게 합니다.

```python
    def get_data(self, request_url):
        # url에 get request를 보내고 받은 결과를 json 형태로 변환 후 반환합니다. 
        data = requests.get(request_url).json()
        return data
```

​	이제 해당 정보를 필요에 따라 가공하여 사용하면 됩니다. 여기서에서는 총 4가지 기능을 구현했습니다.

### 유명 영화 리스트 반환

```python
	def get_popular_movive_list(self):
        """
        popular movie 정보를 json 형태로 반환합니다. 
        """
        request_url = self.get_request_url(language="ko" , region = "KR")
        return self.get_data(request_url).get('results')
```

​	get_request_url의 기본 메소드 값으로 설정되어 있는 '/movie/popular' url'은 유명 영화 정보들을 반환합니다. 이 url에서 받아온 json 데이터 중 `'results' `키 값에는 영화들의 정보가 담긴 dictionary들의 list가 담겨있습니다.

​	해당 정보를 이용한다면, 유명 영화들의 수나 유명 영화들 중 평점이 일정 수준 이상인 영화들만 골라낼 수 있습니다.

```python
# tmdbHelper는 TMDBHelper 클래스의 객체 입니다. api key 보안 문제로 생성 과정은 생략됩니다.
# 유명 영화 수
popular_movies = tmdbHelper.get_popular_movive_list()    
len(popular_movies)

# 평점이 일정 수준 이상인 유명 영화 목록
for movie in popular_movies:
    if movie['vote_average'] >= 8:
        good_movies.append(movie)
good_movies

# 평점이 높은 상위 5개 영화 목록 
[ top5 for top5 in sorted(popular_movies, key=lambda movie:movie['vote_average'], reverse=True)[:5]]
```



## 영화 id 값 추출

```python
    def get_movie_id(self, title):
        request_url = self.get_request_url('/search/movie', query=title, region='KR', language='ko')
        data = requests.get(request_url).json()
        results = data.get('results')
        if results:
            movie = results[0]
            movie_id = movie['id']
            return movie_id
        else:
            raise Exception("해당 영화가 존재하지 않습니다.")
```

​	이번에는 get_request_url의 메소드 값을  '/serach/movie' url'로 설정합니다. 해당 url을 필수적으로 `제목` 파라미터를 필요로 함으로 값을 받아 넣어줍니다. 해당 제목의 영화 목록을 받아 그 중 연관성이 제일 높은 첫번째 영화의 id 값을 반환합니다. 만약 해당 제목의 영화가 존재하지 않는 다면 `Exception`을 발생시킵니다. 



## 영화 추천

```python
    def get_movie_recommend(self, title):
        movie_id = self.get_movie_id(title)
        request_url = self.get_request_url(f'/movie/{movie_id}/recommendations', language='ko' )
        return self.get_data(request_url).get('results')
```

​	추천 영화를 반환하는 url은 영화 id 정보가 필요하기 때문에 방금 생성한 get_movie_id 값을 생성하여 찾아줍니다. id 값을 활용하여 해당 영화와 관련된 추천 영화들을 반환받습니다.

​	이를 활용한다면, 특정 영화를 입력했을 때 관련된 영화들의 제목만을 추출할 수도 있습니다.

```python
# 해당 함수는 클래스 외부의 함수
try:
	movies = tmdbHelper.get_movie_recommend(title)
except:
	None
else:
    [movie['title'] for movie in movies ]
```

@ 만약 해당 제목의 영화가 존재하지 않는다면, get_movie_id()메소드가 Exception을 발생함으로 이를 처리합니다.

### 영화의 배우와 제작진 정보 추출

```python
    def get_cast_crew_info(self, title):
        movie_id = self.get_movie_id(title)
        request_url = self.get_request_url(f'/movie/{movie_id}/credits', language='ko' )
        return self.get_data(request_url)
```

​	마지막 기능은 영화 제목을 입력받아 해당 영화에 출연한 배우와 제작한 제작진들 정보를 받아옵니다. 해당 정보를 주는 url이 영화 id 값을 필요로 함으로 get_movie_id() 메소드를 통해 id를 받아오고, get_request_url() 를 통해 해당 url을 생성합니다. 마지막으로 생성된 url을 get_data() 메소드에 넣어 원하는 데이터를 받아옵니다. 

​	이를 활용하여, 영화 제목을 입력하여 주연급 배우와 감독들을 반환할 수 있습니다.

```python
try:
	movies = tmdbHelper.get_cast_crew_info(title)
except:
	return None
else:
    dic = {"cast" : [] , "crew" :[] }
    for actor in movies['cast']:
    	if actor["cast_id"] <10:
    		dic["cast"].append(actor["name"])
    for crew in movies["crew"]:
    	if crew["department"] == "Directing":
    		dic["crew"].append(crew["name"])
	dic
```

