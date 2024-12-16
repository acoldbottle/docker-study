## MySQL 도커파일 생성 방법

```text
    FROM mysql

    ENV MYSQL_USER=acoldbottle
    ENV MYSQL_PASSWORD=88888888
    ENV MYSQL_ROOT_PASSWORD=root1234
    ENV MYSQL_DATABASE=acoldbottledb
    
    CMD ["--character-set-server=utf8mb4", "--collation-server=utf8mb4_unicode_ci"]
```

## UTF 8 설정 확인
```sh
SHOW VARIABLES LIKE 'character_set_%';
```

## 볼륨 옵션으로 호스트 폴더 연결해서 실행하는 법
```sh
docker run -d -v /Users/byoungchan/Desktop/DOCKER_LAB/ex05/mysql-volume:/var/lib/mysql -p 3307:3306 --name mysql-container mysql-image
```
## 이름이 있는 볼륨 사용하는 법
```sh
docker run -d -v mysql-volume:/var/lib/mysql -p 3307:3306 --name mysql-container mysql-image;
```
