# GraphQL





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



## Queries and Mutations



### Fields

- GraphQL is about asking for specific fields on objects

  ```json
  {
    hero {
      name
    }
  }
  --------------------
  {
    "data": {
      "hero": {
        "name": "R2-D2"
      }
    }
  }
  ```

  - the query has exactly the same shape as the result
    - you always get back what you expect
    - the server knows exactly what fields the client is asking for

- fields can also refer to Objects. In that case, you can make a *sub-selection* of fields for that object

  ```json
  {
    hero {
      name
      # Queries can have comments!
      friends {
        name
      }
    }
  }
  ------------------------
  {
    "data": {
      "hero": {
        "name": "R2-D2",
        "friends": [
          {
            "name": "Luke Skywalker"
          },
          {
            "name": "Han Solo"
          },
          {
            "name": "Leia Organa"
          }
        ]
      }
    }
  }
  ```

  - GraphQL queries look the same for both single items or lists of items



### Arguments

- In GraphQL, every field and nested object can get its own set of arguments

  - In REST, you can only pass a single set of arguments - the query parameters and URL segments in your request

  ```json
  {
    human(id: "1000") {
      name
      height(unit: FOOT)
    }
  }
  ------------------------
  {
    "data": {
      "human": {
        "name": "Luke Skywalker",
        "height": 5.6430448
      }
    }
  }
  ```

  - Arguments can be of many different types
    - ex) Enumeration type
    - GraphQL comes with a default set of types, but a GraphQL server can also declare its own custom types, as long as they can be serialized into your transport format.



### Aliases

- you can't directly query for the same field with different arguments.

  <= the result object fields match the name of the field in the query but don't include arguments

  ```json
  {
    empireHero: hero(episode: EMPIRE) {
      name
    }
    jediHero: hero(episode: JEDI) {
      name
    }
  }
  ----------------------------------------
  {
    "data": {
      "empireHero": {
        "name": "Luke Skywalker"
      },
      "jediHero": {
        "name": "R2-D2"
      }
    }
  }
  ```



### Fragments

- 개념

  - reusable units
  - let you construct sets of fields, and then include them in queries where you need to

  ```json
  {
    leftComparison: hero(episode: EMPIRE) {
      ...comparisonFields
    }
    rightComparison: hero(episode: JEDI) {
      ...comparisonFields
    }
  }
  
  fragment comparisonFields on Character {
    name
    appearsIn
    friends {
      name
    }
  }
  ----------------------------------------
  ...
  ```

  - concept of fragments is frequently used to split complicated application data requirements into smaller chunks

- 응용 : Using variables inside fragments

  - It is possible for fragments to access variables declared in the query or mutation

  ```json
  query HeroComparison($first: Int = 3) {
    leftComparison: hero(episode: EMPIRE) {
      ...comparisonFields
    }
    rightComparison: hero(episode: JEDI) {
      ...comparisonFields
    }
  }
  
  fragment comparisonFields on Character {
    name
    friendsConnection(first: $first) {
      totalCount
      edges {
        node {
          name
        }
      }
    }
  }
  --------------------------------------------
  ...
  ```

  

### Operation name

- 개념

  - operation type
    - describes what type of operation you're intending to do
    - ex) *query*, *mutation*, or *subscription*
    - The operation type is required unless you're using the query shorthand syntax
  - operation name
    - a meaningful and explicit name for your operation
    - only required in multi-operation documents, but its use is encouraged because it is very helpful for debugging and server-side logging

- 예시

  ```
  query HeroNameAndFriends {
    hero {
      name
      friends {
        name
      }
    }
  }
  -----------------------
  ..
  ```

  - `query` as *operation type* and `HeroNameAndFriends` as *operation name* :



### Variables

- 개념

  - GraphQL has a first-class way to factor dynamic values out of the query, and pass them as a separate dictionary
    - the arguments to fields will be dynamic
  -  in our client code, we can simply pass a different variable rather than needing to construct an entirely new query
  - This is a good practice for denoting which arguments in our query are expected to be dynamic

- 사용법

  1. Replace the static value in the query with `$variableName`
  2. Declare `$variableName` as one of the variables accepted by the query
  3. Pass `variableName: value` in the separate, transport-specific (usually JSON) variables dictionary

- 예시

  ```json
  query HeroNameAndFriends($episode: Episode) {
    hero(episode: $episode) {
      name
      friends {
        name
      }
    }
  }
  --------------------------- VARIABLES
  {
    "episode": "JEDI"
  }
  ```

