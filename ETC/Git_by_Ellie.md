sourcetree / gitKraken

cmder 터미널 / git이 기본적으로 포함되어서 설치됨.

1. Setting

   1. 설치 

      - 터미널과 GUI 도구를 설치한다. 

   2. 설정

      - git config --list

      - git config --global -e : 텍스트 편집기로 편집

      - git config --global [설정이름] [설정값] 

        - git config --global core.editor "code --wait" 
          - editor를 vscode로 설정. vscode가 닫히기 전까지는 터미널 사용 불가
        - git config --global core.autocrlf true(윈도우) / input(Mac) 
          - 윈도우에서는 줄바꿈 할 때 \r\n을 둘 다 넣고, Mac은 \n만 넣는다.
          - \r : carriage-return , \n : line feed
          - 윈도우와 Mac이 충돌 날 수 도 있기에 git에 PUSH 할 때, \n으로 저장되고 PULL 할 때는 운영체제에 맞게 전환해서 가져온다.
          - 윈도우 : push (\r 삭제) , pull (\r 삽입)
          - Mac : push (\r 삭제)

      - 단축키 설정 

        - git config --global alias.[단축어] [명령어]

        ```powershell
        git config --global alias.st stauts
        git st
        ```

2. Basic

   - Working directory : 프로젝트 파일의 작업 공간

     - tracked : git이 파일 정보가 있는 파일
       - unmodified : 이전 버전과 비교하여 수정이 안됨
       - modified : 수정이 됨
     - untracked : git이 파일정보가 없는 파일

   - staging area : 작업 파일들을 옮겨놓음

   - git directory : 버전 히스토리를 가지고 있음

     - Local에 저장하면 Local에서 문제가 생겼을 경우 데이터가 다 없어지기 때문에 원격 저장소에 저장한다.

   - 버전 정보

     - 고유한 해시코드 ID 부여
     - message
     - author
     - date/time

   - gitignore

     - git에 올리고 싶지 않은 파일들을 지정한다.

     ```powershell
     echo *.log > .gitignore
     echo build/ > .gitignore
     echo build/*log > .gitignore
     ```

   - git status

     - 옵션 설정이 가능. 
     - default는 long이며, short 옵션은 간략한 정보 확인이 가능.

     ```powershell
     git status -s
     ```

   - git diff

     - 변경 사항을 볼 수 있음.
     - 변경 사항을 비교하는 Tool을 설정 할 수 있음.

     ```javascript
     [diff]
     	tool = vscode
     [difftool "vscode"]
     	cmd = code --wait --diff $LOCAL $REMOTE
     ```

     ```powershell
     git difftool
     git difftool --staged
     ```

   - git commit

     - staging area에 있는 변경사항을 git repository에 저장한다.

     ```powershell
     git commit
     git commit -m [메세지]
     git commit -am [메세지]
     ```

     - git commit을 하면 에디터를 통해 title과 description을 설정할 수 있다.
     - a옵션을 통해 staging area와 working directory에 있는 모든 파일을 commit 할 수 있다.
     - git repository에 저장하는 것은 프로젝트의 history를 작성하는 것이기 때문에 기능별, 작업별로 commit을 하여 관리해야 한다.
     - 보통 Commit의 메세지는 현재형, 동사형으로 만든다. Init, Add , Fix 등



