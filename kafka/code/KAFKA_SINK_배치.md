# KAFKA SINK 배치 모드



### application.yml

```yml
spring.cloud.stream:
	kafka:
		binder:
			brokers: {broker ip}
			autoCreateTopics: false
		bindings:
			myObj-sink:
				consumer:
					startOffset: latest
					autoCommitOffset: false
					configuration:
						max.poll.records: 1000
						fetch.min.bytes : 1
						fetch.max.wait.ms : 1000
    default:
    	consumer:
    		useNativeDecoding: false
    bindings:
    	myObj-sink:
    		destination: {target table}
    		group : {topic}
    		consumer:
    			batch-mode : true
```







### SinkChannel

```java
public interface MyObjSinkChannel {
    String INPUT = "myObj-sink";
    String TOPIC = "my-topic";
    
    @Input(INPUT)
    SubcribableChannel input();
}
```





### Dao

```java

@Repository
@Mapper
public interface MyObjDao {
    
    public int inserIFDI(MyObjVO myObjVO);
    
    public int updateIFDI(MyObjVO myObjVO);
    
    public int deleteIFDI(MyObjVO myObjVO);
}
```





### VO

```java

@Data
@Alias("myObjVO")
public class MyObjVO{
    
    private String field1;
    
    private String field2;
    
    private String field3;
    
    // 순번
    private Long kafkaSno;
    
    public MyObjVO(Map<String, Object> payloadMap) throws KafkaConnectMessageException {
        super();
        if( isEmpty(payloadMap)){
            throw new KafkaConnectMessageException("Empty payload!");
        }
        
        this.field1 = (getString(payloadMap, "FIELD1", ""));
        this.field2 = (getString(payloadMap, "FIELD2", ""));
        this.field3 = (getString(payloadMap, "FIELD2", ""));
        
        this.kafkaSno = (getLong(payloadMap, "SNO", 1));
    }
}
```



### Service

```java
@Service
@Slf4j
@Transactional
public class MyObjService {
    
    @Autowired
    private MyObjDao myObjDao;
    
    @Autowired
    private SinkExceptionProducer sinkExceptionProducer;
    @Autowired
    private SqlSessionFactory sqlSessionFactory;
    @Value("${performance-test-column-enabled}")
    private String performanceYn;
    
    public void saveIFDIEach(List<byte[]> messageList, MessageHeaders headers, long startTime) throws Exception {
        
        String payload = StringUtils.EMPTY;
        Map<String, Object> payloadMap = new HashMap<>();
        
        List kafkaTimeList = (List) headers.get(KafkaHeaders.RECEIVED_TIMESTAMP);
        List partitionIdList = (List) headers.get(KafkaHeaders.RECEIVED_PARTITION_ID);
        
        for(int idx = 0; idx < messageList.size(); idx++){
            
            String kafka_time = String.valueOf(kafkaTimeList.get(idx));
            
            try {
                payload = new String(messageList.get(idx));
                payloadMap = KafkaConnectMessageUtils.getPayloadMap(payload);
                
                Map<String, Object> processParam = new HashMap<String, Object>();
                
                for(String key : payloadMap.keySet()){
                    processParam.put(key, payloadMap.get(key));
                }
                
                Command command = Command.valueOf((String) payloadMap.get("CMD_CD"));
                
                MyObjVO vo = new MyObjVO(processParam);
                
                switch (command) {
                    case INS:
                        myObjDao.insertIFDI(vo);
                        break;
                    case UPD:
                        myObjDao.updateIFDI(vo);
                        break;
                    case DEL:
                        myObjDao.deleteIFDI(vo);
                        break;
                }
                List<BatchResult> results = SqlSessionUtils.getSqlSession(sqlSessionFactory).flushStatements();
                int count = results.get(0).getUpdateCounts()[0];
                if(count == 0 || count > 1){
                    if(log.isDebugEnabled()){
                        log.debug("Execution Update Count:" + count);
                    }
                    sinkExceptionProducer.logEach(idx, messageList, headers, "CMD_CD", count, null);
                }
            }
            catch (Exception e) {
                String skipMessage;
                if((skipMessage= SinkSkipHandler.isSkip(e)) != null) {
                    log.error(skipMessage);
                    inkExceptionProducer.logEach(idx, messageList, headers, "CMD_CD", -1, null);
                } else {
                    inkExceptionProducer.logEach(idx, messageList, headers, "CMD_CD", 0, null);
                    throw e;
                }
            }
        }
    }
}
```

