- Variable definitions
  - the part that looks like `($episode: Episode)`
  - works just like the argument definitions for a function in a typed language.
    - lists all of the variables, prefixed by `$`, followed by their type
  - All declared variables must be either scalars, enums, or input object types
  - Variable definitions can be optional or required
    -  if there isn't an `!` next to the type, it's opitonal
    - if the field you are passing the variable into requires a non-null argument, then the variable has to be required as well

- Default variables

  - When default values are provided for all variables, you can call the query without passing any variables

  - If any variables are passed as part of the variables dictionary, they will override the defaults

  - 예시

    ```json
    query HeroNameAndFriends($episode: Episode = JEDI) {
      hero(episode: $episode) {
        name
        friends {
          name
        }
      }
    }
    ```



### Directives

-  개념

  - a way to dynamically change the structure and shape of our queries using variables
  - can be attached to a field or fragment inclusion, and can affect execution of the query in any way the server desires
  - core GraphQL specification includes exactly two directives
    - `@include(if: Boolean)`
      - Only include this field in the result if the argument is `true`
    - `@skip(if: Boolean)`
      - Skip this field if the argument is `true`

  - can be useful to get out of situations where you otherwise would need to do string manipulation to add and remove fields in your query

- 예시

  ```json
  query Hero($episode: Episode, $withFriends: Boolean!) {
    hero(episode: $episode) {
      name
      friends @include(if: $withFriends) {
        name
      }
    }
  }
  --------------------------------------------------------VARIABLES
  {
    "episode": "JEDI",
    "withFriends": false
  }
  --------------------------------------------------------
  {
    "data": {
      "hero": {
        "name": "R2-D2"
      }
    }
  }
  ```



### Mutations

- 개념

  - needs a way to modify server-side data as well
  - useful to establish a convention that any operations that cause writes should be sent explicitly via a mutation.
  - useful for fetching the new state of an object after an update

- 예시

  ```JSON
  mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
    createReview(episode: $ep, review: $review) {
      stars
      commentary
    }
  }
  -------------------------------------------------------------------------VARAIBLES
  {
    "ep": "JEDI",
    "review": {
      "stars": 5,
      "commentary": "This is a great movie!"
    }
  }
  -------------------------------------------------------------------------
  {
    "data": {
      "createReview": {
        "stars": 5,
        "commentary": "This is a great movie!"
      }
    }
  }
  ```

- Multiple fields in mutations

  - one important distinction between queries and mutations

    - **While query fields are executed in parallel, mutation fields run in series, one after the other.**
    - if we send two `incrementCredits` mutations in one request, the first is guaranteed to finish before the second begins, **ensuring that we don't end up with a race condition**

    

### Inline Fragments

- 개요

  - If you are querying a field that returns an interface or a union type, you will need to use *inline fragments* to access data on the underlying concrete type

- 예시

  ```json
  query HeroForEpisode($ep: Episode!) {
    hero(episode: $ep) {
      name
      ... on Droid {
        primaryFunction
      }
      ... on Human {
        height
      }
    }
  }
  ---------------------------------------VARIABLES
  {
    "ep": "JEDI"
  }
  ---------------------------------------
  {
    "data": {
      "hero": {
        "name": "R2-D2",
        "primaryFunction": "Astromech"
      }
    }
  }
  ```

  - the `hero` field returns the type `Character`, which might be either a `Human` or a `Droid` depending on the `episode` argument
  - In the direct selection, you can only ask for fields that exist on the `Character` interface, such as `name`.
  - To ask for a field on the concrete type, you need to use an *inline fragment* with a type condition
    - Because the first fragment is labeled as `... on Droid`, the `primaryFunction` field will only be executed if the `Character` returned from `hero` is of the `Droid` type.

- Meta fields

  - In case you don't know what type you'll get back from

  - GraphQL allows you to request `__typename`, a meta field, at any point in a query to get the name of the object type at that point.

    ```json
    {
      search(text: "an") {
        __typename
        ... on Human {
          name
        }
        ... on Droid {
          name
        }
        ... on Starship {
          name
        }
      }
    }
    ------------------------------
    {
      "data": {
        "search": [
          {
            "__typename": "Human",
            "name": "Han Solo"
          },
          {
            "__typename": "Human",
            "name": "Leia Organa"
          },
          {
            "__typename": "Starship",
            "name": "TIE Advanced x1"
          }
        ]
      }
    }
    ```

    



































































