# 💻 윈도우 ↔ 맥 간의 Git/GitHub 연동 작업 및 트러블슈팅 완벽 가이드

이 문서는 윈도우 환경에서 작업한 내용을 맥(Mac) 환경으로 안전하게 가져오고, 앞으로 두 기기를 오가며 원활하게 코딩하기 위해 설정한 과정을 요약한 내용입니다. 터미널 작업 중 자주 발생하는 에러와 해결 방법도 함께 정리되어 있습니다.

---

## 1. 🎯 작업 목표
*   윈도우에서 작업하던 파일과 폴더를 맥 환경으로 복사 및 동기화
*   USB나 클라우드 대신 **개발자 표준 방식인 Git과 GitHub**을 활용한 완벽한 버전 관리 환경 구축

---

## 2. 🚀 초기 세팅: 윈도우에서 코드 업로드
윈도우 환경의 터미널에서 로컬 코드를 GitHub 저장소로 처음 업로드하는 과정입니다.

1.  `git init` : 현재 폴더를 Git으로 관리하기 시작
2.  `git add .` : 모든 파일을 업로드 준비 목록(Staging Area)에 추가
3.  `git commit -m "첫 업로드"` : 파일의 현재 상태를 확정(Commit)
4.  `git remote add origin [GitHub 주소]` : 로컬 폴더와 온라인 저장소 연결
5.  `git push -u origin main` : 온라인 저장소로 코드 밀어 올리기(Push)

> [!WARNING]
> **초기 세팅 중 발생할 수 있는 에러 (트러블슈팅)**
> 
> *   `error: failed to push some refs to ...`
>     *   **원인:** GitHub에서 저장소를 만들 때 `README.md`를 함께 생성하여 로컬과 상태가 불일치함.
> *   `fatal: couldn't find remote ref main`
>     *   **원인:** 윈도우 로컬의 기본 브랜치명이 `master`이고, GitHub은 `main`이어서 발생.
> *   **해결 방법:** 로컬 브랜치명을 강제로 변경하고 강제 업로드 진행.
>     ```bash
>     git branch -M main
>     git push -u origin main --force
>     ```

---

## 3. 🍏 맥(Mac)에서 코드 다운로드
윈도우에서 무사히 업로드된 코드를 맥으로 가져오는 과정입니다.

1.  원하는 폴더(예: 바탕화면)로 터미널 경로 이동 
    ```bash
    cd Desktop
    ```
2.  GitHub 저장소 전체 복제 다운로드 
    ```bash
    git clone https://github.com/5060sons/short-form.git
    ```
    *   **결과:** 현재 터미널이 위치한 경로에 `short-form` 이라는 폴더가 생성되며 코드가 안전하게 다운로드 됨.

---

## 4. 🔄 향후 일상적인 작업 루틴 (Daily Workflow)

앞으로 기기를 변경하며 작업할 때는 다음 두 가지만 기억하면 됩니다.

> [!TIP]
> 습관처럼 작업 전후에 `pull`과 `push`를 해주시면 코드가 꼬일 일이 없습니다!

**1️⃣ 작업을 시작할 때 (가장 최신 코드 받아오기)**
```bash
git pull origin main
```

**2️⃣ 작업을 마칠 때 (오늘 작업한 내용 저장하고 올리기)**
```bash
git add .
git commit -m "오늘 작업한 내용 메모 (예: 로그인 기능 추가)"
git push origin main
```

---

## 5. 🚨 흔히 발생하는 Git 명령어 오류와 해결 방법

터미널에서 작업을 진행하다가 자주 겪을 수 있는 3가지 실수와 해결 방법입니다.

### ❌ `git add.` 에러
*   **에러 메시지:** `git: 'add.' is not a git command.`
*   **원인:** 명령어에 **띄어쓰기**가 누락되어 발생. 컴퓨터가 `add.`를 하나의 잘못된 단어로 인식함.
*   **해결 방법:** `add`와 `.` 사이에 스페이스바를 한 번 입력.
    👉 **올바른 명령어:** `git add .`

### ❌ `git commit` 에러
*   **에러 메시지:** `nothing to commit, working tree clean`
*   **원인:** `git add .` 명령어가 실패했거나 실행되지 않아, 업로드 준비 목록(Staging Area)에 담긴 파일이 없기 때문.
*   **해결 방법:** 먼저 `git add .`를 성공적으로 실행하여 변경된 파일을 준비 목록에 담은 후 다시 시도.

### ❌ `git push` 에러
*   **에러 메시지:** `Updates were rejected because the remote contains work that you do not have locally`
*   **원인:** 윈도우 등 다른 기기에서 작업한 최신 코드가 온라인(GitHub)에는 올라가 있으나, 현재 작업 중인 맥에는 아직 반영되지 않아서 발생하는 버전 충돌.
*   **해결 방법:** 강제로 밀어올리기 전에, 온라인의 최신 내용을 먼저 가져오기(Pull)를 수행.
    👉 **해결 순서:** `git pull origin main` → `git add .` → `git commit -m "..."` → `git push origin main`
