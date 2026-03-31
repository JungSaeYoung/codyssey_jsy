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
따로 이미지를 설치하지 않으면 Alpine Linux로 컨테이너가 열린다.

git 패키지 설치는 

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

의도한대로 잘 수정 된 것을 확인할 수 있다.ㄴ