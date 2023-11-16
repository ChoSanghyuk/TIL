# Fetch Type



### Fetch Type

- Eager : will retrieve everything
- Lazy : will retrieve on request 
  - Load dependent entities on demand



### Default Fetch Types

- @OneToOne
  - FetchType.EAGER
- @OneToMany
  - FetchType.LAZY
- @ManyToOne
  - FetchType.EAGER
- @ManyToMany
  - FetchType.LAZY



### Lazy Loading 주의점

- To retrieve lazy data, you will need to open a Hibernate session
- Retrieve lazy data using
  1. session.get and call appropriate getter methods (즉, session이 열려있을 때 작업)
  2. Hibernate query with HQL



### 지정 방법

```java
@OneToMany( fetch= FetchType.LAZY,
           mappedBy = "instructor" , 
           cascade= {CascadeType.PERSIST, CascadeType.MERGE, CascadeType.DETACH , CascadeType.REFRESH} )
```

