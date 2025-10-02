# Домашнее задание к занятию "`Работа с данными (DDL/DML)`" - `Кудряшов Андрей`

---

### Задание 1

```
docker run --name mysql-sys -e MYSQL_ROOT_PASSWORD=rootpass123 -p 3306:3306 -d mysql:8.0
```
```
CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'temppass123';
```
```
SELECT User, Host FROM mysql.user;
```
```
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%';
FLUSH PRIVILEGES;
```
```
SHOW GRANTS FOR 'sys_temp'@'%';
```
```
ALTER USER 'sys_temp'@'%' IDENTIFIED WITH mysql_native_password BY 'temppass123';
```
```
CREATE DATABASE IF NOT EXISTS sakila;
USE sakila;
SOURCE /tmp/sakila-schema.sql;
SOURCE /tmp/sakila-data.sql;
```
```
USE sakila;
SHOW TABLES;
```
```
docker cp sakila-db/sakila-schema.sql mysql-sys:/tmp/
docker cp sakila-db/sakila-data.sql mysql-sys:/tmp/
docker exec -it mysql-sys mysql -usys_temp -ptemppass123
```
```
docker exec -it mysql-sys mysql -usys_temp -ptemppass123 -e "SELECT COUNT(*) FROM sakila.actor;"
```

<img width="1017" height="298" alt="1" src="https://github.com/user-attachments/assets/11c0769c-0f70-4ee7-ba96-a18907a2b5c0" />

<img width="1285" height="658" alt="2" src="https://github.com/user-attachments/assets/0fc3707b-326b-44ec-b4d4-5e6735dbbced" />

<img width="1279" height="636" alt="3" src="https://github.com/user-attachments/assets/79f9a975-e395-4fdc-9c71-962d9408e3d4" />



---

### Задание 2

```
docker exec -it mysql-sys mysql -usys_temp -ptemppass123 -e "
SELECT 
    TABLE_NAME AS 'Название таблицы',
    COLUMN_NAME AS 'Название первичного ключа'
FROM information_schema.KEY_COLUMN_USAGE
WHERE 
    CONSTRAINT_SCHEMA = 'sakila'
    AND CONSTRAINT_NAME = 'PRIMARY'
ORDER BY TABLE_NAME;
"
```

<img width="969" height="564" alt="4" src="https://github.com/user-attachments/assets/4f6b8ee2-9a86-4d09-87e6-3834e8feed2a" />


---

### Задание 3

```
docker exec -it mysql-sys mysql -uroot -prootpass123
```
```
REVOKE ALL PRIVILEGES ON sakila.* FROM 'sys_temp'@'%';
GRANT SELECT, SHOW VIEW ON sakila.* TO 'sys_temp'@'%';
FLUSH PRIVILEGES;
```
```
SHOW GRANTS FOR 'sys_temp'@'%';
```
```
docker exec -it mysql-sys mysql -usys_temp -ptemppass123 -e "USE sakila; INSERT INTO actor (first_name, last_name) VALUES ('Test', 'User');"
```
```
docker exec -it mysql-sys mysql -uroot -prootpass123 -e "SHOW GRANTS FOR 'sys_temp'@'%';"
```

<img width="1276" height="671" alt="5" src="https://github.com/user-attachments/assets/041e3cfe-02e5-4400-be71-197b2add68ae" />


---
