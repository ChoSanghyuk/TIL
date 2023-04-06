# [Java] hashCode() 메소드는 무슨 역할을 하는가? (feat. vs equals())



Eclipse와 IntelliJ를 사용하다보면 IDE가 제공해주는 overriding 기능을 자주 사용하게 되는데, equals()을 overriding 할 때면 hashCode()도 같이 overriding 해주곤 한다.

equals()는 객체끼리의 비교 작업을 위해서 자주 코드를 변경하여 익히 용도를 알고 있었지만, hashCode()는 건드려본적이 없기 때문에 문득 용도가 궁금해졌다. 

분명 equals()와 비슷한 역할을 할거 같긴 하지만 정확히 hashCode()의 용도는 무엇일까?



## Hash란 무엇인가?

해시는 특정 input을 넣었을 때 일련의 변환 과정을 걸쳐서 나오는 값이다. 이러한 변환 과정은 주로 다음과 같은 특징을 가진다.



1. 단방향 변환 : 결과값으로부터 인풋값을 유추할 수 없다.
2. 일관성 : 같은 인풋을 넣었을 때, 같은 결과가 나온다.
3. Black-Box 모델 : 내부의 변환 로직이 비공개이다.



이러한 해시값은 정보를 보호하기 위한 암호화에 많이 사용되며, 특정 값의 identity를 나타내는데 사용되기도 한다.





## hashCode() 동작



Java의 hashCode() 메소드는 해당 객체를 해싱 알고리즘을 걸쳐 Integer 타입의 해시코드로 변환하는 작업을 수행한다. 여기서, 같은 객체라면 항상 같은 해시 코드가 반환되며, **다른 객체라고 해서 다른 해시 값이 나온다는 것을 보장하지 않는다.**



일반적인 hashCode()의 implementation은 다음과 같다.

```java
@Override
public int hashCode() {
    int hash = 7;
    hash = 31 * hash + (int) field1;
    hash = 31 * hash + (name == null ? 0 : field2.hashCode());
    hash = 31 * hash + (email == null ? 0 : field3.hashCode());
    return hash;
}
```

어떤 클래스가 field1,field2,field3을 필드로 가질 때, 해당 클래스의 객체는 필드들의 값이 변함에 따라서, 다른 해시 코드를 가지게 될 것이다.



반대로, hashCode를 다음과 같이 변환한다면

```java
@Override
public int hashCode() {
    return 1;
}
```

해당 클래스의 객체들은 모두 1이라는 같은 해시 코드를 가지게 된다.



## hashCode() 역할



hashCode()로 반환된 해시값은 HashMap, HashSet, HashTable와 같이 Hash 기반의 Java collections에 사용된다. 



HashMap을 예로 들어보겠다. 만약에 HashMap이 해시값을 사용하지 않는다면, HashMap에 새로운 key-value 쌍이 들어올 때마다 HashMap은 기존에 들어와 있던 key들이 지금 들어온 key와 같은지를 하나씩 대조해가며 알아내야 할 것이다.



하지만, HashMap의 Key-Value 쌍들이 List 형태로 저장되고, 해당 위치의 Index 값으로 Hash를 사용한다면, 여러 번의 대조 작업 없이 단번에 같고 다름을 알 수 있게 된다. 뿐만 아니라 특정 Key의 값을 조회하고, 삭제할 때에도 바로 수행 가능할 것이다.



이렇게 Java에서는 객체의 Identity를 hashCode()로 생성해 내고, 해당 값을 Index로 활용함으로써 효율적인 처리를 가능하게 한다.





## Hash 충돌 (Hash Collision)

위의 코드 예시에서 보았듯이, hashCode()는 객체가 다르다고 항상 다른 값을 도출하지 않는다. 



즉, hashCode()는 해당 객체의 유일한 identity를 보장하지 않기에, HashMap에 저장할 때 서로 다른 Key의 hash 값이 중복될 수도 있다. 이 경우에는, 같은 Hash 값을 가지는 Key-Value 쌍들을 linked list 형태의 bucket에 저장해둔다. 같은 Buckect 안에 있는 객체들 끼리는 equals()를 활용한 1:1 비교 작업을 통해서 같고 다름을 판단하게 된다.



이렇게 서로 다른 객체들이 같은 hash 값을 가지도록 hashCode() 메소드를 작성하게 된다면, 기존에 효율적인 연산을 위해 고안되었던 방식이 쓸모 없게 되버린다. 따라서, Hash 기반의 collection들을 효율적으로 사용하기 위해서는 hashCode()도 그에 맞게 작성되어야 한다. 



## 마무리

정리하자면, Java에서 hashCode()는 hash 기반 클래스들의 효율적인 처리를 위한 코드 생성 수단이며, equals()는 두 객체가 같은 객체인지 비교하기 위한 수단으로 사용된다.



추가로 두 메소드 모두 override() 하지 않는다면, 객체의 memory 주소 기반으로 작동하게 된다. hashCode()는 객체의 address 기반으로 hash 값을 생성하며, equal()는 두 객체가 같은 address를 pointing 하고 있는지를 기반으로 판단한다. 즉, overriding하지 않는 채로는 필드 값들이 모두 같지만, 각각 선언된 두 객체는 다른 해시값과 일치하지 않는다라는 결과를 가진다.





해당 글은 다음의 강의, 자료들을 참조, 번역하여 작성되었습니다.

- Baeldung,Guide to hashCode() in Java ,https://www.baeldung.com/java-hashcode
- SCALER Topics,HashCode() in Java,https://www.scaler.com/topics/hashcode-in-java/
- Investopedia, What Is a Hash? Hash Functions and Cryptocurrency Mining, https://www.investopedia.com/terms/h/hash.asp