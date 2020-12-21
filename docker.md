# How to docker install

## install sshd
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   shell> yum install -y openssh-server
>  >   shell> yum install -y openssh-client
>  >   shell> yum install -y openssh-askpass
>  >   shell> vi /etc/ssh/sshd_config
>  >   .
>  >   .
>  > # If you want to change the port on a SELinux system, you have to tell
>  > # SELinux about this change.
>  > # semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
>  > #Port 22 # <- 주석을 해제 한다.
>  > #AddressFamily any
>  > #ListenAddress 0.0.0.0
>  > #ListenAddress ::
>  >   .
>  >   .
>  >```
> ---------------------------------------------------------------------------------------------

## selinux 해제
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   # 개발 및 테스트 목적으로 해제한 상태로 사용
>  >   
>  >   shell> vi /etc/selinux/config
>  >   
>  > # This file controls the state of SELinux on the system.
>  > # SELINUX= can take one of these three values:
>  > #     enforcing - SELinux security policy is enforced.
>  > #     permissive - SELinux prints warnings instead of enforcing.
>  > #     disabled - No SELinux policy is loaded.
>  > #SELINUX=enforcing
>  > SELINUM=disabled
>  > # SELINUXTYPE= can take one of three values:
>  > #     targeted - Targeted processes are protected,
>  > #     minimum - Modification of targeted policy. Only selected processes are protected.
>  > #     mls - Multi Level Security protection.
>  > SELINUXTYPE=targeted
>  > 
>  >   shell> reboot
>  >```
> ---------------------------------------------------------------------------------------------

## 방화벽 해제
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   # 개발 및 테스트 목적으로 해제한 상태로 사용
>  >   
>  >   shell> iptables -F
>  >   
>  >```
> ---------------------------------------------------------------------------------------------

## yum 을 이용하여 기존 설치된 패키지의 Update 및 필요한 패키지 설치
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   shell> yum update -y
>  >   shell> yum group install -y "Development Tools"
>  >   shell> yum install -y net-tools
>  >   shell> yum install -y bind-utils
>  >   shell> yum install -y rsync
>  >   shell> yum install -y wget
>  >   
>  >   shell> iptables -F
>  >   
>  >```
> ---------------------------------------------------------------------------------------------

## yum 을 이용한 docker 설치 및 서비스 시작
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   shell> yum install docker docker-registry
>  >   shell> systemctl enable docker.service
>  >   shell> systemmctl start docker.service
>  >   shell> systemctl status docker
>  >   
>  >```
> ---------------------------------------------------------------------------------------------

## docker 를 이용한 Apache Web Service Container 생성하기
> ---------------------------------------------------------------------------------------------
>  >
>  >```
>  >   shell> docker search a84146943/centos7-httpd2.4-php7.2:latest
>  >   shell> docker pull a84146943/centos7-httpd2.4-php7.2
>  >   shell> docker run -dit --name centos7  -p [HOST IP ADDR]:[PORT]:[ContainerPort] --privileged -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup:ro [이미지아이디] /usr/sbin/init
>  >   shell> docker exec -it centos7 /bin/bash
>  >```
> ---------------------------------------------------------------------------------------------

## 경험한 장애 유형
> ---------------------------------------------------------------------------------------------
>  > # Apache 컨테이너가 실행중인데 http 접속이 안되는 경우( 브라우져에서 접속 등 )
>  >```
>  >   shell> docker search a84146943/centos7-httpd2.4-php7.2:latest
>  >   shell> docker pull a84146943/centos7-httpd2.4-php7.2
>  >   shell> docker run -dit --name centos7  -p [HOST IP ADDR]:[PORT]:[ContainerPort] --privileged -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup:ro [이미지아이디] /usr/sbin/init
>  >   shell> docker exec -it centos7 /bin/bash
>  >```
> ---------------------------------------------------------------------------------------------










docker commit -a 'yym' -m 'centos7-httpd2.4[80:50080]-php7.2.34' centos7-httpd2.4-php7.2.34:20201219_02
docker images
docker tag centos7-httpd2.4-php7.2 2c78cd7c7cc2
docker tag e31efd6cc327 a84146943/centos7-httpd2.4-php7.2
docker images
docker push a84146943/centos7-httpd2.4-php7.2


docker network