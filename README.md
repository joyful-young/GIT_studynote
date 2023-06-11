# 생활코딩 - GIT 강의

- 2023.6.4 ~ 2023.6.7 생활코딩 간헐적 학습 강의
- 하루 8시간 강의 후 3일간 1시간 이내로 복습

- VS code GUI 이용 GIT 강의
- VS code 확장프로그램 Git Graph 설치

## 버전관리 프로그램을 사용하는 이유

### 버전관리를 손으로 하면

- 파일 이름 지저분해짐
- 각각의 버전을 잘 설명하지 못함

### 전문 소프트웨어 등장 - 버전 관리 시스템

3가지 임무
1. `버전`을 만드는 것
2. 인터넷 상의 다른 컴퓨터로 안전하게 `백업`
3. 백업을 매개로 파일 공유 -> `협업`

### 버전을 만드는 이유

- 프로그램을 한 번에 만들지 못함
- 여러 번의 수정. 여러 개의 버전
- 프로젝트 과정 중에 생기는 버그
  - 버그가 당시에 바로 발견되지 않는 경우 많음
  - 버전 관리 하지 않으면 모든 코드를 다 뒤져봐야.
  - 버전 관리 했을 경우 버그가 발생한 지점의 버전에서 조사하면 됨.
- **버전 관리를 이용해서 디버깅**
- 근데 사람들이 깃의 버전 관리로 디버깅은 잘 안 함.
- 깃을 잘 모르고 디버깅에 깃을 사용하면 작업물이 날아감.
- 잘 알고 사용하자.

## 실습

### 단위 작업

- 커밋은 하나의 단위 작업마다 해야 한다.
- 디버깅을 위해 버전을 작을수록 좋다. 하나의 버전은 하나의 주제만 갖고 있어야 한다.

### add

- `working directory`의 버전을 `staging area`로

#### 왜 바로 커밋하지 않을까? - staging area의 존재 이유

- 다른 버전 관리 프로그램에는 없던 개념이라고 함.
- 일부분만 commit 하려고 할 때 유용
  - 원하는 파일만 add해놓고 commit
- 여러 개의 파일을 병합하다가 여러 파일에서 충돌이 발생했을 경우
  - 파일별로 충돌을 해결할 때마다 중간에 commit을 해두는 것이 안정적
- commit을 수정할 때
  - 과거의 commit 이력을 수정하려고 할 때 파일 상태를 staged로 내리고 추가적으로 변경할 사항만 반영해 commit 하는 게 효율적

### commit

- *만든 변경 사항을 깃에 제출해 버전을 만드는 행위*
- `staging area`의 버전이 `repository`로
- 여러 개의 단위 작업을 해버렸을 때 파일을 쪼개서 버전 관리를 해야 한다.
  - 같은 버전을 하나로 묶고 다른 버전을 분리
- **작업 끝났을 때 내가 한 작업 확인하는 습관 들이기**

#### commit ID

- 파일 내용을 기반으로 해시 알고리즘을 이용해 만들어진 고유한 식별자

### master & HEAD

- `master`는 **마지막으로 작업한 버전**을 가리킴 (시간여행하기 직전의 시간)
- `HEAD`는 현재 작업을 가리킴. 현재 `working dir`이 어떤 버전과 같은지를 가리킴. `HEAD`를 바꾸면 그 버전의 `working dir`로 바뀐다.

### 원하는 버전으로 이동

- `checkout`: 현재 `working dir`에서 다른 버전으로 바꾸기 위해 `HEAD`를 바꿈.
  - `$ git checkout "가고 싶은 버전의 commit ID"`

<br/>

- "각각의 버전은 그 버전이 만들어진 시점의 `staging area`의 `스냅샷`이다."
  - 그래서 (바뀐 파일만 표시되지만) 실제로 `staging area`는 모든 내용을 다 갖고 있다.
  - 그럼 중복이 너무 심하지 않은가? 용량은?
    - 깃은 중복되는 건 더이상 새로 저장하지 않는다.
- `checkout`하면 그 버전 내용을 `staging area`에 씌우고 `working directory`도 덮어씌움.`HEAD`는 그 버전의 commit ID

<br/>

