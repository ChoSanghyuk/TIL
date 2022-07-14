# KAFKA 삭제 커넥터 구현



### Sink

```java
@Slf4j
@Component
public class DelObjSink {
    
    @Autowired
    private DelObjService delObjService;
    
    @Scheduled(fixedDelayString = "${spring.scheduler.fixedDelay}")
    private void processLogic(){
        
        long startTime = System.currentTimeMillis();
        
        try {
            delObjService.saveIFDI();
            log.info("myLog");
        } catch (Exception e) {
            log.error("myErrorLog");
        }
    }
}
```



### Service

```java
@Service
@Slf4j
@Transactional
public class DelObjService {
    
    @Autowired
    private DelObjDao delObjDao;
    
    public void saveIFDI() throws Exception {
        delObjDao.deleteSDWIFDI();
    }
}
```



### Dao

```java
import org.apache.ibatis.annotations.Mapper;

@Repository
@Mapper
public interface DelObjDao{
    public void deleteSDWIFDI();
}
```



### sql

```XML
<?xml version="1.0" encoding="UTF-8?"
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
			"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mycode.sink.del.DelObjDao">

	<delete id="deleteSDWIFDI">
		<![CDATA[
		DELETE FROM MY_TABLE
		WHERE 1=1
			AND CONDITION1
		]]>
 </delete>
</mapper>
```

