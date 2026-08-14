`mysql.ibd`라는 이름의 테이블스페이스에 저장되는 데이터베이스. MySQL 서버 관리를 위한 각종 시스템 테이블과 데이터 딕셔너리 정보를 관리한다.

실제 mysql DB에서 테이블을 살펴보면 실제 테이블의 구조가 저장된 테이블이 보이지 않는데, 이는 사용자가 임의로 테이블을 수정하지 못하도록 화면에 보여주지만 않고, 실제로 존재한다. 대신 데이터 딕셔너리 정보를 information_schema DB의 TABLES와 COLUMS 등과 같은 뷰를 통해 조회할 수 있다.

```sql
SHOW CREATE TABLE INFORMATION_SCHEMA.TABLES;
# 테이블 메타데이터를 담고 있는 테이블을 조회하면 권한없음 오류가 발생한다.
SELECT * FROM mysql.tables LIMIT 1;
```

