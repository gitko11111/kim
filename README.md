# 🚀 게시판 프로젝트 & 프로토타입 시스템 관리 가이드

이 문서는 웹사이트 운영, 유지보수, 프로토타입 복제 및 주요 인프라 관리를 위한 통합 시스템 가이드 문서입니다.

---

## 1. 프로젝트 개요

* **주요 기능:** 회원가입/로그인, 게시글 작성·수정·삭제, 댓글 작성, 파일 첨부
* **운영 목적:** 게시판 서비스 운영 및 프로토타입 테스트

---

## 2. 연동 서비스 (인프라 현황)

| 역할 | 서비스명 | 주소 / 대시보드 | 주요 기능 및 특징 |
| :--- | :--- | :--- | :--- |
| **코드 저장소** | **GitHub** | [저장소 바로가기](https://github.com/gitko11111/kim) | 메인 소스코드 관리 (index.html 등) |
| **웹 배포** | **Netlify** | [실제 사이트](https://regal-melba-913d66.netlify.app/) / [대시보드](https://app.netlify.com/projects/my-community-board/overview) | 코드가 변경되면 10~30초 내 자동 배포 |
| **데이터베이스** | **Supabase** | [대시보드 접속](https://supabase.com) | 게시글, 댓글, 회원정보, 파일 저장소 |
| **자동 유지** | **cron-job.org** | [콘솔 접속](https://console.cron-job.org) | DB 일시정지(Sleep) 방지용 주기적 호출 |

---

## 3. 주요 운영 및 수정 방법

### 3.1 소스코드 수정 및 자동 배포 (GitHub & Netlify)
* **코드 수정:** GitHub 저장소의 `index.html` 파일을 수정 후 **`Commit changes`**를 완료하면 작업이 끝납니다.
* **자동 업데이트:** GitHub에 코드가 업데이트되면 연동된 **Netlify가 변경사항을 자동으로 감지하여 10~30초 이내에 실제 웹사이트에 즉시 반영**합니다.

### 3.2 신규 회원가입 차단 및 허용 설정 (Supabase)
이벤트 종료나 관리 목적으로 신규 회원가입을 잠시 막거나 다시 열어야 할 때 사용합니다.
1. [Supabase 대시보드](https://supabase.com) 접속 후 로그인
2. 좌측 메뉴 **[Authentication]** ➔ **[Providers]** 클릭
3. 목록에서 **[Email]** 선택
4. **[Allow new users to sign up]** 스위치 설정:
   * **회원가입 차단:** `OFF` 설정 후 하단 **[Save]** 클릭
   * **회원가입 재개:** `ON` 설정 후 하단 **[Save]** 클릭

### 3.3 데이터 백업 (CSV 추출)
1. Supabase ➔ **[Table Editor]** 이동
2. 백업할 테이블(예: `posts`, `users` 등) 선택
3. 우측 상단 **[Export as CSV]** 클릭하여 데이터 다운로드

### 3.4 게시판 복제 및 새 프로토타입 만들기
1. **Supabase:** 새 프로젝트를 생성하고 DB 테이블 구조(Schema) 세팅
2. **Netlify:** 동일한 GitHub 저장소(`gitko11111/kim`)를 연결하여 새 사이트 생성
3. **환경변수 변경:** 새로 만든 Netlify 사이트의 Environment Variables에 새 Supabase의 `URL`과 `ANON_KEY` 적용

---

## 4. 인프라 운영 유의사항 & 꿀팁

* **GitHub 계정 관리:** Supabase 등의 주요 서비스가 GitHub 연동 계정으로 연결되어 있으므로, GitHub 계정 보안 및 로그인을 항상 유지해 주세요.
* **Supabase 용량 & 프로젝트 관리:**
  * 무료 플랜은 **프로젝트당 500MB DB 용량**을 제공하며, **동시 활성화 프로젝트 2개**로 제한됩니다.
  * 용량이 가득 차면 새 프로젝트를 생성해야 합니다.
  * 기존 프로젝트를 'Pause(정지)' 상태로 두면 무료 개수 제한에 걸리지 않고 여러 프로토타입 프로젝트를 계속 생성해 둘 수 있습니다.
* **DB 서버 정지 방지 (cron-job.org):**
  * Supabase 무료 DB는 일정 기간 요청이 없으면 서버가 잠드는(Sleep) 현상이 있습니다.
  * `cron-job.org`를 통해 3일에 한 번씩 정기적으로 DB에 Ping(접속 요청)을 보내어 **DB가 자동 정지되지 않도록 방지**하고 있습니다.
📝 GitHub README.md에 반영하는 방법
