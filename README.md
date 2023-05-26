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
