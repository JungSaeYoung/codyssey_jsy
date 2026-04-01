# codyssey_jsy
## 미션 요약: 개발 워크스테이션 구축

### 목표
터미널 + Docker + Git/GitHub를 직접 세팅하고, 재현 가능한 개발 환경을 만드는 경험

### 제출물
GitHub 저장소 하나에 모든 결과물 포함

| # | 항목 | 핵심 내용 |
|---|------|-----------|
| 1 | 터미널 조작 로그 | pwd, ls -a, mkdir, cp, mv, rm, cat, touch 등 명령어 + 출력 기록 |
| 2 | 권한 실습 | chmod로 파일/디렉토리 권한 변경 전/후 비교 (최소 각 1개) |
| 3 | Docker 설치 점검 | docker --version, docker info 결과 기록 |
| 4 | Docker 기본 운영 | images, ps, logs, stats + hello-world 실행 + ubuntu 컨테이너 진입 |
| 5 | 커스텀 Dockerfile 및 바인드 마운트 테스트 | (A) nginx 등 웹서버 베이스 또는 (B) ubuntu/alpine 베이스로 커스텀 이미지 빌드 |
| 6 | 포트 매핑 + 볼륨 | -p 포트 매핑 접속 증거 + Docker 볼륨 영속성 증명 (컨테이너 삭제 후 데이터 유지) |
| 7 | Git/GitHub 연동 및 동작 | git config 설정 + GitHub 저장소 연동 증거 및 push 과정 |
| 8 | 트러블슈팅 | 진행했던 것 중 마주한 문제 |
| 9 | 보너스 문제 | Docker Compose 단일 서비스 및 멀티 컨테이너 |

### README.md 필수 포함 사항

프로젝트 개요 / 실행 환경 / 수행 체크리스트 / 검증 방법

트러블슈팅 2건 이상 (문제 → 원인 → 해결)

민감정보 마스킹

### 보너스

Docker Compose 단일 서비스 실행

Docker Compose 멀티 컨테이너 (웹서버 + 보조서비스) + 컨테이너 간 통신 확인

### 환경 특이사항

Mac + OrbStack 사용 (sudo 없이 Docker 실행 가능)

### 세부 실행 환경

1. OS 환경

ashofrondol9475@c3r8s7 ~ % sw_vers 
ProductName:		macOS
ProductVersion:		15.7.4
BuildVersion:		24G517

2. 쉘 종류

ashofrondol9475@c3r8s7 ~ % echo $SHELL
/bin/zsh

3. 도커 버전
ashofrondol9475@c3r8s7 ~ % docker --version
Docker version 28.5.2, build ecc6942

4. 컨테이너 OS 정보
root@a0e013d547db:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.4 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo

5. 컨테이너 커널 정보
root@a0e013d547db:/# uname -a
Linux a0e013d547db 6.17.8-orbstack-00308-g8f9c941121b1 #1 SMP PREEMPT Thu Nov 20 09:34:02 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

6. 칸테이너 쉘 종류
root@a0e013d547db:/# echo $SHELL
/bin/bash

7. Git 버전
root@a0e013d547db:/# git --version
git version 2.43.0

## 환경 설정 방법
Mac 환경에 설치되어 있는 OrbStack으로 CLI 없이 환경 설정을 진행하려 했으나, GUI에서 컨테이너를 여는게 불가능 했다.
따라서 OrbStack과 함께 설치되어 있는 Docker에서 Ubuntu:latest 이미지를 pull 하여 Terminal로 열었다.
따로 이미지를 설치하지 않으면 자동으로 Alpine Linux로 컨테이너가 열린다.

컨테이너 내에서 git 패키지 설치는 

apt update && apt install -y git

로 진행한다. apt update를 하지 않으면 패키지 목록을 찾지 못하기 때문에 꼭 먼저 실행해줘야 한다.

Git의 경우 VS Code에서 Source Control 기능이 있어서 이를 통해 commit 및 push를 진행한다.

만약 CLI를 통해 commit 및 push 작업을 진행할 경우, config를 통해 사용자명과 이메일을 입력해야 하고, 추가적으로 push 할 때마다 github에서 발급받은 토큰을 password로 입력해야 한다.

추가적으로, VS Code에서 Dev Container라는 Extension을 설치해 VS Code로 컨테이너에 연결함으로써 최종 환경을 구성했다.


