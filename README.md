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

- Docker Compose 단일 서비스 실행
- Docker Compose 멀티 컨테이너 (웹서버 + 보조서비스) + 컨테이너 간 통신 확인

---

## 환경 특이사항

Mac + OrbStack 사용 (sudo 없이 Docker 실행 가능)

---

## 세부 실행 환경

**1. OS 환경**

```
ashofrondol9475@c3r8s7 ~ % sw_vers
ProductName:    macOS
ProductVersion: 15.7.4
BuildVersion:   24G517
```

**2. 쉘 종류**

```
ashofrondol9475@c3r8s7 ~ % echo $SHELL
/bin/zsh
```

**3. 도커 버전**

```
ashofrondol9475@c3r8s7 ~ % docker --version
Docker version 28.5.2, build ecc6942
```

**4. 컨테이너 OS 정보**

```
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
```

**5. 컨테이너 커널 정보**

```
root@a0e013d547db:/# uname -a
Linux a0e013d547db 6.17.8-orbstack-00308-g8f9c941121b1 #1 SMP PREEMPT Thu Nov 20 09:34:02 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

**6. 컨테이너 쉘 종류**

```
root@a0e013d547db:/# echo $SHELL
/bin/bash
```

**7. Git 버전**

```
root@a0e013d547db:/# git --version
git version 2.43.0
```

---

## 환경 설정 방법

Mac 환경에 설치되어 있는 OrbStack으로 CLI 없이 환경 설정을 진행하려 했으나, GUI에서 컨테이너를 여는 것이 불가능했다.
따라서 OrbStack과 함께 설치되어 있는 Docker에서 `ubuntu:latest` 이미지를 pull 하여 Terminal로 열었다.
따로 이미지를 설치하지 않으면 자동으로 Alpine Linux로 컨테이너가 열린다.

컨테이너 내에서 git 패키지 설치는 아래 명령어로 진행한다.
`apt update`를 하지 않으면 패키지 목록을 찾지 못하기 때문에 꼭 먼저 실행해야 한다.

```bash
apt update && apt install -y git
```

Git의 경우 VS Code의 Source Control 기능을 통해 commit 및 push를 진행한다.

CLI를 통해 commit 및 push 작업을 진행할 경우, `git config`로 사용자명과 이메일을 입력해야 하고, push할 때마다 GitHub에서 발급받은 토큰을 password로 입력해야 한다.

추가적으로, VS Code에서 Dev Container 익스텐션을 설치해 VS Code로 컨테이너에 연결함으로써 최종 환경을 구성했다.

---

## 1. 터미널 조작 로그

> 해당 과제 수행 이후 다른 컨테이너로 변경하게 되어 실행 환경이 달라졌다. 때문에 ID가 다를 수 있다.

**pwd** — 현재 디렉터리 위치 확인

```
root@da9ce8573016 /codyssey_jsy main* ❯ pwd
/codyssey_jsy
```

현재 잡고 있는 디렉터리 위치를 알려주는 명령어다.

---

**ls -la** — 디렉터리 내용 상세 출력

```
root@da9ce8573016 /codyssey_jsy main* ❯ ls -la
drwxr-xr-x root root  26 B  Mon Mar 30 09:10:42 2026 .
drwxr-xr-x root root  68 B  Mon Mar 30 08:57:42 2026 ..
drwxr-xr-x root root 138 B  Mon Mar 30 09:13:17 2026 .git
.rw-r--r-- root root 1.4 KB Mon Mar 30 09:10:35 2026 README.md
```

`ls`는 디렉터리를 따로 지정하지 않으면 현재 위치의 파일과 폴더 목록을 반환한다.

- `-l` : 파일의 상세 정보를 표시한다. `d`로 시작하면 디렉터리, 그렇지 않으면 파일이다.
- `-a` : 숨김 파일을 포함하여 반환한다.

> Ubuntu에서는 자주 쓰는 `ls -l`을 `ll`로 축약해서 사용할 수도 있다.

---

**mkdir** — 디렉터리 생성

```
root@da9ce8573016 /codyssey_jsy main* ❯ mkdir test-dir
```

MaKe DIRectory의 약어로, 폴더를 생성하는 명령어다.

---

**touch** — 파일 생성

```
root@da9ce8573016 /codyssey_jsy main* ❯ touch test-file.txt
```

파일을 생성하는 명령어다. `.`을 기준으로 파일명과 확장자를 구분한다.

---

**cp** — 파일 복사

```
root@da9ce8573016 /codyssey_jsy main* ❯ cp test-file.txt
cp: missing destination file operand after 'test-file.txt'
Try 'cp --help' for more information.
```

CoPy의 약어로, 복사 대상과 복사 결과 파일명을 모두 지정해야 한다. 위는 결과 파일명을 지정하지 않아 오류가 발생한 경우다.

```
root@da9ce8573016 /codyssey_jsy main* ❯ ls
README.md  test-dir  test-file.txt

