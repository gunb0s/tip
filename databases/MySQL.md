
## binlog
### Amazon Aurora SQL
`aurora mysql` 의 `binlog retention hours` 를 볼 수 있음
```
call mysql.rds_show_configuration;
```

## procedure
procedure를 통해 test data를 batch로 `insert`할 수 있다.
```mysql
create table person (
	id VARCHAR(36) DEFAULT (UUID()) PRIMARY KEY, 
	email VARCHAR(50),
	p_no int unsigned
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

DELIMITER $$ 
DROP PROCEDURE IF EXISTS BASIC_INSERT $$
CREATE PROCEDURE BASIC_INSERT()
BEGIN
 DECLARE i INT DEFAULT 0;
 DECLARE lo INT DEFAULT 100000;
 WHILE( i < lo) DO
  INSERT INTO person(email, p_no) 
  VALUES(
   CONCAT(i, "@email.com"), 
   i
  );
  SET i = i+1;
 END WHILE;
END $$
DELIMITER ;

CALL BASIC_INSERT;
```

## Cannot drop index \[key\]: needed in a foreign key constraint

복합 인덱스를 걸었을 때 인덱스가 걸린 칼럼 중 하나라도 다른 외래키가 걸려있으면 복합 키를 삭제할 수 없다.