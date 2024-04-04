# ON PREMISE WAS 배포관리(우분투 서버)

클라우드 서비스의 비용 부담을 해결하고 실제 서버 운영 및 관리 경험을 얻기 위해 개인 서버를 구축하여 웹 애플리케이션을 배포하는 프로젝트입니다. 이 프로젝트는 서버 성능 최적화 및 보안 강화에 중점을 두어 개발되었습니다.

1. 3/29 - 구입한 mini pc가 도착하다. 할인에 할인을 또받에 거의 11만원이라는 엄청 싼 가격에 구매!! 득템!!

![Untitled](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/Untitled.png)

![KakaoTalk_Photo_2024-04-04-07-57-50.jpeg](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/KakaoTalk_Photo_2024-04-04-07-57-50.jpeg)

![Untitled](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/Untitled%201.png)

1. 2024/3/29
    
    저녁을 먹고 서버 설치 시작! 
    
    장기 지원이 된다는 LTS 버전이 믿음이가서 Ubuntu Server [22.04 LTS](https://discourse.ubuntu.com/t/jammy-jellyfish-release-notes/24668) 를 받기로 하였다. 
    
    1) 우분투 서버 버전 다운받기 
    
    [Get Ubuntu Server | Download | Ubuntu](https://ubuntu.com/download/server)
    
    2) Rufus 로 설치 USB만들기
    
    [Rufus - 간편하게 부팅 가능한 USB 드라이브 만들기](https://rufus.ie/ko/)
    

3) 설치하기 

… miniPC에 윈도우가 깔려있는데 F11이랑 F2랑 DEL키를 아무리 눌러도 USB설치를 위한 BIOS진입이 되지 않는다.. 털석…검색검색!

[윈도우11 컴퓨터 에서 BIOS 화면으로 넘어 가는 방법 - HowToDoIT](https://blog.howtodoit.kr/윈도우11-컴퓨터-에서-bios-화면으로-넘어-가는-방법/)

여자저차 해서 윈도우에서 종료시에 BIOS로 진입하게 하는 설정으로 해줘야되는 것을 찾아내었다.

4) 진짜 설치하기

[Ubuntu 22.04 Server 설치 가이드 - Junorion 블로그](https://junorionblog.co.kr/ubuntu-22-04-server-설치-가이드/)

기본적인 설정을 확인만 누르면서 설치하면 되는 느낌이지만 마지막에 SSH와 Docker를 미리 옵션으로 설치해주었다. 

우분투 서버가 성공적으로 설치되었다. 터미널이 보이기 시작하였다. 

이제 도커를 설치한다.

5) 도커설치

도커를 설치시 옵션으로 따라오는 버전이 아닌 최신 버전으로 새로 설치해주었다. 

```java
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

6) 외부에서 내 서버에 접속하기 셋팅(SSH)

SSH 서버 설치 및 포트설정하기!

[https://ca.ramel.be/74](https://ca.ramel.be/74)

나는 터미널로 iterm2를 사용하기때문에 자동셋팅 방법으로 적용해보았다.

[https://chaelin0722.github.io/mac/ssh_save/](https://chaelin0722.github.io/mac/ssh_save/)

7) 다 셋팅이 되었다… 접속을 시도해보았다… 안된다… 안된다…

왜 안될까.. ???

hostname -I 를 다시 썻다. 나는 왜 아이피가 두개가 나올까????

188.99.244.199 177.11.0.1 (실제 아이피 아님) 이렇게 나오는데 왜 이러지?

8) 원인을 찾았다. 공유기 네트워크를 통해 들어오기 때문에 포트포워딩이 필요한 것이었다.

[KT 공유기 포트포워딩 설정하기](https://atnbt.com/케이티-공유기-포트포워딩/)

공유기에 SSH접속 포트번호를 할당한후 접속하니 마법 같이 접속이 되었다. 

## 9) 드디어 대망의 나만의 DB 서버를 만드는 시간이 되었다

mysql 설치 

1. `docker pull mysql:8.0`.36
2. 

**MySQL 컨테이너 실행**: 이미지를 가져온 후, 아래 같은 명령어로 MySQL 컨테이너를 실행할 수 있었다. 환경 변수를 통해 MySQL 인스턴스의 초기 설정 성공

```
docker run --name mysql -v mysql-db-volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1367854a -p 3306:3306 -d mysql:8.0.36