## 1. 터미널 조작 로그
*해당 과제 수행 이후 다른 컨테이너로 변경하게 되어 실행 환경이 달라졌다. 때문에 ID가 다를 수 있다. 

root@da9ce8573016 /codyssey_jsy main*
❯ pwd
/codyssey_jsy

현재 잡고있는 디렉터리 위치를 알려주는 pwd 명령어를 실행했다. 

root@da9ce8573016 /codyssey_jsy main*
❯ ls -la
drwxr-xr-x root root  26 B  Mon Mar 30 09:10:42 2026 .
drwxr-xr-x root root  68 B  Mon Mar 30 08:57:42 2026 ..
drwxr-xr-x root root 138 B  Mon Mar 30 09:13:17 2026 .git
.rw-r--r-- root root 1.4 KB Mon Mar 30 09:10:35 2026 README.md

ls는 따로 디렉터리를 지정하지 않으면 현재 위치의 디렉터리 안에 있는 파일과 폴더를 결과로 반환받는다.
매개 값인 l과 a는 각각 하는 일이 다르다.
l의 경우 파일의 상세 정보를 표시해준다.
위 로그를 확인해보면 또 다른 점을 알 수 있는데, d로 시작하는 경우와 그렇지 않은 경우를 볼 수 있다.
d로 시작하는 경우는 폴더를 나타낸다. 그렇지 않은 경우는 파일로 식별할 수 있다.
a의 경우 숨김 파일을 포함해 반환하라는 매개값이다.

Ubuntu의 경우 많이 사용하는 ls -l을 ll로 사용할 수도 있다.

root@da9ce8573016 /codyssey_jsy main*
❯ mkdir test-dir

mkdir은 MaKe DIRectory를 줄인 것으로, 폴더를 생성하는 명령어다.

root@da9ce8573016 /codyssey_jsy main*
❯ touch test-file.txt

touch는 파일을 생성하는 명령어다. "."을 기준으로 파일명과 확장자명을 구분할 수 있다.

root@da9ce8573016 /codyssey_jsy main*
❯ cp test-file.txt
cp: missing destination file operand after 'test-file.txt'
Try 'cp --help' for more information.

cp 명령어는 CoPy를 줄인 명령어인데, 어떤 이름으로 복사할지 지정해주지 않아 오류가 난 모습이다.

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  test-dir  test-file.txt

다시 한번 ls로 디렉토리 내용물을 확인했다.

root@da9ce8573016 /codyssey_jsy main*
❯ cp test-file.txt copy-file.txt

이번엔 cp "복사 대상 파일" "복사 결과 파일명"으로 제대로 작동시켰다.

root@da9ce8573016 /codyssey_jsy main*
❯ mv copy-file.txt renamed.txt

이어서 mv 명령어인데, MoVe를 줄인 것이다. 이름과 달리 파일명을 수정할 때도 쓸 수 있는데, mv "대상 파일" "변경될 이름"으로 사용했다.

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  renamed.txt  test-dir  test-file.txt

결과를 확인해보면 이름이 바뀐 것을 확인할 수 있다.

root@da9ce8573016 /codyssey_jsy main*
❯ cat renamed.txt

cat은 ConcAtenaTe의 약어이다. 파일 내용을 출력하는 명령어인데, 파일 내용이 아무것도 없어서 아무것도 반환되질 않았다.

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  renamed.txt  test-dir  test-file.txt

다시 한 번 내용물을 확인했다.

root@da9ce8573016 /codyssey_jsy main*
❯ vim renamed.txt

Vim 에디터라는 에디터가 있다. Vi 에디터를 더 쓰기 쉽게 만들어 놓은 것인데. 사실 Vim 에디터도 사용하기 전 단축키를 알아야 쓸 수 있다.
환경 설정에서 설명하지 않았지만, apt install vim을 입력하면 사용할 수 있다.
여기에서는 vim 에디터의 사용 방법을 설명하진 않는다.
파일 안에는 test라고 입력했다.

root@da9ce8573016 /codyssey_jsy main* 14s
❯ cat renamed.txt
test

다시 한 번 cat을 이용해 입력한 내용을 확인해보니 그 내용과 똑같이 출력되는 모습을 확인할 수 있다.

