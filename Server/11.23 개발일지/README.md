# TIL

## 11.23 개발일지

1. parallels 설치
   - https://www.parallels.com/products/desktop/welcome-trial/
   - https://gaemi606.tistory.com/entry/MacBook-Air-M1에-Ubuntu설치하기-Parallels
2. 우분투 버전 확인) lsb_release -a

1. sudo apt update

   - sudo
     - 관리자 권한으로 실행한다
     - 접근이 제한되어 있는 파일을 수정, 삭제할 때 사용
     - 일반적으로 새로운 어플리케이션, OS 설정변경 등을 할 때 사용
   - apt
     - 데비안과 우분투를 포함하여 데비안 계열 리눅스 배포판들의 주 패키지 관리 도구
     - apt / apt-get / apt-cache / apt-key / apt-add-repository / apt-mark / apt-configapt-cdrom / apt-extracttemplates / apt-ftparchive / apt-sortpkgs / aptdcon 등 명령어가 있다
   - update
     - 설치가능한 패키지 최신화
     - upgrade 명령어가 실제 업데이트임

2. 아파치2 설치) sudo apt install apache2

   [localhos](http://localhost)t에 연결이 안되어서 apache2 -v로 버전확인을 해보니 설치를 했는데 설치가 안되었다고 함

   연습 영상과 버전이 다르므로 ubuntu 20.4 버전으로 apache2 설치를 해보자

   https://nyangnyangworld.tistory.com/4

   - sudo apt-get update
   - sudo apt-get install apache2

   문제없이 잘 된다.

3. mysql 서버 설치) sudo apt install mysql-server

1. php 설치) sudo apt install php php-mysql

하면 Apache2 + php + mysql 설치가 끝난다 하지만 과제는 수동 설치이므로 다시 초기화를 하고 설치를 해보자