# Git

1. 개요
   - SVN은 중앙집중식으로 대부분의 기능이 완성된 상태로 Commit을 하게 된다.
   - Git은 분산형 버전 관리를 지향하여, 각 개발자별로 자신만의 commit history를 가지고 개발자의 Repository와 서버의 Repository를 독립적으로 운영하는 것이 가능하다.
   - 각 개발자별 commit history가 존재하기 때문에 한 개발자의 수정내역이 다른 개발자에게 영향을 미치지 않으며, 통합 관리자가 원하는 시점에 유연하게 원하는 개발자의 commit history를 가져와 운영할 수 있다.
   - Repository 구조

![img](https://t1.daumcdn.net/cfile/tistory/99E842355DAFD9FE0C)

1. git init
   - 새로운 저장소를 생성한다.
   - 저장소 구성을 위한 .git 폴더가 생성되며, 이 폴더에 프로젝트 관리를 위한 파일들과 config 파일 등이 들어간다.
2. git status
   - 현재 저장소 내 파일들의 상태들을 보여준다.
   - 현재 작업중인 브랜치 / commit 내역 / Untracked files / Modified files 등을 확인할 수 있다.
3. git add
   - 작업폴더(Working dir)에서 추가 및 변경하고자 하는 파일을 Index에 기록(stage)한다.
   - git add [파일명] 명령을 이용해 원하는 파일을 기록한다.
   - git add . 명령어를 통해 해당 폴더의 Untracked Files 모두를 stage 할 수 있다.
4. git rm
   - git rm --cached
   - Index의 파일들을 다시 working directory로 내려보낸다.
5. git commit
   - Index의 파일들을 HEAD에 적용한다.
   - git commit -m [설명] 명령을 통해 설명을 적용한다.
   - git commit -am [설명] 명령어를 통해 스테이징 절차(add)를 생략하고 add와 commit을 동시에 할 수 있다.
6. git log
   - git 내역을 확인 할 수 있다.
   - 여기서 확인할 수 있는 확정본 식별자를 기준으로 다시 해당시점으로 되돌아 오거나 할 수 있다.
7. git remote
   - 원격 저장소(remote repository) 연결을 관리한다.
   - git remote -v  : 현재 연결되어 있는 remote repository를 확인한다.
   - git remote add [NAME] [URL] : NAME의 이름으로 저장소를 연결한다.
   - git remote remove [NAME] : NAME의 저장소 연결을 끊는다.
8. git branch
   - Branch를 관리한다.
   - git branch [NAME] : NAME의 이름으로 브랜치를 생성한다.
   - git checkout -b [NAME] : NAME의 이름으로 브랜치를 생성하는 동시에 체크아웃한다.
   - git push [remote명] [branch명] : 해당 remote 저장소에 branch를 저장한다.
   - git branch : 브랜치 목록을 조회한다.
     - git branch : 로컬 브랜치 목록 조회
     - git branch -r : 원격 브랜치 목록 조회
     - git branch -a : 모든 브랜치 목록 조회
   - git branch -d [branch명] : 로컬 브랜치를 삭제한다.
     - merge 되지 않거나 충돌이 해결되지 않은 branch의 경우 에러가 날 수 있다.
     - 이 경우, -D 옵션을 통해 강제로 삭제할 수 있다.
     - git branch -D [branch명]
   - git push origin --delete [branch명] : 원격 저장소의 브랜치를 삭제할 수 있다.  
9. git merge
   - 다른 branch에 있는 변경 내용을 현재 가지에 merge 한다.
10. git tag
    - git tag [꼬리표] [확정본 식별자] : 원하는 tag를 달 수 있다.
11. git checkout
    - git checkout -- [파일 이름] : 원하는 파일을 변경 전 상태(HEAD)로 되돌려준다.
      - 다만, 이미 인덱스에 추가된 변경 내용과 새로 생성된 파일은 그대로 남는다.
12. git reset
    - git reset [파일 이름] : staged 상태에서 Modified 상태로 되돌린다.
13. git fetch origin  /  git reset --hard origin/master
    - 원격 저장소의 최신 이력을 가져오고, 로컬 master 가지가 그 이력을 가리케도록 할 수 있다.