root@da9ce8573016 /codyssey_jsy main*
❯ rm renamed.txt

rm은 ReMove의 약어로, 이름과 같이 파일/폴더를 삭제하는 명령어다.

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  test-dir  test-file.txt

확인해보면 잘 삭제된 것을 확인할 수 있다.

## 2. 권한 실습
root@a0e013d547db:/codyssey_jsy# touch myfile.txt

touch로 myfile.txt를 생성했다.

root@a0e013d547db:/codyssey_jsy# ls -l myfile.txt
-rw-r--r-- 1 root root 0 Mar 31 09:04 myfile.txt

ls -l 과 대상을 지정해서 세부 사항을 보여달라고 한 명령어이다.
앞에는 알아볼 수 없는 -rw-r--r--가 있다. 이는 접근 권한을 표현한 것이다.
여기에는 r, w, x가 있는데 각각 의미가 있다.
r은 읽기 권한(read), w는 쓰기 권한(write), x는 실행 권한(execute)으로 이해하면 된다.
그런데, 각 권한이 뭔가 반복되는 것처럼 보이는데, 이는 이상한 점이 아니다.
첫 번째는 소유자 계정에서 사용할 수 있는 권한을 의미하고, 두 번째는 그룹 사용자가 접근할 수 있는 권한을, 세 번째 자리는 기타 사용자가 접근할 수 있는 권한을 의미한다.
따라서 이를 해석하면 소유자는 root이며, 읽기와 쓰기를 할 수 있다. 그룹 사용자는 읽을 수만 있으며, 기타 사용자도 읽을 수만 있다.

root@a0e013d547db:/codyssey_jsy# chmod 755 myfile.txt

chmod는 CHange MODe의 약어다. 파일이나 폴더의 접근 권한을 변경할 수 있다.
이 때 매개값으로 권한 수준을 숫자로 표현하는데, 이는 다음과 같다.
r : 4
w : 2
x : 1
이 숫자를 합쳐 접근 권한을 변경할 수 있다.
각 숫자의 순서는 위에서도 보았듯이 소유자, 그룹, 기타 사용자 순서이다.

root@a0e013d547db:/codyssey_jsy# ls -l myfile.txt
-rwxr-xr-x 1 root root 0 Mar 31 09:04 myfile.txt

권한 변경이 잘 이루어진 모습이다.

root@a0e013d547db:/codyssey_jsy# mkdir mydir

마찬가지로 폴더도 생성해서 확인해보자.

root@a0e013d547db:/codyssey_jsy# ls -ld mydir
drwxr-xr-x 1 root root 0 Mar 31 09:05 mydir

앞서 설명했듯, 폴더의 경우 d로 시작한다. directory를 줄여놓은 것이다.

root@a0e013d547db:/codyssey_jsy# chmod 700 mydir

똑같이 chmod로 권한을 변경해보자.

root@a0e013d547db:/codyssey_jsy# ls -ld mydir
drwx------ 1 root root 0 Mar 31 09:05 mydir

의도한대로 잘 수정 된 것을 확인할 수 있다.

## 3. Docker 설치 점검

여태까지 잘 사용했지만, 추가로 Docker의 상태를 다시 점검해본다.

OrbStack이 켜진 상태에서 아래 명령어를 Terminal에 입력해보자.

ashofrondol9475@c3r8s7 ~ % docker --version
Docker version 28.5.2, build ecc6942

또, 아래 명령어도 입력해보자.

ashofrondol9475@c3r8s7 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/ashofrondol9475/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/ashofrondol9475/.docker/cli-plugins/docker-compose

Server:
 Containers: 1
  Running: 1
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: edaf9914-5c2d-4675-91b2-714093abe4c0
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set

맨 아래 WARNING이 뜨는데, 크게 신경쓰지 않아도 된다.
Docker의 경우 기뵌적으로 iptables라는 리눅스 방화벽을 사용하는데, 이 기능이 비활성화 되어 있어서 그렇다.
내 경우에서는 도커 컨테이너에 중요한 정보가 없기도 하고, 외부망에 열어 놓은 것도 아니고, 컨테이너를 올 때마다 새로 만들어서 사용하기 때문에 신경쓰지 않았다.

## 4. Docker 기본 운영

여기에서는 Docker의 가장 기본적인 사용 방법을 다룬다.
맥의 터미널을 열어 아래 명령어를 입력해보자.

