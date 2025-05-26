### 리눅스 CLI 간략 가이드 요약 (김현송)
- **Shell? **
	- Shell을 GUI 애플리케이션으로 만든다면 일반인이 생각하는 OS
	- 리눅스 환경  일반적으로 default shell = bash, 운영체제 마다 다름
- **man**
	- man man
	- Linux/Unix 환경에서  Helper
	- 경량 이미지 컨테이너에는 설치되지어 있지 않을 수 있어 설치 필요함 man-db 등..
- apt-get, apt, brew(MacOS), scoop(Windows) **패키지 매니저**
	- 필요한 명령어, 프로그램을 설치할 수 있는 CLI
	- 이를 통해 개발 환경에 필요한 도구를 설치, 관리할 수 있음
- **PATH**
	- env | grep PATH
	- execve
- **Shell Profile?**
	- Login Shell
		- 터미널에 처음 로그인하거나 ssh, su - 등으로 들어올 때 실행
		- 실행 순서: /etc/profile → ~/.bash_profile → ~/.bash_login → ~/.profile 중 첫 번째 존재하는 파일
	- Logout Shell
		- login shell 종료 시 실행
		- ~/.bash_logout 파일에 정의된 내용 실행됨
		- 예: 터미널 종료 시 로그 남기기, 정리 작업 등
	- **.bashrc**
		- interactive non-login shell에서 실행 (예: 터미널 새 창 열기)
		- 프롬프트 설정, alias, 함수 등 자주 쓰는 셸 설정 정
		- 보통 .bash_profile에서 .bashrc를 호출해서 설정 공유
- **Shell Operators**
	- <, >, |, <<, >>