root@da9ce8573016 /codyssey_jsy main* ❯ cp test-file.txt copy-file.txt
```

`cp <복사 대상> <복사 결과 파일명>` 형식으로 올바르게 사용했다.

---

**mv** — 파일 이동 / 이름 변경

```
root@da9ce8573016 /codyssey_jsy main* ❯ mv copy-file.txt renamed.txt

root@da9ce8573016 /codyssey_jsy main* ❯ ls
README.md  renamed.txt  test-dir  test-file.txt
```

MoVe의 약어로, 파일 이동 외에도 `mv <대상 파일> <변경될 이름>` 형식으로 이름 변경에도 사용할 수 있다.

---

**cat** — 파일 내용 출력

```
root@da9ce8573016 /codyssey_jsy main* ❯ cat renamed.txt
```

ConcAtenaTe의 약어다. 파일 내용이 비어 있어 아무것도 출력되지 않았다.

이후 vim 에디터로 `test`라는 내용을 작성한 뒤 다시 확인했다.

```bash
apt install vim
```

```
root@da9ce8573016 /codyssey_jsy main* ❯ cat renamed.txt
test
```

---

**rm** — 파일 삭제

```
root@da9ce8573016 /codyssey_jsy main* ❯ rm renamed.txt

root@da9ce8573016 /codyssey_jsy main* ❯ ls
README.md  test-dir  test-file.txt
```

ReMove의 약어로, 파일 또는 폴더를 삭제하는 명령어다.

---

## 2. 권한 실습

**파일 생성 및 권한 확인**

```
root@a0e013d547db:/codyssey_jsy# touch myfile.txt

root@a0e013d547db:/codyssey_jsy# ls -l myfile.txt
-rw-r--r-- 1 root root 0 Mar 31 09:04 myfile.txt
```

`ls -l <파일명>`으로 세부 권한 정보를 확인할 수 있다.

권한 표기 `-rw-r--r--`의 의미는 다음과 같다.

| 위치 | 의미 |
|------|------|
| `r` | 읽기 권한 (read) |
| `w` | 쓰기 권한 (write) |
| `x` | 실행 권한 (execute) |

권한은 세 그룹으로 나뉜다: **소유자 / 그룹 사용자 / 기타 사용자**

따라서 `-rw-r--r--`는 소유자(root)는 읽기·쓰기 가능, 그룹 및 기타 사용자는 읽기만 가능하다는 의미다.

---

**chmod로 파일 권한 변경**

```
root@a0e013d547db:/codyssey_jsy# chmod 755 myfile.txt

root@a0e013d547db:/codyssey_jsy# ls -l myfile.txt
-rwxr-xr-x 1 root root 0 Mar 31 09:04 myfile.txt
```

CHange MODe의 약어로, 숫자로 권한을 지정한다.

| 숫자 | 권한 |
|------|------|
| 4 | r (읽기) |
| 2 | w (쓰기) |
| 1 | x (실행) |

숫자를 합산하여 사용한다. 순서는 소유자 / 그룹 / 기타 사용자 순이다.

---

**디렉터리 권한 변경**

```
root@a0e013d547db:/codyssey_jsy# mkdir mydir

root@a0e013d547db:/codyssey_jsy# ls -ld mydir
drwxr-xr-x 1 root root 0 Mar 31 09:05 mydir
```

디렉터리는 `d`로 시작한다 (directory의 약자).

```
root@a0e013d547db:/codyssey_jsy# chmod 700 mydir