ashofrondol9475@c3r8s7 ~ % docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    f794f40ddfff   5 weeks ago   78.1MB

그러면 아까 설치했던 ubuntu 이미지가 하나 있는 것을 볼 수 있다.
내 경우에선 ubuntu 이미지가 깔려있지 않았을 때 Alpine Linux로 실행되는 것을 확인했는데, 이미지가 없을 때 무조건 그렇게 실행되는지는 확인해 볼 필요가 있다.

이어서 아래 명령어를 입력해보자.

ashofrondol9475@c3r8s7 ~ % docker ps -a 
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS       PORTS     NAMES
a0e013d547db   ubuntu    "bash"    5 hours ago   Up 5 hours             strange_haibt

지금 실행 중인 컨테이너의 아이디와 image는 어떤 것인지, command type이 뭔지, 언제 만들어졌는지, 현황, 포트, 이름을 확인할 수 있다.

아래 명령어를 사용해보면 로그를 확인할 수 있다.

docker logs 컨테이너이름    # 전체 로그 출력
docker logs -f 컨테이너이름  # 실시간 로그 확인 (Ctrl+C로 종료)
docker logs --tail 10 컨테이너이름  # 마지막 10줄만

로그는 달리 보여줄 것이 없어서 보여주진 않지만, 앞서 docker ps -a 명령어로 확인한 Container ID나 Name을 컨테이너 이름에 넣어주면 작동한다.

docker stats를 입력하면 컨테이너의 실시간 자원 할당량이나 프로세스 아이디를 확인할 수 있다.(나가려면 ctrl + c)

CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O        PIDS 
a0e013d547db   strange_haibt   525.40%   9.395GiB / 15.67GiB   59.94%    73.3MB / 544kB   4.56GB / 643MB   124 

docker stats --no-stream 을 치면 실시간 확인이 아닌 당장의 값만 반환받을 수 있다.

## 5.1. 커스텀 Dockerfile (NGINX)
먼저, 커스텀 Dockerfile을 위한 준비가 필요하다.
가장 먼저 작업 폴더를 준비하자.

root@a0e013d547db:/codyssey_jsy# mkdir -p ~/docker-project/app
root@a0e013d547db:/codyssey_jsy# cd ~/docker-project

root@a0e013d547db:~/docker-project# vim app/index.html

<!DOCTYPE html>
<html>
<head>
    <title>My Docker Web</title>
</head>
<body>
    <h1>Hello Docker!</h1>
    <p>This is my custom NGINX container.</p>
</body>
</html>

위 내용을 vim 에디터로 작성 한 뒤 cat으로 확인해보자.

root@a0e013d547db:~/docker-project# cat app/index.html
<!DOCTYPE html>
<html>
<head>
    <title>My Docker Web</title>
</head>
<body>
    <h1>Hello Docker!</h1>
    <p>This is my custom NGINX container.</p>
</body>
</html>

잘 작성된 것을 확인할 수 있다.
이제 dockerfile을 준비하자.

root@a0e013d547db:~/docker-project# vim Dockerfile
FROM nginx:alpine
COPY app/index.html /usr/share/nginx/html/index.html
EXPOSE 80

root@a0e013d547db:~/docker-project# cat Dockerfile 
FROM nginx:alpine
COPY app/index.html /usr/share/nginx/html/index.html
EXPOSE 80

잘 저장됐는지 확인까지 한다.

위 코드는 다음과 같은 역할을 한다.
FROM nginx:alpine : nginx 웹 서버가 설치된 Alpine 이미지를 베이스로 사용
COPY app/index.html ... : 내가 만든 HTML을 nginx 기본 웹 경로에 복사
EXPOSE 80 : 컨테이너가 80번 포트를 사용한다고 명시

이어서 만든 docker-project를 컨테이너 폴더 바깥으로 옮긴다.
도커 안에서 도커를 여는(DinD, Docker in Docker) 것은 일반적으로 불가능하고, 전용 이미지와 --privileged 옵션이 필요하기 때문이다.
따라서 터미널을 열어 새로운 컨테이너를 생성해서 실행해야 한다.
먼저 이미지를 만들어보자.


