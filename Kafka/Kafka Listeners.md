https://www.confluent.io/blog/kafka-listeners-explained/
![[Pasted image 20240125111217.png]]
client를 실행할 때 넘겨주는 broker는 cluster에서 broker에 대한 메타데이터를 가져오는 용도이다. 
데이터를 읽고 쓸 때 연결해야하는 실제 host, IP는 초기 커넥션에서 broker가 넘겨주는 data를 기반으로 한다.

`KAFKA_LISTENERS`: Kafka가 수신 대기하는 host/IP, port 기본 값은 0.0.0.0이다. 복잡한 네트워크 환경에서는 주어진 머신의 네트워크 인터페이스에 관련된 IP 주소일 수 있다.

`KAFKA_ADVERTISED_LISTENERS`: 클라이언트에게 응답하는 metadata

`KAFKA_LISTENER_SECURITY_PROTOCOL_MAP`: listener 이름 마다 사용할 security protocol 의 키, 값쌍들이다.