## 도커 컴포즈 백그라운드 실행법
```txt
docker-compose up -d;
```

## DB dummy data
```sql
use acoldbottledb;

create table person(
	id int primary key,
    name varchar(100)
    );
    
insert into person(id, name) values(1, "kim");

select * from person;
```