```

데이터를 컨테이너 외부에 저장하여 삭제해도 데이터가 유지되게 하였다.

```

docker run --name mysql -v mysql-db-volue:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1367854a -d mysql:8.0.36

여기서 **`my-db-volume`**은 호스트 시스템에 데이터를 저장할 볼륨의 이름입니다.
```

[https://poiemaweb.com/docker-mysql](https://poiemaweb.com/docker-mysql)

### **2. MySQL 클라이언트를 통한 데이터베이스 접속**

컨테이너 내부에서 MySQL 클라이언트를 실행하여 MySQL 서버에 접속할 수 있습니다. 루트 사용자로 MySQL에 로그인하려면 다음 명령어를 사용합니다:

```bash
mysql -u root -p
```

이 명령어를 실행하면, 설정한 루트 비밀번호를 입력하라는 메시지가 나타나고. 

비밀번호를 입력하면 MySQL 프롬프트로 이동하여 SQL 쿼리를 실행할 수 있습니다.

### **3. 데이터베이스 및 사용자 관리**

MySQL 프롬프트에서 다양한 데이터베이스 관리 작업을 수행할 수 있습니다. 

예를 들어, 새 데이터베이스를 생성하거나 사용자를 추가하는 등의 작업입니다.

- **데이터베이스 생성**:

```sql
CREATE DATABASE mydatabase; 너무 뻔하게 지었나?? 내 데이터베이스 맞다.
```

- **새 사용자 생성 및 권한 부여**:

```sql
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON mydatabase.* TO 'myuser'@'%';
FLUSH PRIVILEGES;
```

##오라클에서 권한 설정을 해본적은 있지만 MySql은 처음이라 GPT에게 물어보았다.

GPT 답변 : 

MySQL에서는 오라클 데이터베이스의 **`GRANT RESOURCE, CONNECT, DBA`**에 해당하는 직접적인 권한 세트가 없습니다. 오라클의 이러한 권한들은 데이터베이스 객체를 생성하고, 데이터베이스에 연결하며, 데이터베이스 관리 권한을 부여하는 데 사용됩니다. MySQL에서는 사용자에게 필요한 권한을 개별적으로 부여해야 합니다.

MySQL에서 비슷한 권한을 부여하기 위해 다음과 같은 권한을 사용할 수 있습니다:

- **`CONNECT` 권한**: MySQL에서는 이 권한이 명시적으로 존재하지 않습니다. MySQL에서 사용자 계정을 생성하면 자동으로 데이터베이스에 연결할 수 있는 권한이 부여됩니다.
- **`RESOURCE` 권한**: 이 권한에 해당하는 것은 MySQL에서 **`CREATE`**, **`ALTER`**, **`DROP`**, **`INDEX`**, **`CREATE TEMPORARY TABLES`** 등의 권한으로, 사용자가 데이터베이스 객체를 생성하고 관리할 수 있게 합니다.
- **`DBA` 권한**: MySQL에서는 **`GRANT ALL PRIVILEGES`**를 사용하여 데이터베이스의 모든 권한을 부여할 수 있지만, 이는 **`DBA`**와 같은 최고 수준의 권한을 의미합니다. 시스템 관리와 관련된 추가 권한(**`SUPER`**, **`RELOAD`** 등)은 별도로 부여해야 할 수 있습니다.

따라서, 오라클의 **`RESOURCE`**, **`CONNECT`**, **`DBA`**에 해당하는 MySQL의 권한 부여 예시는 다음과 같습니다:

```sql
sqlCopy code

CREATE USER 'madang'@'%' IDENTIFIED BY 'madang';

