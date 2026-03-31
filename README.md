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
| 5 | 커스텀 Dockerfile | (A) nginx 등 웹서버 베이스 또는 (B) ubuntu/alpine 베이스로 커스텀 이미지 빌드 |
| 6 | 포트 매핑 + 볼륨 | -p 포트 매핑 접속 증거 + Docker 볼륨 영속성 증명 (컨테이너 삭제 후 데이터 유지) |
| 7 | Git/GitHub 연동 | git config 설정 + GitHub 저장소 연동 증거 |

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

## 5. 커스텀 Dockerfile (NGINX)
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

