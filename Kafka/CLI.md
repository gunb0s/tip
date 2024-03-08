#### kafka-topics.sh
토픽 설정 조회
```bash
./kafka-topics.sh --describe --topic af-connect-offsets --bootstrap-server host:port
```

#### kafka-configs.sh
토픽 설정 변경
```bash
./kafka-configs.sh --alter --bootstrap-server host:port --entity-type topics --entity-name af-connect-configs --add-config cleanup.policy=compact
```