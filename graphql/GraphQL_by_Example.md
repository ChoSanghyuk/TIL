# GraphQL By Example



## Introduction



### 소개

- overfetching 해결
  - get exactly what you need, nothing more and nothing less
  - 기존 API로는 fetching too much data 문제 발생
- underfetching 방지
  - While typical REST APIs require loading from multiple URLs, GraphQL APIs get all the data your app needs in a single request.  

- schema-first approach
  - GraphQL APIs are organized in terms of types and fields, not endpoints.
  - GraphQL uses types to ensure Apps only ask for what’s possible and provide clear and helpful errors
  - clear contract between server and client
- powerful developer tools
- versionless API
  - Add new fields and types to your GraphQL API without impacting existing queries
  - modify API without breaking backward compatibility
  - Aging fields can be deprecated and hidden from tools



## Fundamentals



### Schema Definition

- type definition

  - represents the interface for out API 
  - require what the client request

- resolver function 

  - resolves the value of the greeting field

  - the implementation which provides the code that knows how to return the actual values

    



### Apollo Server

expose API over Http

Apllolo Server

```
npm install @apollo/server graphql
```



반환 종류 data, errors

Operation 영역에서 query 생략 후 `{`로 시작 시, 자동 query로 인식















## Mutations









