- 다시 현재로 돌아가고 싶으면?
  - `$ git checkout "돌아가고 싶은 버전 commit ID"`
  - `$ git checkout master` -> 이걸 써야 한다!

  ![현재로 돌아가기](./%EA%B9%83%EC%97%90%EC%84%9C%ED%98%84%EC%9E%AC%EB%A1%9C%EB%8F%8C%EC%95%84%EA%B0%80%EA%B8%B0.png)

- 그림에서, `git checkout C버전`을 한 다음에 새로운 커밋(D, E)을 하면 master는 가만히 있고 HEAD만 새로운 커밋을 따라감.
- 그 상태에서 `git checkout master`를 하면 HEAD가 master를 가리키고 working dir은 버전 C와 같아진다.
- D, E는 사라짐.
  - 아예 없어진 건 아님. `git checkout E`하면 살아남.
  - **위험한 작업 시에는 commit ID를 적어좋으면 복원 가능하다.**

<br/>

- 실험적인 작업을 하고자 할 때 일부러 이렇게 하기도 한다.
- 이걸 `detached head state`라고 함.
- 실패하면 쉽게 버릴 수 있다.
- 여기에 이름을 붙여놓으면 그게 `branch`가 되는 것

<br/>

- `master`의 위치를 바꾸려면?
  - 일단 `HEAD`가 `master`를 가리켜야한다. `$ git checkout master`
  - `$ git reset E` (`HEAD`가 가리키는 `master`가 E를 가리키게 됨)
- `checkout`: `HEAD`를 바꿈
- `reset`: `HEAD`가 가리키는 `branch`를 바꿈

### git log

- `$ git log --oneline`: 로그가 한줄씩만 나옴
  - `HEAD`의 부모를 따라가며 리스트를 보여준다.
  - `--all` 옵션을 붙이면 다 보여줌.
- `$ git log --oneline --graph`: 그래프로 보여줌

### branch

- 버전에 이름을 붙이고 싶다 -> `branch`
- VS code GUI
  - 버전 우클릭 -> create branch -> 브랜치명 설정
- `$ git branch "브랜치명"`: 새로운 브랜치 생성
- `$ git checkout -b "브랜치명"`: 브랜치 생성과 전환을 동시에. HEAD가 새로 생성된 branch를 가리킴.
- `$ git switch -c "브랜치명"`: 위와 같음. (-c: create)

#### checkout & switch

- git 2.23 버전부터 `checkout` 명령을 대신할 `switch` 명령과 `restore` 명령이 도입됨
- `checkout` 명령어가 가진 기능이 너무 많기 때문
- `switch`와 `restore` 사용하는 게 권장됨
- restore는 working tree의 파일을 복원해주는 역할

### merge

- 코딩 과정에서 실패를 두려워하지 않으려면
  - 쉽게 버릴 수 있어야하고,
  - 성공 시 합칠 수 있어야 한다.

![병합](./%EB%B3%91%ED%95%A9.png)

- 병합해서 생긴 버전의 부모는 병합된 두 버전이다.
- 그림에서 `HEAD`가 가리키고있던 `master`가 새로운 커밋을 따라간다.

<br/>

- **병합할 땐 방향이 중요**
- "`master`가 `feature/1`을 병합"
  - 움직이는 주체는 `master`, 여기에 `HEAD`가 가야.
    - 이 상태에서 `feature/1` 우클릭 -> Merge into current branch
    - `$ git merge "병합 대상 브랜치명"`
- 결과
  - branch가 합쳐짐(병합)
  - 새로운 버전 만들어짐
  - master branch가 새로 만든 버전 따라감

<br/>

- branch를 사용하면 버리기 쉽고 병합하기 쉽다.
- **branch 만들고 나면 HEAD가 누군지 꼭 확인할 것!**

### conflict

![병합 - 같은 이름 파일 수정](./%EB%B3%91%ED%95%A9_%EA%B0%99%EC%9D%80%EC%9D%B4%EB%A6%84%ED%8C%8C%EC%9D%BC%EC%88%98%EC%A0%95%EC%8B%9C.png)

- 충돌 나는 부분을 어떻게 할지 물어봄

<br/>

