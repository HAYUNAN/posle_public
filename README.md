# 🐾 Hugroomer (허그루머) — Pet Care B2B SaaS

**"전문 미용사를 위한 올인원 운영 파트너"**

애견미용샵 및 펫케어 비즈니스의 디지털 전환을 위한 B2B SaaS 플랫폼입니다.
단순한 예약 관리를 넘어 AI 기반 리텐션 도구, 실시간 커뮤니케이션, 케어 모듈, 정기결제 인프라를 제공합니다.

> 기획·DB 설계·UI/UX·백엔드·모바일 앱까지 전 과정을 1인 풀스택으로 개발한 상용 수준의 SaaS 프로젝트입니다.

---

## 핵심 기능

| 기능 | 설명 |
|---|---|
| **스마트 예약 타임라인** | 직원별 Gantt 차트, 실시간 겹침 감지·시각화, 지명 예약 시스템 |
| **AI 재방문 문자** | Claude 3.5 기반 — 마지막 방문 D+30·D+45 자동 감지 후 3가지 톤의 맞춤 초안 생성 |
| **케어 모듈** | 펫호텔·유치원 통합 관리 — 공간 CRUD, 체크인/아웃, 돌봄 일지, AI 케어 리포트 |
| **보호자 앱** | React Native Expo — 예약·사진 갤러리·전자 동의서·멤버십·Expo Push 알림 |
| **정기결제 인프라** | 토스페이먼츠 빌링키 기반 구독 + 예약금 시스템 + 구독권/횟수권 |
| **무게별 가격 티어** | 서비스별 단일/구간별 가격 설정, 샵 맞춤 구간 레이블 |
| **실시간 채팅** | Supabase Realtime 기반 DM·그룹 채팅, 웹 푸시 알림 연동 |
| **전자 동의서** | 예약 확정 시 SMS 링크 자동 발송 → 앱 없이 웹에서 타이핑 서명 |

---

## 기술 스택

### Frontend & Mobile
- **Next.js 14** (App Router) + TypeScript (Strict) + TailwindCSS + shadcn/ui
- **React Native** (Expo SDK 54, New Architecture) — 보호자 앱
- **Recharts** — 매출 통계 차트
- **Turborepo** — 모노레포 빌드 최적화

### Backend & Infrastructure
- **Next.js Server Actions** — API Routes 없이 타입 세이프 서버 로직
- **Supabase** — PostgreSQL + Auth + Storage + RLS + Realtime
- **Row Level Security** — DB 레벨 멀티테넌트 데이터 격리
- **Vercel** — 웹 배포 + Cron Jobs
- **EAS** (Expo Application Services) — Android APK 빌드

### AI & 외부 연동
- **Anthropic Claude API** — 재방문 문자 초안 생성, 케어 일지 AI 리포트
- **Solapi** — HMAC-SHA256 인증 SMS 발송
- **토스페이먼츠** — 빌링키 발급·정기결제·일반결제·환불
- **web-push + VAPID** — 웹 PWA 푸시 알림
- **Expo Push API** — 모바일 앱 푸시 알림

---

## 아키텍처 하이라이트

### 1. 예약 겹침 시각화 (Greedy Column Assignment)
직원별 Gantt 타임라인에서 동일 시간대 겹치는 예약을 동적 열 인덱스로 분배합니다. 예약 블록의 `width`와 `left`를 런타임에 계산해 겹침 정도에 따라 유연하게 배치합니다.

### 2. 멀티테넌트 보안 (Row Level Security)
애플리케이션 레벨 필터링에 의존하지 않고 DB 레벨에서 RLS 정책을 적용합니다. `get_my_shop_id()` SECURITY DEFINER 함수로 모든 쿼리에 샵별 격리를 강제합니다.

### 3. 통합 메시징 레이어 (messaging.ts)
SMS와 카카오 알림톡을 자동 선택하는 통합 레이어입니다. `ALIMTALK_ENABLED` 환경변수 하나로 전환 가능하며, 알림톡 실패 시 SMS로 자동 폴백합니다.

