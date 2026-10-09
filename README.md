# 🚀 With Keke & New Language Study - Web Application Setup Guide

본 문서는 Supabase 백엔드와 Netlify 호스팅을 연동한 웹 애플리케이션의 인프라 설정, 환경변수, 배포 파이프라인 및 트러블슈팅 가이드입니다. 1년 뒤나 새로운 개발 환경에서도 그대로 적용할 수 있도록 모든 핵심 설정을 기술합니다.

---

## 🛠 1. 프로젝트 시스템 구조 및 기술 스택

* **Frontend**: HTML5 / JavaScript / Tailwind CSS (CDN/Vite)
* **Hosting & Deployment**: Netlify
* **Backend as a Service (BaaS)**: Supabase (`ap-northeast-2` 서울 리전)
* **Linked Projects**: `New Language Study`, `With Keke`

---

## 🔑 2. Supabase 서버 접속 정보

* **Project Name**: With Keke
* **Region**: `ap-northeast-2` (Seoul)
* **Project URL**: `https://tspmpdtslcnozbbkhirs.supabase.co`
  > ⚠️ **주의 (URL 오타 주의)**: `tspmpd` 뒤에 **`t`**가 포함되어 있어야 합니다 (`tspmpdtslcnozbbkhirs`). `t`가 빠지면 DNS 조회 오류(`ERR_NAME_NOT_RESOLVED`)가 발생합니다.
* **API Key (anon public)**: `eyJ...`로 시작하는 긴 JWT 토큰 문자열
  > ⚠️ **주의 (키 형식)**: `sb_publishable_...` 형태의 Publishable Key 대신, `Project Settings -> API`의 `anon` `public` (`eyJ...`) 키를 사용해야 `401 (Unauthorized)` 에러가 발생하지 않습니다.

---

## ⚙️ 3. Netlify 환경변수 (Environment Variables) 설정

Netlify Dashboard $\rightarrow$ `Site configuration` $\rightarrow$ `Environment variables`에 반드시 등록해야 하는 항목입니다.

| Variable Key | Value / Example | Description |
| :--- | :--- | :--- |
| `SUPABASE_URL` | `https://tspmpdtslcnozbbkhirs.supabase.co` | Supabase API 접속 엔드포인트 |
| `SUPABASE_ANON_KEY` | `eyJ...` (JWT anon key) | Supabase 클라이언트 퍼블릭 인증 키 |
| `VITE_SUPABASE_URL` | `https://tspmpdtslcnozbbkhirs.supabase.co` | Vite 프레임워크 빌드 시 주입용 변수 |
| `VITE_SUPABASE_ANON_KEY` | `eyJ...` (JWT anon key) | Vite 프레임워크 빌드 시 주입용 키 |
| `SECRETS_SCAN_ENABLED` | `false` | Netlify 빌드 시 API 키 차단 차단 해제 |
| `SECRETS_SCAN_OMIT_KEYS` | `SUPABASE_ANON_KEY, SUPABASE_URL` | 시크릿 스캐너 예외 처리 대상 |

---

## 🛡️ 4. Netlify 빌드 오류 (Secrets Scan) 방지 설정

Netlify는 빌드 중 환경변수 내 `SUPABASE_ANON_KEY`나 URL 패턴을 감지하면 보안 위협으로 판단하여 빌드를 블록(`Build Failed`)할 수 있습니다.

### 해결 조치
1. Netlify 환경변수에 `SECRETS_SCAN_ENABLED = false` 추가
2. `SECRETS_SCAN_OMIT_KEYS = SUPABASE_ANON_KEY, SUPABASE_URL` 추가
3. 설정 후 반드시 **`Trigger deploy -> Deploy project without cache`**로 배포 진행

---

## 🔐 5. Supabase Auth (인증) 및 CORS 설정

1. [Supabase Dashboard](https://supabase.com/dashboard) 접속 $\rightarrow$ 프로젝트 선택
2. `Authentication` $\rightarrow$ `URL Configuration` 이동
3. **Site URL**: `https://<your-netlify-app-name>.netlify.app` 등록
4. **Redirect URLs**: Netlify 사이트 도메인 및 로그인/회원가입 리다이렉트 경로 등록
5. `Authentication` $\rightarrow$ `Providers` $\rightarrow$ `Email`:
   * 이메일 인증 절차 없이 바로 가입시키려면 `Confirm email` 비활성화 (`Off`)

---

## 🚨 6. 트러블슈팅 마스터 가이드 (자주 발생하는 오류 및 해결)

### 1) `net::ERR_NAME_NOT_RESOLVED`
* **원인**: 
  1. `SUPABASE_URL`에 오타가 존재함 (`tspmpdsl` vs `tspmpdtsl`).
  2. Supabase 프로젝트가 무료 플랜 미사용으로 인해 일시 정지(`Paused`)됨.
* **해결**:
  1. URL 중간에 **`t`**가 들어간 `https://tspmpdtslcnozbbkhirs.supabase.co` 주소로 교체.
  2. Supabase 대시보드에서 `Restore project` 버튼을 눌러 프로젝트 가동 (`Active` 상태 확인).

### 2) `401 (Unauthorized)`
* **원인**: `SUPABASE_ANON_KEY`에 잘못된 API 키(`sb_publishable_...` 등)가 주입됨.
* **해결**: Supabase 대시보드 `Project Settings -> API`에서 **`eyJ...`**로 시작하는 **`anon public`** 키를 복사하여 Netlify 환경변수에 재입력.

### 3) 소스 코드/사이트에 이전 설정값이 계속 적용되는 현상
* **원인**: Netlify 빌드 캐시 또는 브라우저 캐시 문제.
* **해결**:
  1. Netlify: `Deploys` $\rightarrow$ `Trigger deploy` $\rightarrow$ `Deploy project without cache` 실행.
  2. 브라우저: `Ctrl + F5` (Mac: `Cmd + Shift + R`) 강력 새로고침 또는 시크릿 창 접속.

---

## 🔄 7. 유지보수 및 재배포 표준 체크리스트

1. [ ] Supabase 프로젝트가 `Active` 상태인지 확인
2. [ ] Netlify Environment Variables 값(`SUPABASE_URL`, `SUPABASE_ANON_KEY`) 확인
3. [ ] 배포 실행 시 `Deploy project without cache` 선택
4. [ ] 배포 후 브라우저 개발자 도구(`F12`) Console 및 Network 탭에서 200 OK 상태 확인
