#  URL Shortener



## Basic System Design & Algorithm

- Problem
  - Generate a short and unique url for given original url
- Solution
  - generate a unique hash of given url using one of the hash algorithm's present out there such as MD-5 or SHA-256
- Possible issues
  - Multiple user can request to shorten the same original url => generate same hash for the url
  - we can append a increasing number to the url before generating its hash => generated hash will always unique
  - Increasing number can grow large. Need a better alternative
  - 고려 요소 : Data Replicaton, Caching, Load Balancing, Capacity Estimations





### dependencies

- jpa : https://gmlwjd9405.github.io/2019/08/04/what-is-jpa.html

- commons-lang3 : https://recordsoflife.tistory.com/474

  - Java API의 기능 확장을 목표로하는 인기 있고 모든 기능을 갖춘 유틸리티 클래스 패키지입니다 .

    라이브러리의 레퍼토리는 문자열, 배열 및 숫자 조작, 반영 및 동시성에서 쌍 및 트리플 (일반적으로 [튜플](https://en.wikipedia.org/wiki/Tuple) 이라고 함)과 같은 여러 순서가 지정된 데이터 구조의 구현에 이르기까지 매우 풍부

- starter-test : https://blog.neonkid.xyz/218
  - junit : 단위 테스트 도구