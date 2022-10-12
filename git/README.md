# Git 정리

## 초기 설정

```bash
버전 확인
git --version

git에서 master라는 기본 브런치를 main으로 변경됨 오류 방지 목적으로 쓰는 것
(처음에만 쓰면됨)
git config --global init.defaultBranch main


유저명과 유저 이메일 정해주는 것
git config --global user.name "JEONGHYEON KWON"
git config --global user.email "givejeong2468@gmail.com"
설정 제대로 되었는지 확인
git config --list

```

## git 기본 명령어

```bash
# 초기 설정
git init

# 깃 상태 확인
git status

# 파일들 스테이징
git add 현재경로_파일
git add . # 현재 경로의 모든 파일들
git add git/*

# 깃 로그 확인
git log

# 이전 버전으로 돌리기
git reset --hard 버전_해쉬_코드

# 스테이징한 파일들 메시지 넣기
git commit -m "메시지"

# 깃 레포지토리 연결
git remote add origin "repository url.git" #초기 레포지토리 생성시 확인 해보기


# 레포지토리에 스테이징한 파일들
git push origin


# 레포지토리의 내용 가져오기
git clone "레포지토리"


# 변경 사항이 반영된 레포지토리 가져오기
git pull
```