### 4. 전자 동의서 퀵 패스
예약 확정 시 1회용 `public_token`을 생성해 SMS로 발송합니다. 보호자는 앱 설치 없이 웹 브라우저에서 서명을 완료할 수 있습니다 (국내 법적 유효).

### 5. 케어 모듈 통합 설계
기존 `hotel_*` 테이블을 `care_*` 테이블로 마이그레이션했습니다. `category` 컬럼(hotel·kindergarten)으로 분기해 하나의 코드베이스로 두 서비스를 관리합니다.

---

## 프로젝트 구조

```
hugroomer/
├── apps/
│   ├── web/       # Next.js 14 — 미용샵 운영 대시보드 (B2B)
│   ├── mobile/    # React Native Expo — 보호자 앱 (B2C)
│   └── landing/   # 마케팅 랜딩페이지
├── packages/
│   ├── ui/        # 공유 디자인 시스템 (shadcn/ui 기반)
│   ├── types/     # DB 스키마와 1:1 매핑되는 공통 타입
│   └── utils/     # 순수 함수 유틸리티
└── supabase/
    └── schema.sql # Single Source of Truth — 마스터 스키마
```

---

## 구현 현황

### B2B 웹 대시보드
- [x] 인증 (로그인·회원가입·온보딩·직원 초대 링크)
- [x] 예약 관리 (Gantt 타임라인, 겹침 감지, 지명 예약, 예약금)
- [x] 고객·반려동물 관리 (블랙리스트 3단계, 멤버십 연동)
- [x] 미용 기록 (Before/After 사진, 건강 이력)
- [x] 케어 모듈 (펫호텔·유치원, 돌봄 일지, AI 리포트)
- [x] 직원 관리 (RBAC, 근무 스케줄, 연차·휴가 결재)
- [x] AI 재방문 문자 (D+30·D+45 감지, 3가지 초안)
- [x] 매출 통계 (기간별·직원별·서비스별 차트)
- [x] 서비스 관리 (카테고리 탭, 무게별 가격 티어, 드래그 순서)
- [x] 구독권·횟수권 (상품 설계, 예약 완료 시 자동 차감)
- [x] 실시간 채팅 (DM·그룹, Supabase Realtime)
- [x] 알림 시스템 (인앱·웹 푸시·Expo Push)
- [x] 전자 동의서 (SMS 퀵 패스, 타이핑 서명)
- [x] 정기결제 (토스페이먼츠 빌링키, Cron 자동 청구)
- [x] 랜딩 페이지

### B2C 보호자 앱 (React Native)
- [x] 로그인·회원가입·전화번호 계정 연결
- [x] 예약 생성·수정·취소 (포인트 사용, 예약금 웹뷰 결제)
- [x] Before/After 사진 갤러리
- [x] 전자 동의서 작성
- [x] 멤버십 구매 (토스 웹뷰 결제)
- [x] Expo Push 알림 (예약 확정·케어 체크인/아웃·일지)

---

## 주요 설계 결정

**왜 Server Actions인가?**
클라이언트-서버 간 타입 세이프티를 강화하고 API 엔드포인트 관리 공수를 줄이기 위해 Next.js 14 Server Actions를 적극 활용했습니다.

**왜 Supabase인가?**
1인 개발 환경에서 인증·실시간·스토리지 인프라 구축 비용을 최소화하고 비즈니스 로직에 집중하기 위해 선택했습니다. PostgreSQL의 RLS로 보안 로직을 DB 레벨로 이관했습니다.

**Cross-Platform UI 전략**
웹은 `shadcn/ui`, 모바일은 React Native를 사용하되 `packages/ui`에서 디자인 시스템 핵심 로직을 공유해 브랜드 일관성을 유지했습니다.

---

## 소스코드 안내

본 저장소의 상세 구현 코드는 지식재산권 보호를 위해 비공개로 관리됩니다.
아키텍처 설계나 기술 구현 방식에 대한 논의는 아래 연락처로 문의해 주세요.

---

## Contact

- **Email**: yoonani2720@naver.com
- **LinkedIn**: https://www.linkedin.com/in/hayunan/
- **GitHub**: https://github.com/HAYUNAN

<p align="center">© 2026 Hugroomer. All rights reserved.</p>