![2-way, 3-way merge](./%EB%B3%91%ED%95%A9_2way_3way.png)

![충돌 상황 만들기](./%EC%B6%A9%EB%8F%8C%EC%83%81%ED%99%A9%EB%A7%8C%EB%93%A4%EA%B8%B0.png)

#### 2-way merge

- 병합할 두 버전에서 같은 파일인데 내용이 다르면 일일이 다 정해줘야 함.

#### 3-way merge

- GIT은 병합할 두 버전의 공통 조상을 찾아 `Base`라고 이름붙인다.
- `Base`, V1, V2가 삼자대면. `Base` 기준으로 한쪽은 수정이 되고 다른 한쪽은 수정이 안 되었으면 수정된 것으로 Git이 알아서 정해 줌.
- V1, V2에서 모두 수정되었는데 내용이 다른 부분만 사람이 정해주면 된다.

<br/>

- VS code GUI 이용 시 `병합편집기에서 확인` 이용.
  - 결과 화면부터 보기. 원래(`Base`)는 뭐였고 내가 작업한 건 뭐고 다른 쪽에서는 어떻게 작업했는지 확인.
  - 파일 적절히 수정해서 병합

### 원격 저장소 - Github

- 원격저장소 여러개를 연결할 수도 있다.
  - -> 별명을 지음. 보통 origin
- `origin/master` branch가 생김. - 원격저장소에 어디까지 업로드했는지를 나타낸다. "remote tracking branch"
- 추가: `$ git remote add <원격 저장소 이름> <원격 저장소 링크>`
- 제거: `$ git remote remove <원격 저장소 이름>`
- 이름 변경: `$ git remote rename <기존 이름> <변경할 이름>`

### 협업

- 서로 다른 이름의 파일 수정
  - 충돌이 일어나지는 않음
  - 바로 push는 안 됨. `reject`
- `pull` = `fetch` + `merge`
  - `fetch`: 원격저장소의 변경사항을 다운로드 받음.
  - `merge`: `origin/master`를 `master`로 병합
- 원격저장소 `origin/master`와 지역저장소 `master`를 다른 걸로 판단.
- **커밋 갖고 오래 있지 말고 자주 push할 것!!**

## 복습 - 추가적인 팁

### 다른 branch를 만들어 작업할 때

- 기본 branch는 대체로 master(main)
- 실험적인 기능 개발을 위한 branch `feature/1`이 있다고 할 때, 그 개발이 성공했을 경우 `master`가 `feature/1`을 병합하게 될 것이므로 그때의 충돌을 줄이기 위해서는 개발 과정에서 `feature/1`이 `master`의 내용을 자주 가져가는 게 좋다.

### add의 3가지 의미

1. 커밋 대기 상태로 만듦
  - 추적되지 않는 파일(untracked)을 추적되는 파일(tracked)로 만들어 `staged`(커밋할 수 있는 상태)로
  - 수정된 파일(tracked, `modified`)을 `staged`로
2. `untracked` -> `tracked`
  - add된 적 있는 파일은 tracked 상태
  - tracked 상태에는 `unmodified`, `modified`, `staged`가 있음
3. 충돌난 파일의 충돌이 해결됐음을 알림

### 그 밖의 명령어

- commit 할 때 add를 자동으로 해서 하도록
  - `$ git commit -a -m "커밋 메시지"`
  - auto add
  - 이때 untracked 파일은 add되지 않는다.
- add 취소
  - `$ git restore --staged <파일명>`
- commit을 지우고싶다
  - 1, 2, 3 중 2를 없애고 싶으면 HEAD를 1로 옮기면 됨
  - `git reset --hard <commit 1의 커밋 아이디>`

### 기타

- 원격 저장소로 push한 것은 삭제하면 안 된다. 로컬에 있는 동안만 삭제 가능
- 이미 원격 저장소에 공유된 커밋으로는 HEAD를 옮길 수 없다. reset 불가
- rebase를 할 경우 pull이 아니고 fetch를 해야한다.
  - rebase는 한 브랜치에서 다른 브랜치로 합치는 또다른 방법
  - 나중에 다시 공부하기
- github로 협업 시 settings -> collaborators and teams -> add people, add teams