ashofrondol9475@c3r8s7 ~ % ls                      
Desktop		Documents	Downloads	Library		Movies		Music		OrbStack	Pictures	Public
ashofrondol9475@c3r8s7 ~ % cd Desktop
ashofrondol9475@c3r8s7 Desktop % ls
docker-project
ashofrondol9475@c3r8s7 Desktop % cd docker-project
ashofrondol9475@c3r8s7 docker-project % ls
Dockerfile	app

가장 먼저 docker-project 폴더로 cd를 통해 넘어간다.

ashofrondol9475@c3r8s7 docker-project % docker build -t my-web .
[+] Building 7.9s (7/7) FINISHED                                                                                                               docker:orbstack
 => [internal] load build definition from Dockerfile                                                                                                      0.2s
 => => transferring dockerfile: 118B                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                           2.9s
 => [internal] load .dockerignore                                                                                                                         0.1s
 => => transferring context: 2B                                                                                                                           0.0s
 => [internal] load build context                                                                                                                         0.2s
 => => transferring context: 237B                                                                                                                         0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34cf6d1                                     3.7s
 => => resolve docker.io/library/nginx:alpine@sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34cf6d1                                     0.2s
 => => sha256:589002ba0eaed121a1dbf42f6648f29e5be55d5c8a6ee0f8eaa0285cc21ac153 3.86MB / 3.86MB                                                            0.6s
 => => sha256:d5030d429039a823bef4164df2fad7a0defb8d00c98c1136aec06701871197c2 12.32kB / 12.32kB                                                          0.0s
 => => sha256:e7257f1ef28ba17cf7c248cb8ccf6f0c6e0228ab9c315c152f9c203cd34cf6d1 10.33kB / 10.33kB                                                          0.0s
 => => sha256:7e89aa6cabfc80f566b1b77b981f4bb98413bd2d513ca9a30f63fe58b4af6903 2.50kB / 2.50kB                                                            0.0s
 => => sha256:91d1c9c22f2c631288354fadb2decc448ce151d7a197c167b206588e09dcd50a 626B / 626B                                                                0.9s
 => => sha256:8892f80f46a05d59a4cde3bcbb1dd26ed2441d4214870a4a7b318eaa476a0a54 1.87MB / 1.87MB                                                            0.8s
 => => extracting sha256:589002ba0eaed121a1dbf42f6648f29e5be55d5c8a6ee0f8eaa0285cc21ac153                                                                 0.1s
 => => sha256:cf1159c696ee2a72b85634360dbada071db61bceaad253db7fda65c45a58414c 953B / 953B                                                                1.1s
 => => extracting sha256:8892f80f46a05d59a4cde3bcbb1dd26ed2441d4214870a4a7b318eaa476a0a54                                                                 0.1s
 => => sha256:3f4ad4352d4f91018e2b4910b9db24c08e70192c3b75d0d6fff0120c838aa0bb 402B / 402B                                                                1.3s
 => => sha256:c2bd5ab177271dd59f19a46c214b1327f5c428cd075437ec0155ae71d0cdadc1 1.21kB / 1.21kB                                                            1.4s
 => => extracting sha256:91d1c9c22f2c631288354fadb2decc448ce151d7a197c167b206588e09dcd50a                                                                 0.0s
 => => extracting sha256:cf1159c696ee2a72b85634360dbada071db61bceaad253db7fda65c45a58414c                                                                 0.0s
 => => sha256:4d9d41f3822d171ccc5f2cdfd75ad846ac4c7ed1cd36fb998fe2c0ce4501647b 1.40kB / 1.40kB                                                            1.7s
 => => extracting sha256:3f4ad4352d4f91018e2b4910b9db24c08e70192c3b75d0d6fff0120c838aa0bb                                                                 0.0s
 => => extracting sha256:c2bd5ab177271dd59f19a46c214b1327f5c428cd075437ec0155ae71d0cdadc1                                                                 0.0s
 => => sha256:3370263bc02adcf5c4f51831d2bf1d54dbf9a6a80b0bf32c5c9b9400630eaa08 20.25MB / 20.25MB                                                          2.1s
 => => extracting sha256:4d9d41f3822d171ccc5f2cdfd75ad846ac4c7ed1cd36fb998fe2c0ce4501647b                                                                 0.0s
 => => extracting sha256:3370263bc02adcf5c4f51831d2bf1d54dbf9a6a80b0bf32c5c9b9400630eaa08                                                                 0.4s
 => [2/2] COPY app/index.html /usr/share/nginx/html/index.html                                                                                            0.3s
 => exporting to image                                                                                                                                    0.3s
 => => exporting layers                                                                                                                                   0.2s
 => => writing image sha256:c88bf25cd40be5df6da00626109fa208f40ff02697a37e6a1834579451140523                                                              0.0s
 => => naming to docker.io/library/my-web                                                                                                                 0.0s

