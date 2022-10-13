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

## branch

```bash
# branch 목록 확
git branch

# branch 생성
git branch newUser

# branch 이동
git checkout newUser

# branch 삭제
git branch -D newUser

# 특정 branch로 push
git push origin newUser

# branch 깃 허브에서 가져와 최신화 한다
git pull origin newUser

```

## clone vs fork

### clone

- 남의 레포지토리에 있는 프로젝트를 내 컴퓨터 폴더로 전체를 다 가져 오는것

```bash
git clone "레포지토리"
```

### fork

- 남의 레포지토리에 있는 프로젝트를 내 계정(깃 허브 레포지토리)에 가져 오는 것 이름 변경 가능

## rebase vs merge

### merge

- 다른 브랜치를 가지고 오는것
- git merge 병합하고\_싶은\_브랜치\_이름

* 브랜치와 브랜치의 합치기는 3가지 종류가 있다
  - 병합 커밋
    - 수정한 두개의 파일을 합쳐서 하나의 파일로 병합
  - 빨리 감기 (fast-forward)
    - 수정안한 파일과 수정한 파일을 합쳐서 수정한 파일로 병함
  - 충돌 (conflict)
    - push 할 때 발생
    - 화살표<<< 와 >>>사이의 === 기준으로 충돌난 부분 보여줌
    - 직접 파일에 들어가서 내용을 수정함

```js

<<< HEAD // 현재 브랜치
modify1
=====
modify2
>>> branch // 병합하려는 브랜치

```
