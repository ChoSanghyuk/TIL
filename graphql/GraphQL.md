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

  - The `__typename` field resolves to a `String` which lets you differentiate different data types from each other on the client.
  
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
  
    



## Schemas and Types



### Type system

- 스키마 필요 이유
  - useful to have an exact description of the data we can ask for 
    - what fields can we select? 
    - What kinds of objects might they return? 
    - What fields are available on those sub-objects?

- defines a set of types which completely describe the set of possible data you can query on that service



### Object types

- 개요

  - The most basic components of a GraphQL schema
  - represent a kind of object you can fetch from your service, and what fields it has

- 예시

  ```
  type Character {
    name: String!
    appearsIn: [Episode!]!
  }
  ```

  - `Character` is a *GraphQL Object Type*, meaning it's a type with some fields
  - `name` and `appearsIn` are *fields* on the `Character` type
    - can appear in any part of a GraphQL query that operates on the `Character` type.
  - `String` is one of the built-in *scalar* types
    - a single scalar object, and can't have sub-selections in the query
    - `String!` means that the field is *non-nullable*,
  - `[Episode!]!` represents an *array* of `Episode` objects



### Arguments

- 개요

  - Every field on a GraphQL object type can have zero or more arguments
  - All arguments are named
    - passed by name specifically
  - Arguments can be either required or optional
    - When an argument is optional, we can define a *default value*

- 예시

  ```
  type Starship {
    id: ID!
    name: String!
    length(unit: LengthUnit = METER): Float
  }
  ```

  - the `length` field has one defined argument, `unit`
  - if the `unit` argument is not passed, it will be set to `METER` by default.



### The Query and Mutation types

- 개요

  - they are special because they define the *entry point* of every GraphQL query
  - Every GraphQL service has a `query` type and may or may not have a `mutation` type
  - It's important to remember that other than the special status of being the "entry point" into the schema, the `Query` and `Mutation` types are the same as any other GraphQL object type, and their fields **work exactly the same way.**

- 예시

  ```
  query {
    hero {
      name
    }
    droid(id: "2000") {
      name
    }
  }
  ```

  - this means that the GraphQL service needs to have a `Query` type with `hero` and `droid` fields:

  ```
  type Query {
    hero(episode: Episode): Character
    droid(id: ID!): Droid
  }
  ```

:bulb: Mutations work in a similar way

- you define fields on the `Mutation` type, and those are available as the root mutation fields you can call in your query



### Scalar types

- 개요

  -  represent the leaves of the query
    - object type has a name and fields, but at some point those fields have to resolve to some concrete data

- 종류

  - `Int`: A signed 32‐bit integer.
  - `Float`: A signed double-precision floating-point value.
  - `String`: A UTF‐8 character sequence.
  - `Boolean`: `true` or `false`.
  - `ID`: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an `ID` signifies that it is not intended to be human‐readable

- custom scalar

  - specify custom scalar types

    ```
    scalar Date
    ```

    - we could define a `Date` type
    - it's up to our implementation to define how that type should be serialized, deserialized, and validated.



### Enumeration types

- 개요
  - special kind of scalar that is restricted to a particular set of allowed values
  - allows you to
    - Validate that any arguments of this type are one of the allowed values
    - Communicate through the type system that a field will always be one of a finite set of values

- Definition

  ```
  enum Episode {
    NEWHOPE
    EMPIRE
    JEDI
  }
  ```

  - wherever we use the type `Episode` in our schema, we expect it to be exactly one of `NEWHOPE`, `EMPIRE`, or `JEDI`.



### Lists and Non-Null

- 개요
  - Object types, scalars, and enums are the only kinds of types you can define
  - you can apply additional ***type modifiers*** that affect validation of those values
    - `!`
    - `[]`

- 예시

  ```json
  myField: [String!]
  ----------------------
  myField: null // valid
  myField: [] // valid
  myField: ["a", "b"] // valid
  myField: ["a", null, "b"] // error
  ```

  

### Interfaces

- 개요

  - an abstract type that includes a certain set of fields that a type must include to implement the interface
  - useful when you want to return an object or set of objects, but those might be of several different types

- 예시

  ```
  interface Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
  }
  ```

  - you could have an interface `Character` that represents any character in the Star Wars trilogy
  - any type that *implements* `Character` needs to have these exact fields, with these arguments and return types.

  ```
  type Human implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    starships: [Starship]
    totalCredits: Int
  }
  
  type Droid implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    primaryFunction: String
  }
  ```

  - both of these types have all of the fields from the `Character` interface, but also bring in extra fields



### Union types

- 개요
  - share similarities with interfaces; however, they lack the ability to define any shared fields among the constituent types
  - members of a union type need to be concrete object types;

- 예시

  ```
  union SearchResult = Human | Droid | Starship
  ```

  - Wherever we return a `SearchResult` type in our schema, we might get a `Human`, a `Droid`, or a `Starship`.



### Input types

- 개요
  - you can also easily pass complex objects
  - This is particularly valuable in the case of mutations, where you might want to pass in a whole object to be created.
  - input types look exactly the same as regular object types, but with the keyword `input` instead of `type`
  -  Input object types also can't have arguments on their fields

- 예시

  ```
  input ReviewInput {
    stars: Int!
    commentary: String
  }
  ```

  ```
  mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
    createReview(episode: $ep, review: $review) {
      stars
      commentary
    }
  }
  ------------------------------------------------------------------------VARAIBLES
  {
    "ep": "JEDI",
    "review": {
      "stars": 5,
      "commentary": "This is a great movie!"
    }
  }
  ```

  





