root@a0e013d547db:/codyssey_jsy# ls -ld mydir
drwx------ 1 root root 0 Mar 31 09:05 mydir
```

의도한 대로 소유자만 모든 권한을 가지도록 변경됐다.

---

## 3. Docker 설치 점검

OrbStack이 켜진 상태에서 아래 명령어로 Docker 상태를 점검한다.

```
ashofrondol9475@c3r8s7 ~ % docker --version
Docker version 28.5.2, build ecc6942
```

```
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
 ...

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```

맨 아래 WARNING은 Docker가 기본적으로 사용하는 `iptables` 리눅스 방화벽 기능이 비활성화되어 있어서 발생한다.
컨테이너에 중요한 정보가 없고 외부망에 노출된 환경도 아니므로 무시했다.

---

## 4. Docker 기본 운영

**이미지 목록 확인**

```
ashofrondol9475@c3r8s7 ~ % docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    f794f40ddfff   5 weeks ago   78.1MB
```

**실행 중인 컨테이너 확인**

```
ashofrondol9475@c3r8s7 ~ % docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED       STATUS       PORTS     NAMES
a0e013d547db   ubuntu    "bash"    5 hours ago   Up 5 hours             strange_haibt
```

컨테이너 ID, 이미지, 커맨드 타입, 생성 시각, 상태, 포트, 이름을 확인할 수 있다.

**로그 확인**

```bash
docker logs <컨테이너명>          # 전체 로그 출력
docker logs -f <컨테이너명>       # 실시간 로그 확인 (Ctrl+C로 종료)
docker logs --tail 10 <컨테이너명> # 마지막 10줄만 출력
```

**리소스 사용량 확인**

```bash
docker stats           # 실시간 자원 사용량 (Ctrl+C로 종료)
docker stats --no-stream  # 현재 시점 값만 출력
```

```
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O        PIDS
a0e013d547db   strange_haibt   525.40%   9.395GiB / 15.67GiB   59.94%    73.3MB / 544kB   4.56GB / 643MB   124
```

---

## 5.1. 커스텀 Dockerfile (NGINX)

**작업 폴더 준비 및 HTML 파일 작성**

```
root@a0e013d547db:/codyssey_jsy# mkdir -p ~/docker-project/app
root@a0e013d547db:/codyssey_jsy# cd ~/docker-project
root@a0e013d547db:~/docker-project# vim app/index.html
```

```html
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
```

```
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
```

**Dockerfile 작성**

```
root@a0e013d547db:~/docker-project# vim Dockerfile
```

```dockerfile
FROM nginx:alpine
COPY app/index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

| 명령어 | 역할 |
|--------|------|
| `FROM nginx:alpine` | nginx 웹 서버가 설치된 Alpine 이미지를 베이스로 사용 |
| `COPY app/index.html ...` | 직접 만든 HTML을 nginx 기본 웹 경로에 복사 |
| `EXPOSE 80` | 컨테이너가 80번 포트를 사용함을 명시 |

> 도커 안에서 도커를 여는 것(DinD, Docker in Docker)은 일반적으로 불가능하고 전용 이미지와 `--privileged` 옵션이 필요하다. 따라서 `docker-project` 폴더를 컨테이너 바깥(Mac 터미널)으로 옮긴 뒤 이미지를 빌드한다.

**이미지 빌드 및 컨테이너 실행**

```
ashofrondol9475@c3r8s7 ~ % cd Desktop/docker-project
ashofrondol9475@c3r8s7 docker-project % ls
Dockerfile  app
```

```
ashofrondol9475@c3r8s7 docker-project % docker build -t my-web .
[+] Building 7.9s (7/7) FINISHED                                docker:orbstack
 => [internal] load build definition from Dockerfile                      0.2s
 => [internal] load metadata for docker.io/library/nginx:alpine           2.9s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:e7257f...            3.7s
 => [2/2] COPY app/index.html /usr/share/nginx/html/index.html            0.3s
 => exporting to image                                                    0.3s
 => => naming to docker.io/library/my-web                                 0.0s
```

```
ashofrondol9475@c3r8s7 docker-project % docker run -d -p 8080:80 --name my-nginx my-web
ca49e10a48eaae1da9b320a854a3d9d66cbcf829e570a19207e1fd2c5d4fbe48
```

웹브라우저에서 `http://localhost:8080`으로 접속하면 직접 만든 웹사이트를 확인할 수 있다.

---

## 5.2. 바인드 마운트 테스트

바인드 마운트를 사용하는 이유는 호스트(Mac)의 파일을 수정하면 컨테이너에 즉시 반영되는지 확인하기 위해서다.

| 상황 | 동작 |
|------|------|
| 바인드 마운트 없이 | Mac에서 파일 수정 → 컨테이너에 반영 안 됨 → 이미지 다시 빌드 필요 |
| 바인드 마운트 있으면 | Mac에서 파일 수정 → 컨테이너에 즉시 반영 → 브라우저 새로고침만으로 확인 |

실제 개발 시 코드 수정마다 이미지를 재빌드하는 건 비효율적이므로, 바인드 마운트를 활용하면 개발 속도가 크게 향상된다. (프론트엔드에서 Live Server 같은 익스텐션을 사용하는 것과 유사한 원리)

```bash
docker run -d -p 8081:80 -v $(pwd)/app:/usr/share/nginx/html --name mount-test nginx:alpine
```

