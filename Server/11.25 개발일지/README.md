# TIL

11월 25일 개발일지

1. - wget <http://mirror.navercorp.com/apache//apr/apr-1.7.0.tar.gz`>

     `wget <http://mirror.navercorp.com/apache//apr/apr-util-1.6.1.tar.gz`>

     - wget - web get의 약어로 웹 상의 파일을 다운로드 할 때 사용하는 명령어

     - apr, apr-util 다운로드

     - wget을 사용하지 말고 직접 해보자.

       - 먼저 https://apr.apache.org/download.cgi 에서 apr,apr-util 다운로드

         ![스크린샷 2021-11-24 오후 8.44.17.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8897bae1-58e0-42c9-a6ed-4850d90441ce/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-11-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.44.17.png)

       - `tar xvfz apr-util-1.6.1.tar.gz`

         `tar xvfz apr-1.7.0.tar.gz`

         - tar xvfz는 압축을 푸는 명령어이다
         - 밑에 명령어들을 xvfz등으로 조합해 사용한다고 한다.

         ```
         tar [OPTION...] [FILE]...
                 -f     : 대상 tar 아카이브 지정. (기본 옵션)
                 -c     : tar 아카이브 생성. 기존 아카이브 덮어 쓰기. (파일 묶을 때 사용)
                 -x     : tar 아카이브에서 파일 추출. (파일 풀 때 사용)
                 -v     : 처리되는 과정(파일 정보)을 자세하게 나열.
                 -z     : gzip 압축 적용 옵션.
                 -j     : bzip2 압축 적용 옵션.
                 -t     : tar 아카이브에 포함된 내용 확인.
                 -C     : 대상 디렉토리 경로 지정.
                 -A     : 지정된 파일을 tar 아카이브에 추가.
                 -d     : tar 아카이브와 파일 시스템 간 차이점 검색.
                 -r     : tar 아카이브의 마지막에 파일들 추가.
                 -u     : tar 아카이브의 마지막에 파일들 추가.
                 -k     : tar 아카이브 추출 시, 기존 파일 유지.
                 -U     : tar 아카이브 추출 전, 기존 파일 삭제.
                 -w     : 모든 진행 과정에 대해 확인 요청. (interactive)
                 -e     : 첫 번째 에러 발생 시 중지.
         ```

       - apr-1.7.0 설치

         `cd apr-1.7.0`

         apr-1.7.0 폴더로 이동한다.

         `/configure --prefix=/usr/local/apr`

         configure는 소스파일에 대한 환경설정을 해주는 명령이다.

         `make`

         소스를 컴파일 하여 실행 가능한 파일로 만들어 준다. 설치파일로 만들어 준다고 생각하면 편할거 같다.

         `make install`

         설치파일을 실행한다(설치한다)

       - apr-util-1.6.1 설치

         `cd apr-util-1.6.1`

         `make`

         `make install`

         위 apr 설치와 똑같이 하려 했는데

         오류가 뜬다

     ## 