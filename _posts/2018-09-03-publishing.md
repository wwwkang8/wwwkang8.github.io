---
title: "원격 서버에 소스 배포하기 1단계"
date: 2018-09-03 08:26:28 -0400
categories: AWS EC2
---

## 1-4강 : 원격 서버에 소스코드 배포하기 1단계
### 참고문서 : https://github.com/slipp/jwp-book  챕터3에서 확인하기

### 1) AWS 리눅스 서버 생성하기 : https://jojoldu.tistory.com/259?category=635883
### 2) Windows에서 리눅스 인스턴스로 접속하는 방법(https://opentutorials.org/module/1946/11280)
    - 넷사랑 Xshell 6 다운로드(https://www.netsarang.co.kr/download/main.html) : window 환경에서 리눅스 명령어로 원격 서버에 접속할 수 있게         해주는 프로그램이다.
    - AWS EC2 인스턴스 --> 우클릭 "연결" : SSH에 어떻게 접속하는지에 대한 정보를 알려준다. 
    - Xshell 6 새로만들기 클릭 : 원격 컴퓨터에 접속을 하기 위한 정보를 입력하는 것이다.
      [연결]
      - 이름 : 원격서버의 닉네임을 지어주는 것이다. 어떤 것으로 해도 가능.
      - 프로토콜 : SSH 선택
      - 호스트 : 인스턴스의 "연결" 정보에서 4번의 퍼블릭 DNS를 통해 연결에 해당하는 주소를 붙여넣는다.
     [연결 > 사용자 인증]
      -  방법 : public key
      - 사용자 이름 : ec2-user@ec2-10-18~~~~~ 써 있는 곳에서 @ 앞부분 "ec2-user"에 해당하는 것이 사용자 이름이다.
      - 사용자 키 : pem 파일이 저장되어 있는 곳으로 경로 지정.
      - 암호 : 빈칸으로 놓는다.
     [접속]
      - 닉네임 우클릭 --> 열기 클릭 : 해당 원격 서버에 연결이 된다.
      - SSH 보안경고 : 수락 및 저장 클릭.
      - 접속 끊기 : exit 명령어 입력
      
### 3) ubuntu 계정에서 root 계정으로 활성화 시키기(이게 어려웠다)
    - 참고 블로그 : http://app-developer.tistory.com/94, http://ryanwoo.tistory.com/6, http://lab4109.blogspot.com/2013/10/aws-ec2-root.html
    - $ sudo passwd root : root 계정의 비밀번호를 바꾸는 작업
    - $ sudo vi /etc/ssh/sshd_config : PermitRootLogin을 yes로 바꾸어준다. (:wq! 명령어를 써서 저장하고 나간다)
    - $ sudo cp /home/ubuntu/.ssh/authorized_keys /root/.ssh
    - $ sudo service sshd restart : ubuntu유저의 인증키를 root로 복사하고 sshd를 리스타트한다.
    - ubuntu server 등록정보에 가서 [연결 > 사용자 인증]에서 이름을 "root"로 변경한다.

### 4) adduser 명령어 사용하여 계정 추가하기
    - ~# adduser 추가할 아이디
    - password 2번 입력
    - Full Name[] : 입력, 나머지 Room Number, Work Phone, Home Phone, Other은 Enter키로 스킵할 수 있다.
    - Is the information correct? [Y/n] Y
    - cd /home : home 폴더로 이동
    - cd wwwkang7 : home 폴더의 wwwkang7 계정으로 이동.
### 5) 생성한 계정에 권한 부여하기
    - vi /etc/sudoers : 생성한 계정들의 권한을 부여할 수 있는 창이 뜬다. (단축키 i : insert모드, esc : 입력모드에서 나온다)
    - 권한 추가 : wwwkang ALL=(ALL:ALL) ALL 이렇게 해서 wwwkang에 권한을 부여한다.(주의 : 숫자로 계정 이름을 끝내면 안된다. 문자로 끝내기)
    - 저장하고 나오기 : :wq!를 치면 저장하고 나온다.
    
### 6) 생성한 계정으로 재접속하기 --> 이 부분이 안된다. 암호 입력을 해도 계속 틀리게 나온다.

### 7) 각 계정별 utf-8 인코딩 설정
    - sudo locale-gen ko_KR.EUC-KR ko_KR.UTF-8
    - sudo dpkg-reconfigure locales
    - Home 디렉토리의 .bash_profile에 다음 설정 추가 --> vi .bash_profile 입력하고 enter하면 해당 파일이 생성되고 입력 가능.
      - LANG="ko_KR.UTF-8"
      - LANGUAGE="ko_KR:ko:en_US:en"
    - $ source .bash_profile : 입력한 UTF 설정을 바로 반영할 수 있도록 하는 
    - $ env : 이 명령어를 실행해서 설정을 확인할 수 있다.
    - 
