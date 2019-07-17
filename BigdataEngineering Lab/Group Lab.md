# Team2 

* 조장 : 이인선  
![이인선](https://user-images.githubusercontent.com/48976549/61349022-9b478c00-a89d-11e9-8ebf-7afcae0989e1.PNG)
* 조원 : 김중호  
![김중호](https://user-images.githubusercontent.com/48976549/61349020-9aaef580-a89d-11e9-9daa-fc1d4eb8e619.PNG)
* 조원 : 윤산성  
![윤산성](https://user-images.githubusercontent.com/48976549/61349027-9c78b900-a89d-11e9-8c56-dfe7dfab83b7.PNG)
* 조원 : 이선민  
![이선민](https://user-images.githubusercontent.com/48976549/61349018-997dc880-a89d-11e9-82eb-bf50f076a2f4.PNG)


## CM Install Lab

### System Configuration Checks <all nodes>

#### 1. Check vm.swappiness on all your nodes
```
sudo sysctl vm.swappiness=1
sudo sh -c "echo 'vm.swappiness=1'>> /etc/sysctl.conf"
```

#### 2. Show the mount attributes of your volume(s)

```
df -Th

sudo fdisk -l
```

#### 3. If you have ext-based volumes, list the reserve space setting

#### 4. Disable transparent hugepage support [all nodes!!]
Hugepage ?  
일반적인 컴퓨팅 시스템은 물리적 메모리 크기를 극복하기 위해 가상메모리 기법을 사용하며, 상이한 두 메모리를 매핑하기 위해 Page Table이 존재하고 Page 단위로 관리
```
sudo sh -c "echo never > /sys/kernel/mm/transparent_hugepage/defrag"
sudo sh -c "echo never > /sys/kernel/mm/transparent_hugepage/enabled"
sudo sh -c "echo 'echo never > /sys/kernel/mm/transparent_hugepage/defrag' > /etc/rc.local" sudo sh -c "echo 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' >
/etc/rc.local" sudo cat /etc/rc.local
```

#### 5. List your network interface configuration
```
ifconfig
```

#### 6. Show that forward and reverse host lookups are correctly resolved
* For /etc/hosts, use getent  
* For DNS, use nslookup  
```
vi /etc/hosts

# 아래 내용 추가
172.31.13.194 util.com util
172.31.19.171 mn.com mn
172.31.13.237 dn1.com dn1
172.31.15.190 dn2.com dn2
172.31.5.181 dn3.com dn3

getent hosts
```

```
sudo yum install bind-utils net-tools -y

nslookup [도메인명]
```

* Hostname modification for each node
```
# 각각의 node ( 노드 명은 약어 말고 full name 으로 해주는 게 좋음)
sudo hostnamectl set-hostname <노드명>
예) sudo hostnamectl set-hostname node1.sk.com

# 변경된 hostname 확인
hostname
```

#### 7. Show the nscd service is running
```
sudo yum -y install nscd  
sudo systemctl enable nscd  
sudo systemctl start nscd  
sudo systemctl status nscd  
```

####8. Show the ntpd service is running
```
sudo yum -y install ntp sudo chkconfig ntpd on  
sudo systemctl enable ntpd  
sudo systemctl start ntpd

# 확인
ntpq -p
```

#### 추가 작업
* sshd_config setting for each node  
SSH를 사용하여 EC2 인스턴스에 로그인할 때 키 페어 대신에 암호 로그인을 활성화
패스워드 인증 허용
( all nodes ! - 진행 안하면 클러스터 안올라감 )
```
sudo vi /etc/ssh/sshd_config
# PasswordAuthentication -> yes 로 변경 후 저장

# sshd 재시작 및 상태확인
sudo systemctl restart sshd.service
sudo systemctl status sshd.service [not found 인 경우도 있음]
```

* Install dependencies using yum - all node  
```
sudo yum update
sudo yum install -y wget

# 전체 y 선택
```

## Cloudera Manager Install Lab
https://www.cloudera.com/documentation/enterprise/5-15-x/topics/install_cm_cdh.html  

### Path B install using CM 5.15.x
* all nodes
```
sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo -P /etc/yum.repos.d/

sudo vi /etc/yum.repos.d/cloudera-manager.repo
수정 => baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.15.2/
```

* util
```
# rpm에 key 추가
sudo rpm --import https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera

# cloudera install
sudo yum install -y cloudera-manager-daemons cloudera-manager-server
```

#### • Install a supported Oracle JDK on your first node(util)
```
# java version 확인
sudo yum list oracle*

sudo yum install -y oracle-j2sdk1.7
```
* java 경로 설정
```
vi ~/.bash_profile

# 아래 두줄 추가
# :$PATH가 뒤에 있어야 java -version 했을 때 정상적인 버전 확인 가능
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
export PATH=/usr/java/jdk1.7.0_67-cloudera/bin:$PATH

source ~/.bash_profile

# java version 확인
java -version
```

#### • Install a supported JDBC connector on all nodes
sqoop 사용 시 모든 node 연결 위해 설치
```
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz
tar zxvf mysql-connector-java-5.1.47.tar.gz
sudo mkdir -p /usr/share/java/
cd mysql-connector-java-5.1.47
sudo cp mysql-connector-java-5.1.47-bin.jar /usr/share/java/mysql-connector-java.jar
cd /usr/share/java/
sudo yum install -y mysql-connector-java
```

#### • Create the databases and access grants you will need (util)
```
# maria db 설치
sudo yum install -y mariadb-server

sudo systemctl enable mariadb
sudo systemctl start mariadb
# 잘 떴는지 확인
sudo systemctl statuc mariadb
```

```
# 권한 설정
sudo /usr/bin/mysql_secure_installation
```

#### • Configure Cloudera Manager to connect to the database

* Creating Databases for Cloudera Software
```
mysql -u root -p

# db, user 생성
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE aman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON aman.* TO 'aman-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON rman.* TO 'rman-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON hue.* TO 'hue-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry-user'@'%' IDENTIFIED BY 'password';

CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie-user'@'%' IDENTIFIED BY 'password';

FLUSH PRIVILEGES;
```
#### • Start your Cloudera Manager server -- debug as necessary

```
#모든 Node에 비밀번호 설정 (*중요*)
sudo passwd centos
```

```
#  set CM DB
sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm-user password

sudo systemctl start cloudera-scm-server
sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log
```

#### • Do not continue until you can browse your CM instance at port 7180

## Cloudera Manager Install Lab
```
<선택>
C:\Windows\System32\drivers\etc > hosts 파일 메모장에서 편집
15.164.82.177 util.com util <- 퍼블릭IP 로 도메인 추가
```
### Install a cluster and deploy CDH
```
http://util.com:7180
admin / admin
- trial 선택
- 화면상에 Could not connect to host. 라고 보이는 경우 ssh server가 제대로 설치되었는지 확인
# sudo yum install openssh-server
# /sbin/service sshd status
# /sbin/service sshd start
# ssh localhost
```

* Cloudera manager 시작  

* CDH 클러스터 설치에 대한 호스트 지정  

* 클러스터 설치 (수정사항 없음)  
- JDK 설치 옵션  
직접 java 설치 진행했기 때문에 uncheck


#### • Do not use Single User Mode. Do not. Don't do it.

- ssh 로그인 정보 제공



#### • Ignore any steps in the CM wizard that are marked (Optional)

#### • Install the Data Hub Edition

#### • Install CDH using parcels

#### • Deploy only the Core set of CDH services.

#### • Deploy three ZooKeeper instances.
o CM does not tell you to do this but complains if you don't
