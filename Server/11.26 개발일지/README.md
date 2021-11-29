# TIL

1. - - ## 11/26 개발일지

       1. mysql community server 8.0.19 tar.gz 다운

          - `wget <https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.19.tar.gz`>
          - `tar xvfz mysql-8.0.19.tar.gz`

       2. cmake 진행

          - ## cmake 를 알아보자

          ```
          /usr/local/mysql-8.0.19# cmake \\
          > -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \\
          > -DMYSQL_DATADIR=/usr/local/mysql/data \\
          > -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \\
          > -DMYSQL_TCP_PORT=3306 \\
          > -DDEFAULT_CHARSET=utf8 \\
          > -DDEFAULT_COLLATION=utf8_general_ci \\
          > -DSYSCONFDIR=/etc \\
          > -DWITH_EXTRA_CHARSETS=all \\
          > -DWITH_INNOBASE_STORAGE_ENGINE=1 \\
          > -DWITH_ARCHIVE_STORAGE_ENGINE=1 \\
          > -DWITH_BLACKHOLE_STORAGE_ENGINE=1 \\
          > -DDOWNLOAD_BOOST=1 \\
          > -DWITH_BOOST=/usr/local/mysql/boost
          ```

          두가지 에러를 마주했다

          - boost 라이브러리를 다운받았지만 버전이 맞지 않아 생긴 문제였다

          - https://boostorg.jfrog.io/artifactory/main/release/1.70.0/source/

            여기서 boost_1_70_0.tar.gz 다운로드 후

            `DWITH_BOOST=/usr/local/mysql/boost_1_70_0`

            로 변경하니 해결!

          - swap 메모리 부족인한 생긴 문제였다

            `swapoff -v /swapfile`

            `sudo fallocate -l 8G /swapfile`

            `sudo mkswap /swapfile`

            `swapon /swapfile`

            후 `make` 진행하니 진행상태에서 정상적으로 진행된다.

          1. 데이터베이스 초기화

             - `cd /usr/local/mysql`
             - `mkdir mysql-files`
             - `chown -R mysql:mysql /usr/local/mysql`
             - `chown mysql:mysql mysql-files`
             - `chmod 750 mysql-files`

             ```
             ./mysqld --initialize --user=mysql \\
             > --basedir=/usr/local/mysql \\
             > --datadir=/usr/local/mysql/data
             ```

          2. 서버 실행

             `./mysqld_safe --user=mysql &`

       3. 서버 연결

       ```
       ./mysql -u root -p
       ```

       비밀번호를 데이터베이스 초기화할때 주는데 보지 못하고 터미널을 재실행했었다..

       mysql를 재설치하여 재발급 받아 해결했다

       1. 비밀번호 재설정

          `alter user 'root'@'localhost' identified with mysql_native_password by '바꿀 패스워드';`

       2. 서버 종료

          `./mysqladmin -u root -p shutdown`