-- 기본적인 데이터베이스 사용 및 객체 생성 권한 부여
GRANT CREATE, ALTER, DROP, INDEX, SELECT, INSERT, UPDATE, DELETE, EXECUTE, SHOW VIEW ON mydatabase.* TO 'madang'@'%';

-- 필요에 따라 추가 시스템 관리 권한 부여
-- GRANT SUPER, RELOAD ON *.* TO 'myuser'@'%';

FLUSH PRIVILEGES;
 ->

```

이렇게 사용자에게 필요한 권한을 세밀하게 조정할 수 있습니다. 실제 권한 부여 시에는 보안을 고려하여 최소 권한 원칙을 따르는 것이 좋습니다. 사용자에게 정말 필요한 권한만을 부여하여, 데이터베이스의 보안을 강화해야 합니다.

MySQL 프롬프트에서 **`FLUSH PRIVILEGES;`** 명령을 입력한 후 화살표(**`->`**)가 나타나는 것은, MySQL이 더 많은 입력을 기다리고 있다는 신호입니다. 이는 주로 SQL 명령이 완전히 끝나지 않았거나, 세미콜론(**`;`**)으로 명령의 종료를 명확히 하지 않았을 때 발생합니다.

MySQL 프롬프트(**`mysql>`**)에서 현재의 사용자 연결을 바꾸고 다른 사용자로 로그인하려면, 직접적인 명령어나 옵션이 존재하지 않습니다. 대신, 현재의 MySQL 세션을 종료하고 새로운 사용자로 다시 접속해야 합니다. 이 과정은 몇 가지 단계로 이루어집니다:

### **1. 현재 MySQL 세션 종료**

현재 세션을 종료하려면, **`exit`** 또는 **`quit`** 명령어를 사용합니다.

```sql
exit;
```

또는

```sql
quit;
```

이 명령을 실행하면, MySQL 프롬프트에서 나와서 시스템의 커맨드 라인으로 돌아갑니다.

### **2. 새 사용자로 MySQL에 로그인**

커맨드 라인에서 다음 명령어를 사용하여 새 사용자로 MySQL에 로그인합니다. **`newuser`**와 **`newpassword`**는 실제 사용자 이름과 비밀번호로 대체해야 합니다.

```bash
mysql -u newuser -p
```

명령어 실행 후에 시스템이 비밀번호를 요청하면, 새 사용자의 비밀번호를 입력합니다. 성공적으로 인증이 완료되면, 새 사용자의 **`mysql>`** 프롬프트에 접근할 수 있습니다.

이제 해보자! 실행중인 컨테이너에서 mysql을 exec 해주고 bash 입력창으로 불러온다.

![스크린샷 2024-04-04 오전 9.57.16.png](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-04-04_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.57.16.png)

SUDO 자꾸 배먹고 암호 어렵게 했더니 자꾸 틀리지만 안전하다고 위로한다. 

![스크린샷 2024-04-04 오전 9.57.50.png](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-04-04_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.57.50.png)

bash 안에서 mysql 유저네임 패스워드 명령어를 넣어 실행한다. 

![스크린샷 2024-04-04 오전 9.58.23.png](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-04-04_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.58.23.png)

mysql이 실행되었다!

![스크린샷 2024-04-04 오전 9.58.35.png](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-04-04_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.58.35.png)

임시로 넣어놓은 테이블과 데이터베이스 스키마가 잘 검색되나 해본다.

(스키마 안들어가고 검색해서 첫시도 실패!!!) 

![스크린샷 2024-04-04 오전 9.59.24.png](ON%20PREMISE%20WAS%20%E1%84%87%E1%85%A2%E1%84%91%E1%85%A9%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5(%E1%84%8B%E1%85%AE%E1%84%87%E1%85%AE%E1%86%AB%E1%84%90%E1%85%AE%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5)%20737f2b6e39434cb8b5d3ae1010c190be/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-04-04_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_9.59.24.png)

# 이제 나는 외부에서 접속할 수 있는

# 나만의 DB를 갖게 되었다!!!! 🏆🏆🏆🏆🏆🏆