`localhost:8081`에 새로 nginx 컨테이너를 실행한 뒤, `app/index.html`을 수정하면 브라우저 새로고침만으로 변경사항이 반영되는 것을 확인할 수 있다.

---

## 6. Docker 볼륨 영속성 검증

컨테이너를 삭제해도 데이터가 유지되는지 확인하는 과정이다.

**1. 볼륨 생성**

```
ashofrondol9475@c3r8s7 docker-project % docker volume create my-volume
my-volume
```

**2. 컨테이너에 볼륨 연결 후 데이터 저장**

```bash
docker run \
  -v my-volume:/data \         # my-volume 볼륨을 컨테이너의 /data 경로에 연결
  --name vol-test \            # 컨테이너 이름을 vol-test로 지정
  ubuntu \                     # ubuntu 이미지 사용
  bash -c "echo 'persistent data' > /data/test.txt"  # /data/test.txt에 텍스트 저장
```

**3. 데이터 확인 (삭제 전)**

```
ashofrondol9475@c3r8s7 docker-project % docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
persistent data
```

**4. 컨테이너 삭제**

```
ashofrondol9475@c3r8s7 docker-project % docker rm vol-test
vol-test
```

**5. 새 컨테이너에서 데이터 확인 (삭제 후)**

```
ashofrondol9475@c3r8s7 docker-project % docker run --rm -v my-volume:/data ubuntu cat /data/test.txt
persistent data
```

삭제 전후 출력이 동일하므로 영속성 증명 완료.

| 상황 | 결과 |
|------|------|
| 볼륨 없이 | 컨테이너 삭제 → 데이터도 함께 삭제 |
| 볼륨 있으면 | 컨테이너 삭제 → 데이터 유지 |

데이터베이스, 로그 파일 등 사라지면 안 되는 데이터를 보존할 때 볼륨을 사용한다.

---

## 7. Git/GitHub 연동 및 동작

VS Code의 소스 제어 기능을 통해 commit 및 push를 진행했다.
최초 연동 시에만 CLI로 토큰을 입력하고, 이후에는 VS Code를 통해 작업했다.

**git config 확인**

```
root@a0e013d547db:/codyssey_jsy# git config --list
credential.helper=...
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/JungSaeYoung/codyssey_jsy.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
user.name=JungSaeYoung
user.email=*****@*****.***
```

**초기 설정 (미설정 시 필요)**

```bash
git config --global user.name "본인이름"
git config --global user.email "본인이메일@example.com"
```

**CLI를 통한 push 전체 과정**

```bash
# 1. git 저장소 초기화
mkdir <프로젝트폴더> && cd <프로젝트폴더>
git init

# 2. 파일 전체 스테이징
git add .

# 3. 커밋
git commit -m "Docker 워크스테이션 구축 과제 완료"

# 4. 푸시 (최초 1회)
git push -u origin main
```

> `-u` 옵션은 업스트림을 설정하며, 이후에는 `git push`만 입력해도 동일하게 동작한다.

---

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
OrbStack이 실행된 상태에서 터미널에 명령어를 입력하면 정상적으로 동작했다.

```bash
docker pull ubuntu:latest
docker run -it ubuntu bash
```

GUI에 의존하지 않고 CLI로 작업하는 것이 더 안정적이었다.

---

### 트러블슈팅 2: 컨테이너 내부에서 docker build 실행 시 소켓 에러

**문제**

Ubuntu 컨테이너 안에서 `docker build`를 실행했을 때 아래 에러가 발생했다.

```
ERROR: failed to connect to the docker API at unix:///var/run/docker.sock;
check if the path is correct and if the daemon is running:
dial unix /var/run/docker.sock: connect: no such file or directory
```

**원인 가설**

- 컨테이너 내부에 Docker 엔진이 설치되어 있지 않을 가능성
- 일반 컨테이너에서는 Docker 데몬에 접근할 수 없는 구조일 가능성

**확인**

일반적인 Docker 컨테이너는 격리된 환경이기 때문에 내부에 Docker 데몬이 존재하지 않는다.
컨테이너 안에서 Docker를 사용하려면 Docker in Docker(DinD) 전용 이미지(`docker:dind`)와 `--privileged` 옵션이 필요하다.

**해결**

작업을 다음과 같이 분리했다.

| 환경 | 작업 |
|------|------|
| Mac 터미널 | `docker build`, `docker run` 등 Docker 명령 실행 |
| 컨테이너 내부 | `ls`, `chmod` 등 리눅스 명령어 실습 |

---

### 트러블슈팅 3: apt install git 패키지 찾기 실패

**문제**

Ubuntu 컨테이너에서 git을 설치하려고 했을 때 아래 에러가 발생했다.