ashofrondol9475@c3r8s7 docker-project % docker run -d -p 8080:80 --name my-nginx my-web
ca49e10a48eaae1da9b320a854a3d9d66cbcf829e570a19207e1fd2c5d4fbe48

웹브라우저에 들어가서 localhost:8080으로 접속하면 직접 만든 웹사이트에 접속할 수 있다.

## 5.2. 바인드 마운트 테스트

바인드 마운트 테스트를 하는 이유는 간단하다. 
호스트(Mac)의 파일을 수정하면 컨테이너에 즉시 반영되는 것을 확인하기 위해서이다.

바인드 마운트 없이
Mac에서 파일 수정 → 컨테이너에 반영 안 됨 → 이미지 다시 빌드해야 함

바인드 마운트 있으면
Mac에서 파일 수정 → 컨테이너에 즉시 반영 → 브라우저 새로고침만 하면 됨

따라서 실제 개발할 때 코드를 수정할 때마다 이미지를 다시 빌드하면 비효율적이기 때문에 바인드 마운트를 쓰면 실시간으로 변경사항이 반영되어 개발이 훨씬 빨라진다.
(실제 프론트엔드 작업 시 코드 업데이트 이후 변경사항을 확인하는게 불편하기 때문에 디버그 모드나 Live server같은 익스텐션을 사용하기도 한다.)

docker run -d -p 8081:80 -v $(pwd)/app:/usr/share/nginx/html --name mount-test nginx:alpine

localhost 8081 포트에 새로 nginx 도커를 실행한다.

