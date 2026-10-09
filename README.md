## 🔐 회원가입 관리 (Supabase)

### 신규 회원가입 차단 / 재개 방법
1. Supabase 대시보드 접속 후 로그인
2. 좌측 메뉴 [Authentication] ➔ [Providers] 클릭
3. 목록에서 [Email] 선택
4. [Allow new users to sign up] 스위치 설정:
   - 회원가입 차단: **OFF** 설정 후 하단 [Save] 클릭
   - 회원가입 재개: **ON** 설정 후 하단 [Save] 클릭


##내 웹사이트 주소 (Netlify)

1.코드 저장소 (GitHub) : 여기 로그인해야 Supabase로그인 가능
2.데이터베이스 (Supabase) : 500mb정도 용량. 용량 다 차면 새로 만들어야.(무료는 2개씩 만들 수 있고. 만들고 정지하면 무한대로 확장 가능. )
3.서버 정지 방지 (cron-job.org) : 여기서 정지 안 당하게 3일에 한번 접속(?)함.
4.웹주소 제공(도메인? 넷리파이) https://app.netlify.com/projects/my-community-board/overview