```
E: Unable to locate package git
```

**원인 가설**

패키지 목록이 업데이트되지 않아 패키지를 찾지 못하는 것으로 추정했다.

**확인**

Docker의 Ubuntu 이미지는 용량을 줄이기 위해 패키지 목록이 포함되지 않은 최소 상태로 배포된다.
따라서 `apt update`를 먼저 실행하여 패키지 목록을 다운로드해야 한다.

**해결**

```bash
apt update && apt install -y git
```

`apt update`를 먼저 실행한 뒤 설치하니 정상적으로 git이 설치되었다.
Docker 컨테이너에서 패키지를 설치할 때는 항상 `apt update`를 먼저 실행해야 한다.

---

## 9.1. 보너스: Docker Compose 단일 서비스

`docker run`은 컨테이너 하나를 실행하는 데 사용되지만, 여러 컨테이너를 종료했다가 다시 시작할 때 매번 명령어를 다시 입력해야 하는 불편함이 있다.
`docker-compose.yaml`을 사용하면 이를 한 번에 관리할 수 있다.

**1. docker-compose.yaml 작성**

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./app:/usr/share/nginx/html
```

**2. 기존 컨테이너 정리 (포트 충돌 방지)**

```bash
docker stop my-nginx mount-test
docker rm my-nginx mount-test
```

**3. 실행**

```bash
docker compose up -d   # 백그라운드 실행 (터미널 자유롭게 사용 가능)
docker compose up      # 터미널이 로그에 묶임 (Ctrl+C로 종료)
```

```
ashofrondol9475@c3r8s7 docker-project % docker compose up -d
WARN[0000] the attribute `version` is obsolete, it will be ignored
[+] Running 3/3
 ✔ docker-project-web              Built
 ✔ Network docker-project_default  Created
 ✔ Container docker-project-web-1  Started
```

**4. 확인**

```
ashofrondol9475@c3r8s7 docker-project % docker compose ps
NAME                   IMAGE                COMMAND                  SERVICE   CREATED         STATUS         PORTS
docker-project-web-1   docker-project-web   "/docker-entrypoint.…"   web       2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp
```

> `version` 관련 경고가 표시되지만 최신 Docker Compose에서 해당 필드가 더 이상 필요하지 않아서 발생하는 것이며, 동작에는 영향이 없다.

**5. 종료**

```
ashofrondol9475@c3r8s7 docker-project % docker compose down
[+] Running 2/2
 ✔ Container docker-project-web-1  Removed
 ✔ Network docker-project_default  Removed
```

---

## 9.2. 보너스: Docker Compose 멀티 컨테이너

**1. docker-compose.yaml 수정**

```yaml
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
```

| 서비스 | 설명 |
|--------|------|
| `web` | 커스텀 nginx (8080 포트) |
| `api` | 보조 서비스 nginx (8081 포트) |
| `depends_on` | `web`은 `api`가 먼저 실행된 후 시작 |

**2. 실행**

```
ashofrondol9475@c3r8s7 docker-project % docker compose up -d
[+] Running 3/3
 ✔ Network docker-project_default  Created
 ✔ Container docker-project-api-1  Started
 ✔ Container docker-project-web-1  Started
```

**3. 두 서비스 모두 실행 확인**

```
ashofrondol9475@c3r8s7 docker-project % docker compose ps
NAME                   IMAGE                COMMAND                  SERVICE   CREATED          STATUS          PORTS
docker-project-api-1   nginx:alpine         "/docker-entrypoint.…"   api       34 seconds ago   Up 33 seconds   0.0.0.0:8081->80/tcp
docker-project-web-1   docker-project-web   "/docker-entrypoint.…"   web       34 seconds ago   Up 33 seconds   0.0.0.0:8080->80/tcp
```

브라우저에서 `http://localhost:8080`과 `http://localhost:8081` 모두 정상 접속을 확인했다.

**4. 컨테이너 간 통신 확인**

```
ashofrondol9475@c3r8s7 docker-project % docker compose exec web sh -c "apk add curl && curl http://api:80"
OK: 58.9 MiB in 72 packages
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
</html>
```

`api` 컨테이너의 nginx 응답이 정상적으로 수신되었다.

> 같은 `docker-compose.yaml` 안의 서비스끼리는 서비스 이름(`api`, `web`)으로 직접 통신할 수 있다. IP 주소를 알 필요가 없다.

**5. 종료**

```
ashofrondol9475@c3r8s7 docker-project % docker compose down
[+] Running 3/3
 ✔ Container docker-project-web-1  Removed
 ✔ Container docker-project-api-1  Removed
 ✔ Network docker-project_default  Removed
```