아까와 동일한 환경으로 켜진 것을 확인할 수 있는데, 여기에서 수정을 가해 볼 것이다.

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta http-equiv="Content-Style-Type" content="text/css">
  <title></title>
  <meta name="Generator" content="Cocoa HTML Writer">
  <meta name="CocoaVersion" content="2575.7">
  <style type="text/css">
    p.p2 {margin: 0.0px 0.0px 12.0px 0.0px; font: 12.0px Times; -webkit-text-stroke: #000000}
    span.s1 {font-kerning: none}
  </style>
</head>
<body>
<h1 style="margin: 0.0px 0.0px 16.1px 0.0px; font: 24.0px Times; -webkit-text-stroke: #000000"><span class="s1"><b>Hello Bind Mount!</b></span></h1>
<p class="p2"><span class="s1">This is my custom NGINX container.</span></p>
<p class="p2"><span class="s1">With bind mount test.</span></p>
</body>
</html>

이렇게 수정하고 나서 확인해보면 잘 바뀐 것을 확인할 수 있다.

## 6. 도커 볼륨 영속성 검증

핵심은 컨테이너를 삭제해도 데이터가 살아있는지 확인하는 것이다.

1. 볼륨 생성

docker volume create my-volume
2. 컨테이너에 볼륨 연결 + 데이터 저장

docker run -v my-volume:/data --name vol-test ubuntu bash -c "echo 'persistent data' > /data/test.txt"
3. 데이터 확인 (삭제 전)

docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
→ 출력: persistent data

4. 컨테이너 삭제

docker rm vol-test
5. 새 컨테이너에서 데이터 확인 (삭제 후)

docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
→ 출력: persistent data → 스크린샷

이게 왜 중요한가?

볼륨 없이: 컨테이너 삭제 → 데이터도 삭제
볼륨 있으면: 컨테이너 삭제 → 데이터 유지
데이터베이스, 로그 파일 등 사라지면 안 되는 데이터를 보존할 때 볼륨을 사용한다. 삭제 전/후 출력이 동일한 것을 보여주면 영속성 증명이 완료된다.

ashofrondol9475@c3r8s7 docker-project % docker volume create my-volume
my-volume

ashofrondol9475@c3r8s7 docker-project % docker run -v my-volume:/data --name vol-test ubuntu bash -c "echo 'persistent data' > /data/test.txt"

위 명령어가 좀 복잡하니 주석을 달았다.

docker run \
  -v my-volume:/data \          # my-volume 볼륨을 컨테이너의 /data 경로에 연결
  --name vol-test \             # 컨테이너 이름을 vol-test로 지정
  ubuntu \                      # ubuntu 이미지 사용
  bash -c "echo 'persistent data' > /data/test.txt"  # /data/test.txt에 텍스트 저장

ashofrondol9475@c3r8s7 docker-project % docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
persistent data

persistent data라고 나오는 것을 확인할 수 있다.
볼륨 영속성을 검증했으니 삭제하고 나서 삭제를 진행해보자.

ashofrondol9475@c3r8s7 docker-project % docker rm vol-test
vol-test

잘 지웠다. 이제 영속성을 확인해보자.

ashofrondol9475@c3r8s7 docker-project % docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
persistent data

docker run \
  --rm \                        # 실행 끝나면 컨테이너 자동 삭제
  -v my-volume:/data \          # my-volume 볼륨을 컨테이너의 /data 경로에 연결
  ubuntu \                      # ubuntu 이미지 사용
  cat /data/test.txt            # /data/test.txt 파일 내용 출력

persistent data라고 나오는 것을 확인할 수 있다. 
성공이다.

## 7. Git/GitHub 연동 및 동작

나는 git을 VS Code의 소스 제어를 하기 때문에 업로드를 할 때 사용할 토큰을 처음 한 번만 CLI로 작업할 때 사용하고, 이후는 전부 VS Code를 이용해 commit과 push 를 진행했다.
다만 git config에서 리스트를 출력하면 초기에 설정을 하기도 했고, 소스 제어를 사용할 때도 user와 email을 등록해놨기 때문에 잘 출력될 것이다.

root@a0e013d547db:/codyssey_jsy# git config --list
credential.helper=!f() { /root/.vscode-server/bin/07ff9d6178ede9a1bd12ad3399074d726ebe6e43/node /tmp/vscode-remote-containers-99677bf1-ff13-4953-a4fd-2c3a7361ddf4.js git-credential-helper $*; }; f
credential.helper=!f() { /root/.vscode-server/bin/07ff9d6178ede9a1bd12ad3399074d726ebe6e43/node /tmp/vscode-remote-containers-99677bf1-ff13-4953-a4fd-2c3a7361ddf4.js git-credential-helper $*; }; f
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/JungSaeYoung/codyssey_jsy.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
branch.main.vscode-merge-base=origin/main
user.name=JungSaeYoung
user.email=이메일은 비공개합니다.

잘 나오는 것을 확인할 수 있다.

만약 이게 안나온다면 아래 명령어를 통해 미리 user.name과 user.email을 설정해야 정상적으로 git을 사용할 수 있다.

git config --global user.name "본인이름"
git config --global user.email "본인이메일@example.com"

가장 처음에 했던 내용이지만 이 과정을 진행하기 위해서 해야 하는 일을 설명하겠다.

처음에 git 저장소로 사용할 곳으로 mkdir을 통해 폴더를 만들고, git init을 해준다.
이후에 아래 과정을 진행하면 된다.

1. 깃 상태 확인
cd ~/docker-project
git status

2. 파일 전체 추가
git add .

3. 커밋
git commit -m "커밋 메시지"
ex) git commit -m "Docker 워크스테이션 구축 과제 완료"

4. 푸시
git push -u origin main

이때 인자값으로 설정한 -u는 업스트림 설정인데, 처음에 이렇게 실행하면 이후에 git push를 하면 이때 설정한대로 git push origin main으로 명령어가 동작하게 된다.


## 8. 트러블슈팅

### 트러블슈팅 1: OrbStack GUI에서 Docker 컨테이너 생성 실패

**문제**
OrbStack 애플리케이션에서 Docker 생성 및 실행 버튼을 눌렀을 때 "failed to create" 에러가 발생했다.

**원인 가설**
- OrbStack 설치 직후 내부 초기화가 완료되지 않았을 가능성
- macOS 보안 정책(및 서울 캠퍼스 보안 상)으로 인한 권한 미허용

**확인**
OrbStack 설정 및 macOS 시스템 설정의 개인정보 보호 및 보안 항목을 확인했다.

