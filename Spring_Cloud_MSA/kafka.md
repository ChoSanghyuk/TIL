



zookeeper : broker들을 중재하는 controller

broker : 메시지를 주고 받을 때 저장되는 공간



kafka connect : 하나의 db에서 변경 사항이 있을 때 이를 다른 db / 서비스에 전달



kafka partition



### kafka 서버 기동

- Zookeeper 및 Kafka 서버 구동

.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties

.\bin\windows\kafka-server-start.bat .\config\server.properties

- Topic 생성

.\bin\windows\kafka-topics.bat --create --topic test-event --bootstrap-server localhost:9092 --partitions 1

- Topic 목록 확인

.\bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --list

- Topic 정보 확인

.\bin\windows\kafka-topics.bat  --describe --topic quickstart-events --bootstrap-server localhost:9092



- 메시지 생산

.\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test-event

- 메시지 소비

.\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test-event --from-beginning





























보상 트랜잭션

