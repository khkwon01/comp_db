# comp_db

MySQL 8.0.33과 Maria 10.8.8 버전 성능을 sysbench로 테스트 했을 경우 비교     

## 1. 테스트 환경
1) OS 버전 : centos 7.9.2009 (core)    
참고 : mariadb는 oracle linux 8.0에 설치 오류로 인해 설치가 안됨.
2) my.cnf 구성    
2.1) MySQL 설정
```
[mysql]
socket=/mysql/mysql8/mysql.sock

[mysqld]
datadir=/mysql/mysql8/data
socket=/mysql/mysql8/mysql.sock
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/mysql/mysql8/log/mysql.log
pid-file=/mysql/mysql8/mysql.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```
2.2) Mariadb 설정
```
[mysql]
socket=/mysql/maria/mysql.sock
[mysqld]
datadir=/mysql/maria/data
socket=/mysql/maria/mysql.sock
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/mysql/maria/log/mariadb.log
pid-file=/mysql/maria/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```
3) 성능 테스트툴 설치 (sysbench)   
yum install sysbench  (1.0.17)
4) MySQL 계정 구성     
4.1) MySQL 설정    
create user systest identified by 'Welcome1';    
```# MySQL 8.0 계정과 sysbench 호환을 위해 아래와 같이 8.0 이전 인증 방식으로 설정, cache_sha2를 사용할 경우 오류발생```    
alter user systest identified with mysql_native_password by 'Welcome1';      
create database sysbench;    
grant all privileges on sysbench.* to systest;    
4.2) Mariadb 설정    
create user systest identified by 'Welcome1';    
create database sysbench;    
grant all privileges on sysbench.* to systest;    
     
## 2. 테스트 수행
1) sysbench 명령어
- prepare
```
sysbench --db-driver=mysql --time=50 --threads=100 --report-interval=20 --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=systest --mysql-password="Welcome1" --mysql-db=sysbench --tables=20 --table_size=1000000 oltp_read_write --db-ps-mode=disable prepare
```
- run
```
sysbench --db-driver=mysql --time=50 --threads=100 --report-interval=20 --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=systest --mysql-password="Welcome1" --mysql-db=sysbench --tables=20 --table_size=1000000 oltp_read_write --db-ps-mode=disable run
```
- clean
```
sysbench --db-driver=mysql --time=50 --threads=100 --report-interval=20 --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-user=systest --mysql-password="Welcome1" --mysql-db=sysbench --tables=20 --table_size=1000000 oltp_read_write --db-ps-mode=disable clean
```
2) sysbench test 결과
- MySQL
![image](https://github.com/khkwon01/comp_db/assets/8789421/92687705-5a29-441c-a8f9-4181e0a01fad)
- MariaDB
![image](https://github.com/khkwon01/comp_db/assets/8789421/4a54bea3-c13d-484d-b40e-dca6ba13a899)
