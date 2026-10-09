## 🔐 회원가입 관리 (Supabase)

### 신규 회원가입 차단 / 재개 방법
1. Supabase 대시보드 접속 후 로그인
2. 좌측 메뉴 [Authentication] ➔ [Providers] 클릭
3. 목록에서 [Email] 선택
4. [Allow new users to sign up] 스위치 설정:
   - 회원가입 차단: **OFF** 설정 후 하단 [Save] 클릭
   - 회원가입 재개: **ON** 설정 후 하단 [Save] 클릭


# 🚀 게시판 프로젝트 & 프로토타입 시스템 문서

## 1. 이 사이트가 뭐하는 사이트인가요?
* **기능:** 회원가입/로그인, 게시글 작성·수정·삭제, 댓글 작성, 파일 첨부
* **목적:** [예: 개인 블로그용 / 프로토타입 테스트용 / 커뮤니티용]

## 2. 연동된 사이트 (인프라)
| 역할 | 서비스명 | 계정/주소 |비고 |
| :--- | :--- | :--- | :--- |
| **코드 저장소** | GitHub | `https://github.com/gitko11111/kim` | 메인 코드 관리 |
| **웹 배포** | Netlify | `https://regal-melba-913d66.netlify.app/` | 코드가 바뀌면 자동 반영 |
| **데이터베이스** | Supabase | `https://supabase.com` | 게시글, 회원 정보 저장 |
| **자동 유지** | cron-job.org | `https://console.cron-job.org` | DB 자동 정지 방지용 |

## 3. 주요 운영 및 관리 방법
* **회원가입 차단/허용:** Supabase ➔ Authentication ➔ Sign In / Providers ➔ Email ➔ `Allow new users to sign up` 설정
* **데이터 백업:** Supabase ➔ Table Editor ➔ 각 테이블선택 ➔ `Export as CSV`
* **게시판 복제 및 프로토타입 만들기:** 
  1. Supabase 새 프로젝트 생성
  2. Netlify에서 동일한 GitHub 연결 후 환경변수(URL, KEY)만 변경

 ##내 웹사이트 주소 (Netlify)

1.코드 저장소 (GitHub) : 여기 로그인해야 Supabase로그인 가능

2.데이터베이스 (Supabase) : 500mb정도 용량. 용량 다 차면 새로 만들어야.(무료는 2개씩 만들 수 있고. 만들고 정지하면 무한대로 확장 가능. )

3.서버 정지 방지 (cron-job.org) : 여기서 정지 안 당하게 3일에 한번 접속(?)함.

4.웹주소 제공(도메인? 넷리파이) https://app.netlify.com/projects/my-community-board/overview