**해결**
GUI 대신 터미널에서 직접 Docker 명령어를 사용하는 방식으로 전환했다.
OrbStack이 실행된 상태에서 터미널에 docker 명령어를 입력하면 정상적으로 동작했다.

docker pull ubuntu:latest
docker run -it ubuntu bash

GUI에 의존하지 않고 CLI로 작업하는 것이 더 안정적이었다.

---

### 트러블슈팅 2: 컨테이너 내부에서 docker build 실행 시 소켓 에러

**문제**
Ubuntu 컨테이너 안에서 docker build를 실행했을 때 아래 에러가 발생했다.

ERROR: failed to connect to the docker API at unix:///var/run/docker.sock; check if the path is correct and if the daemon is running: dial unix /var/run/docker.sock: connect: no such file or directory

**원인 가설**
- 컨테이너 내부에 Docker 엔진이 설치되어 있지 않을 가능성
- 일반 컨테이너에서는 Docker 데몬에 접근할 수 없는 구조일 가능성

**확인**
일반적인 Docker 컨테이너는 격리된 환경이기 때문에 내부에 Docker 데몬이 존재하지 않는다.
컨테이너 안에서 Docker를 사용하려면 Docker in Docker(DinD)라는 전용 이미지(docker:dind)와 --privileged 옵션이 필요하다.

**해결**
Docker 관련 명령어(build, run, ps 등)는 Mac 터미널에서 실행하고, 컨테이너 내부에서는 리눅스 실습(ls, chmod 등)만 수행하는 것으로 작업 방식을 분리했다.

- Mac 터미널: docker build, docker run 등 Docker 명령 실행
- 컨테이너 내부: 리눅스 명령어 실습 (터미널 조작, 권한 변경 등)

---

### 트러블슈팅 3: apt install git 패키지 찾기 실패

**문제**
Ubuntu 컨테이너에서 git을 설치하려고 했을 때 아래 에러가 발생했다.

E: Unable to locate package git

**원인 가설**
- 패키지 목록이 업데이트되지 않아 패키지를 찾지 못하는 것으로 추정

**확인**
Docker의 Ubuntu 이미지는 용량을 줄이기 위해 패키지 목록이 포함되지 않은 최소 상태로 배포된다.
따라서 apt update를 먼저 실행하여 패키지 목록을 다운로드해야 한다.

**해결**

apt update && apt install -y git

apt update를 먼저 실행한 뒤 설치하니 정상적으로 git이 설치되었다.
Docker 컨테이너에서 패키지를 설치할 때는 항상 apt update를 먼저 실행해야 한다는 것을 알게 되었다.

## 9.1. 보너스 문제

1. docker-compose.yml 작성

cd ~/Desktop/docker-project
vim docker-compose.yml

version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./app:/usr/share/nginx/html
2. 기존 컨테이너 정리 (포트 충돌 방지)

docker stop my-nginx mount-test
docker rm my-nginx mount-test
3. 실행

docker compose up -d
4. 확인

docker compose ps
→ 브라우저에서 http://localhost:8080 접속 → 스크린샷

5. 종료

docker compose down
핵심 포인트: docker run 명령어를 매번 치는 대신, yml 파일에 설정을 저장해두고 docker compose up 한 줄로 실행할 수 있다.

## 9.2. 보너스 문제

1. docker-compose.yml 수정

vim docker-compose.yml

version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./app:/usr/share/nginx/html
    depends_on:
      - api

  api:
    image: nginx:alpine
    ports:
      - "8081:80"
항목	설명
web	아까 만든 커스텀 nginx (8080 포트)
api	보조 서비스 nginx (8081 포트)
depends_on	web은 api가 먼저 실행된 후 시작
2. 실행

docker compose up -d
3. 두 서비스 모두 실행 확인

docker compose ps
→ 브라우저에서 localhost:8080, localhost:8081 둘 다 접속 → 스크린샷

4. 컨테이너 간 통신 확인

docker compose exec web sh -c "apk add curl && curl http://api:80"
→ api 컨테이너의 nginx 응답이 오면 통신 성공 → 스크린샷

핵심 포인트: 같은 docker-compose.yml 안의 서비스끼리는 서비스 이름(api, web)으로 서로 통신할 수 있다. IP 주소를 몰라도 된다.

5. 종료

docker compose down