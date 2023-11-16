# One To Many Mapping





## One To Many Bi-directional



### Development Process

1. Define database tables
2. Create class
3. Create Main App



### 1. Define database tables

- Many에 해당되는 table에서 mapping 하는 table의 primary key를 foreign key로 설정



### 2. Create class

- Many에 해당되는 클래스에서 @ManyToOne 지정함

  ```java
  @Entity
  @Table(name="course")
  public class Course {
  
  	@Id
  	@GeneratedValue(strategy = GenerationType.IDENTITY)
  	@Column(name="id")
  	private int id;
  	
  	@Column(name="title")
  	private String title;
  	
  	@ManyToOne(cascade= {CascadeType.PERSIST , CascadeType.MERGE , 
                           CascadeType.DETACH , CascadeType.REFRESH})
  	@JoinColumn(name="instructor_id")
  	private Instructor instructor;
      
      ...
  }
  ```

- One에 해당되는 클래스에서 @OneToMany 지정함 (+ mappedBy)

  ```java
  @Entity
  @Table(name = "instructor")
  public class Instructor {
  
  	@Id
  	@GeneratedValue(strategy = GenerationType.IDENTITY)
  	@Column(name="id")
  	private int id;
  	
  	@Column(name="first_name")
  	private String firstName;
  	
  	@Column(name="last_name")
  	private String lastName;
  	
  	@Column(name="email")
  	private String email;
  	
  	// setting up mapping to InstructorDetail entity
  	@OneToOne(cascade = CascadeType.ALL)
  	@JoinColumn(name = "instructor_detail_id")
  	private InstructorDetail instructorDetail;
  	
  	@OneToMany(mappedBy = "instructor" , cascade= {CascadeType.PERSIST , CascadeType.MERGE,
                                                     CascadeType.DETACH , CascadeType.REFRESH} )
  	private List<Course> courses;
      
      // constructor
      // getters & setters
      
      public void add(Course tempCourse) {
  		if( courses == null) {
  			courses = new ArrayList<>();
  		}
  		
          // Bi-directional link
  		courses.add(tempCourse);
  		tempCourse.setInstructor(this);
  	}
  
  ```

  - One => Many 연결을 해줄 때, Many => One 으로도 같이 연결을 해줌 



## One To Many Uni-Directional



### Development Process

1. Define database tables
2. Create Review class
3. Update Course class
4. Create Main App



### 1. Define databse tables : review

- 자신이 참조하는 course의 primary key를 foreign key로 지정
- 참조되는 course는 지정 필요 x



### 2. Create Review Class

- ! review table에는 foreign key가 있지만 Review class에서는 mapping x

```java
@Entity
@Table(name = "review")
public class Review {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	private String comment;
    
    // constructors
    // getters & setters
}
```



### 3. Update Course Class

- foreign key를 지정하지 않은 course class에서 review에 대한 mapping이 이루어짐
- course => review 의 단방향 접근 설정하기 위함

```java
@Entity
@Table(name="course")
public class Course {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="title")
	private String title;
	
	@ManyToOne(cascade= {CascadeType.PERSIST , CascadeType.MERGE ,
                         CascadeType.DETACH , CascadeType.REFRESH})
	@JoinColumn(name="instructor_id")
	private Instructor instructor;
	
	@OneToMany( fetch = FetchType.LAZY, cascade= CascadeType.ALL)
	@JoinColumn(name="course_id")
	private List<Review> reviews;
    
    // constructors
    // getters & setters
    
    public void addReview(Review theReview) {
		if(reviews == null) {
			reviews = new ArrayList<>();
		}
		
		reviews.add(theReview);
	}
}
```

- JoinColumn의 'course_id'는 review table의 foreign key column을 의미
- @JoinColumn을 통해 spring에서 review table의 course_id를 통해서 연관된 review들을 가져오라 알려줌
- addReview를 통해서는 단방향 연결만

























