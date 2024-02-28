1. Not Enough Replicas
	https://sungjk.github.io/2023/05/13/kafka-not-enough-replicas.html
	 `acks` 설정이 all이거나 -1이면 Leader로 부터 `ack` 응답을 받는 것에 더해 모든 `Replication`으로의 동기화까지 잘 이루어져야지만 Producer는 성공적인 커밋으로 처리할 수 있다.
	 `ISR(In Sync Replicas)` 은 Leader와 동기화된 상태를 유지하고 있는 복제본의 집합인데 Producer의 `acks` 값에 따라 결정된다. 이 값의 크기, 최소 복제본 수는 `min.insync.replicas` 속성을 통해서 설정할 수 있다. 그러니까 `min.insync.replicas` 값은 2인데 Partition의 `replication factor`는 1이라면 에러가 발생할 수 있다.