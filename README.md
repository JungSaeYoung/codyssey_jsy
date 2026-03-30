# codyssey_jsy
## 미션 요약: 개발 워크스테이션 구축
목표
터미널 + Docker + Git/GitHub를 직접 세팅하고, 재현 가능한 개발 환경을 만드는 경험

제출물
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

README.md 필수 포함 사항
프로젝트 개요 / 실행 환경 / 수행 체크리스트 / 검증 방법
트러블슈팅 2건 이상 (문제 → 원인 → 해결)
민감정보 마스킹
보너스
Docker Compose 단일 서비스 실행
Docker Compose 멀티 컨테이너 (웹서버 + 보조서비스) + 컨테이너 간 통신 확인
환경 특이사항
Mac + OrbStack 사용 (sudo 없이 Docker 실행 가능

## 1. 터미널 조작 로그

root@da9ce8573016 /codyssey_jsy main*
❯ pwd
/codyssey_jsy

root@da9ce8573016 /codyssey_jsy main*
❯ ls -la
drwxr-xr-x root root  26 B  Mon Mar 30 09:10:42 2026 .
drwxr-xr-x root root  68 B  Mon Mar 30 08:57:42 2026 ..
drwxr-xr-x root root 138 B  Mon Mar 30 09:13:17 2026 .git
.rw-r--r-- root root 1.4 KB Mon Mar 30 09:10:35 2026 README.md

root@da9ce8573016 /codyssey_jsy main*
❯ mkdir test-dir

root@da9ce8573016 /codyssey_jsy main*
❯ touch test-file.txt

root@da9ce8573016 /codyssey_jsy main*
❯ cp test-file.txt
cp: missing destination file operand after 'test-file.txt'
Try 'cp --help' for more information.

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  test-dir  test-file.txt

root@da9ce8573016 /codyssey_jsy main*
❯ cp test-file.txt copy-file.txt

root@da9ce8573016 /codyssey_jsy main*
❯ mv copy-file.txt renamed.txt

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  renamed.txt  test-dir  test-file.txt

root@da9ce8573016 /codyssey_jsy main*
❯ cat renamed.txt

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  renamed.txt  test-dir  test-file.txt

root@da9ce8573016 /codyssey_jsy main*
❯ vim renamed.txt

root@da9ce8573016 /codyssey_jsy main* 14s
❯ cat renamed.txt
test

root@da9ce8573016 /codyssey_jsy main*
❯ rm renamed.txt

root@da9ce8573016 /codyssey_jsy main*
❯ ls
README.md  test-dir  test-file.txt

## 2. 권한 실습