- **tldr**
	- man보다 실용적인 CLI Helper
	- [https://tldr.sh/](https://tldr.sh/)
- **Readline Signal**
	- <C-c> interrupt
	- <C-z> STOP
	- <C-D> EOF
- ---
- # 리눅스 CLI 완전 가이드
- ## 1. Shell이란?
- ### Shell의 개념
- **Shell = 명령어 해석기**: 사용자와 운영체제 사이의 인터페이스
- GUI 애플리케이션으로 만든다면 → 일반인이 생각하는 "바탕화면 + 파일탐색기"
- **리눅스 기본 Shell**: `bash` (Bourne Again Shell)
- ### 주요 Shell 종류
  
  ```
  # 현재 사용 중인 Shell 확인
  echo $SHELL
  
  # 사용 가능한 Shell 목록
  cat /etc/shells
  ```
  
  **Shell 종류들:**
- `bash` - 가장 일반적, 리눅스 기본
- `zsh` - macOS 기본, 확장 기능 풍부
- `fish` - 사용자 친화적, 자동완성 강력
- `sh` - 최소한의 기본 Shell
  
  ---
- ## 2. man - 매뉴얼 시스템
- ### 기본 사용법
  
  ```
  # man 자체에 대한 매뉴얼
  man man
  
  # 명령어 매뉴얼 보기
  man ls
  man grep
  man find
  
  # 매뉴얼 섹션 지정
  man 1 printf  # 명령어
  man 3 printf  # C 라이브러리 함수
  ```
- ### man 페이지 내 네비게이션
- `Space` - 다음 페이지
- `b` - 이전 페이지
- `/검색어` - 텍스트 검색
- `q` - 종료
- ### 경량 컨테이너에서 설치
  
  ```
  # Ubuntu/Debian
  apt-get update && apt-get install -y man-db
  
  # Alpine Linux
  apk add man-db man-pages
  ```
  
  ---
- ## 3. 패키지 매니저
- ### Linux 배포판별 패키지 매니저
- #### Ubuntu/Debian 계열
  
  ```
  # apt vs apt-get 차이점
  apt install package     # 더 사용자 친화적, 진행률 표시
  apt-get install package # 스크립트에서 권장, 안정적
  
  # 일반적인 명령어들
  apt update              # 패키지 목록 업데이트
  apt upgrade             # 설치된 패키지 업그레이드
  apt search nodejs       # 패키지 검색
  apt show nodejs         # 패키지 정보 확인
  ```
- #### RedHat/CentOS 계열
  
  ```
  yum install package     # 구버전
  dnf install package     # 신버전
  ```
- #### macOS
  
  ```
  # Homebrew 설치 후
  brew install package
  brew search package
  brew info package
  ```
- #### Windows
  
  ```
  # Scoop 설치 후
  scoop install package
  scoop search package
  ```
- ### 개발 환경 구축 예제
  
  ```
  # Node.js 개발 환경
  apt install nodejs npm
  brew install node
  
  # Python 개발 환경  
  apt install python3 python3-pip
  brew install python3
  
  # Docker
  apt install docker.io
  brew install docker
  ```
  
  ---
- ## 4. PATH 환경변수
- ### PATH의 역할
- Shell이 명령어를 찾는 디렉토리 목록
- `execve` 시스템 콜이 실행 파일을 찾을 때 사용
- ### PATH 확인 및 관리
  
  ```
  # PATH 확인 방법들
  echo $PATH
  env | grep PATH
  printenv PATH
  
  # PATH에 디렉토리 추가
  export PATH=$PATH:/new/directory
  export PATH="/usr/local/bin:$PATH"  # 앞에 추가 (우선순위 높음)
  
  # 영구적으로 PATH 수정
  echo 'export PATH=$PATH:/new/directory' >> ~/.bashrc
  source ~/.bashrc
  ```
- ### 명령어 실행 과정
  
  ```
  # 명령어 위치 찾기
  which python3
  whereis python3
  type python3
  
  # PATH 없이 직접 실행
  /usr/bin/python3 script.py
  ./my_program  # 현재 디렉토리의 실행파일
  ```
  
  ---
- ## 5. Shell Operators (입출력 리디렉션)
- ### 기본 리디렉션
  
  ```
  # 출력 리디렉션
  command > file.txt          # 덮어쓰기
  command >> file.txt         # 추가
  command 2> error.log        # 에러만 리디렉션
  command &> all.log          # 표준출력 + 에러 모두
  
  # 입력 리디렉션
  command < input.txt
  mysql database < schema.sql
  ```
- ### 파이프 (|)
  
  ```
  # 명령어 연결
  ps aux | grep python
  cat file.txt | grep "error" | wc -l
  ls -la | sort -k5 -n | tail -10
  
  # 복잡한 파이프라인 예제
  cat access.log | grep "404" | awk '{print $1}' | sort | uniq -c | sort -nr
  ```
- ### Here Document (<<)
  
  ```
  # 여러 줄 입력
  cat << EOF > config.txt
  server=localhost
  port=3306
  database=mydb
  EOF
  
  # 변수 치환 방지
  cat << 'EOF'
  $HOME will not be expanded
  EOF
  ```
- ### 실용적인 예제들
  
  ```
  # 로그 분석
  tail -f /var/log/nginx/access.log | grep "POST"
  
  # 백업과 압축
  tar czf backup.tar.gz /important/data 2> backup.log
  
  # 데이터 처리 파이프라인
  curl -s api.json | jq '.results[]' | grep -i "error"
  ```
  
  ---
- ## 6. tldr - 간단한 도움말
- ### tldr이란?
- **Too Long; Didn't Read**의 줄임말
- man 페이지보다 간단하고 실용적인 예제 제공
- 커뮤니티 기반으로 관리되는 도움말
- ### 설치 및 사용법
  
  ```
  # 설치 방법들
  npm install -g tldr
  pip install tldr
  brew install tldr
  
  # 사용 예제
  tldr ls
  tldr find
  tldr tar
  tldr curl
  
  # 페이지 업데이트
  tldr --update
  ```
- ### tldr vs man 비교
  
  ```
  # man: 완전하지만 복잡
  man tar
  
  # tldr: 간단하고 실용적
  tldr tar
  # Output:
  # Create an archive: tar -czf archive.tar.gz file1 file2
  # Extract an archive: tar -xzf archive.tar.gz
  ```
  
  ---
- ## 7. Readline 시그널 및 단축키
- ### 기본 제어 시그널
  
  ```
  Ctrl + C    # SIGINT - 현재 실행 중인 명령 중단
  Ctrl + Z    # SIGTSTP - 프로세스 일시정지 (백그라운드로)
  Ctrl + D    # EOF - 입력 종료 (로그아웃, 프로그램 종료)
  ```
- ### 유용한 Readline 단축키
  
  ```
  # 이동
  Ctrl + A    # 줄 맨 앞으로
  Ctrl + E    # 줄 맨 뒤로
  Ctrl + B    # 한 글자 뒤로 (← 키와 동일)
  Ctrl + F    # 한 글자 앞으로 (→ 키와 동일)
  
  # 편집
  Ctrl + U    # 커서부터 줄 시작까지 삭제
  Ctrl + K    # 커서부터 줄 끝까지 삭제
  Ctrl + W    # 커서 앞 단어 삭제
  Ctrl + Y    # 삭제한 텍스트 붙여넣기
  
  # 히스토리
  Ctrl + R    # 명령어 히스토리 검색 (매우 유용!)
  Ctrl + P    # 이전 명령 (↑ 키와 동일)
  Ctrl + N    # 다음 명령 (↓ 키와 동일)
  
  # 화면
  Ctrl + L    # 화면 지우기 (clear 명령과 동일)
  ```
- ### 프로세스 관리 실용 예제
  
  ```
  # 백그라운드 작업 관리
  sleep 100 &          # 백그라운드에서 실행
  jobs                 # 백그라운드 작업 목록
  fg %1                # 작업 1을 포그라운드로
  bg %1                # 작업 1을 백그라운드로 계속
  
  # Ctrl+Z 후 백그라운드 실행
  some_long_command
  # Ctrl+Z 누름
  bg                   # 백그라운드에서 계속 실행
  ```
  
  ---
- ## 실무 활용 팁
- ### 1. 효율적인 명령어 사용
  
  ```
  # 히스토리 활용
  !!              # 이전 명령 재실행
  !grep           # 'grep'으로 시작하는 최근 명령 실행
  history | grep docker
  
  # 명령어 조합
  cd /var/log && tail -f nginx/access.log
  ```
- ### 2. 개발 워크플로우
  
  ```
  # 개발 환경 설정 스크립트
  #!/bin/bash
  export PATH="$HOME/.local/bin:$PATH"
  export EDITOR=vim
  alias ll='ls -la'
  alias grep='grep --color=auto'
  
  # 프로젝트별 설정
  source .env 2>/dev/null || echo "No .env file found"
  ```
- ### 3. 문제 해결
  
  ```
  # 명령어가 없다고 나올 때
  which command_name
  echo $PATH
  type command_name
  
  # 권한 문제
  ls -la /path/to/file
  sudo !!  # 이전 명령을 sudo로 재실행
  ```
  
  이 가이드를 통해 리눅스 CLI 환경에서 더 효율적으로 작업할 수 있을 것입니다!