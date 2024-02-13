## mysql timezone
mysql 8 버전 부터 다음과 같은 에러가 발생하는데
```bash
[2024-02-13 15:27:27,872] ERROR Failed testing connection for jdbc:mysql://localhost:3306/?useInformationSchema=true&nullCatalogMeansCurrent=false&useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&zeroDateTimeBehavior=CONVERT_TO_NULL&connectTimeout=30000 with user 'root' (io.debezium.connector.mysql.MySqlConnector:91)
java.sql.SQLException: The server time zone value 'KST' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the 'connectionTimeZone' configuration property) to use a more specific time zone value if you want to utilize time zone support.
```
다음과 같이 해결할 수 있다.
```bash
mysql --help
```
로 `my.conf` 의 위치가 될 수 있는 곳을 찾은 후 그 중 하나에 `my.conf` 를 생성하고 다음을 추가한다.
```
[mysqld]  
default-time-zone=Asia/Seoul
```
그리고 나서 재시작하면 되는데
mysql 에 timezone 정보가 없을 경우 알 수 없는 timezone 이라며 재시작을 못할 수 있다.
https://dev.mysql.com/downloads/timezones.html 여기서 `timezone_2024a_posix_sql.zip` 를 다운 받은 후 `mysql -u root -p mysql < timezone_posix.sql` 를 실행하면 된다. 다음의 쿼리로 타임존 정보가 제대로 들어갔는지 확인할 수 있다.
```sql
select b.name, a.time_zone_id from mysql.time_zone a, mysql.time_zone_name b where a.time_zone_id = b.time_zone_id and b.name like '%Seoul';
```
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