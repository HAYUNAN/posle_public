# 🐾 Hugroomer (허그루머) - All-in-One Pet Care SaaS

**"전문 미용사를 위한 최고의 운영 파트너"**  
애견미용샵 및 펫케어 비즈니스의 DX(Digital Transformation)를 위한 엔터프라이즈급 B2B SaaS 플랫폼입니다.
단순한 예약 관리를 넘어 AI 기반 리텐션 도구, 실시간 커뮤니케이션 인프라, 엔터프라이즈급 보안 아키텍처를 제공합니다.

> **"원장님은 미용에만 집중하세요, 나머지는 허그루머가 관리합니다."**  
> 본 프로젝트는 1인 개발로 기획, DB 설계, UI/UX, 백엔드 로직, 모바일 앱까지 전체 풀스택 과정을 수행하며 실제 상용 수준의 완성도를 목표로 개발되었습니다.

---

## ✨ 주요 기능 및 비즈니스 가치

| 기능 | 설명 |
|---|---|
| **Enterprise Timeline** | **Greedy Column Assignment** 알고리즘을 적용하여 직원별 예약 겹침을 실시간으로 계산하고 시각화합니다. |
| **AI Retention Engine** | **Claude 3.5 Sonnet**을 활용하여 단순 예약 유도를 넘어, 각 반려동물의 미용 주기와 특성에 맞춘 초개인화 메시지를 생성합니다. |
| **Real-time Engine** | **Supabase Realtime** 기반의 채팅 및 인앱 알림 시스템을 통해 샵 내부 협업 효율을 극대화합니다. |
| **Smart Care Module** | 펫호텔, 유치원 등 복잡한 입/퇴실 프로세스를 자동화하고 AI 기반 돌봄 리포트를 제공합니다. |
| **Cross-Platform** | Next.js 14(Web)와 React Native(Mobile) 간의 코드 및 타입 공유를 통해 일관된 데이터 경험을 제공합니다. |

---

## 💻 Technical Stack & Architecture

### Framework & Language
- **Frontend**: Next.js 14 (App Router), React 18, TypeScript (Strict Mode)
- **Mobile**: React Native (Expo SDK 50+), New Architecture 기반 빌드
- **State Management**: TanStack Query (Server State), Server Actions (Mutation)
- **Styling**: TailwindCSS, Radix UI (Shadcn/ui 기반 커스텀)

### Backend & Cloud Native
- **Database**: PostgreSQL (Supabase)
- **Security**: Row Level Security (RLS) 기반 테넌트 격리
- **Storage**: Supabase Storage (미용 전/후 사진 고속 서빙)
- **Serverless**: Vercel Edge Functions (API & Cron Jobs)

### DevOps & Infrastructure
- **Monorepo**: **Turborepo**를 통한 패키지 관리 및 빌드 최적화
- **CI/CD**: GitHub Actions, Vercel, EAS (Expo Application Services)
- **Payments**: Toss Payments (Subscription Billing)

---

## 🏗️ Monorepo Directory Strategy

확장성과 코드 재사용성을 극대화하기 위해 모노레포 구조를 채택했습니다.

```text
hugroomer/
├── apps/
│   ├── web/          # Next.js 14 기반 미용샵 운영 대시보드 (B2B)
│   ├── mobile/       # React Native Expo 기반 보호자용 앱 (B2C)
│   └── landing/      # 서비스 소개 및 마케팅 페이지
├── packages/
│   ├── ui/           # 공통 디자인 시스템 (Shared Components)
│   ├── types/        # DB 스키마와 1:1 매칭되는 공통 TypeScript 타입 정의
│   └── utils/        # 날짜 연산, 포맷팅 등 순수 함수 유틸리티
└── supabase/
    ├── schema.sql    # Single Source of Truth (마스터 스키마)
    └── migrations/   # 버전 관리가 포함된 SQL 마이그레이션 전략
```

---

## 🔍 Core Engineering Highlights

### 1. 고밀도 예약 시각화 알고리즘 (Greedy Column Assignment)
**Challenge:** 여러 직원의 예약이 동일 시간대에 겹칠 때, UI가 중첩되지 않으면서도 가독성을 유지해야 했습니다.  
**Solution:** 겹치는 예약 그룹을 식별하고, 각 예약에 동적 열(Column) 인덱스를 부여하는 알고리즘을 구현했습니다. 이를 통해 예약 블록의 `width`와 `left` 값을 런타임에 계산하여 겹침 정도에 따라 유연하게 배치되도록 설계했습니다.

### 2. 멀티 테넌트 보안 아키텍처 (Row Level Security)
**Challenge:** B2B SaaS로서 매장 간 데이터가 섞이는 것은 치명적인 결함입니다.  
**Solution:** 애플리케이션 레벨의 필터링에 의존하지 않고, DB 레벨에서 **Supabase RLS** 정책을 적용했습니다. 모든 쿼리에 `auth.uid()`와 `staff.shop_id`를 검증하는 정책을 심어, 개발자의 실수로 인한 데이터 유출 가능성을 원천 차단했습니다.

### 3. AI 기반 비즈니스 자동화 (Claude 3.5 Sonnet Integration)
**Challenge:** 원장님들이 직접 마케팅 메시지나 돌봄 리포트를 작성하는 데 드는 공수를 줄여야 했습니다.  
**Solution:** Anthropic의 Claude API를 연동하여, 축적된 미용 데이터를 바탕으로 "친근한", "정중한", "간단한" 세 가지 톤의 재방문 메시지 초안을 자동 생성합니다. 또한 케어 일지를 요약하여 보호자에게 감동을 주는 리포트 자동화 기능을 구현했습니다.

### 4. 무지개다리 정책 (Data Empathy Design)
**Challenge:** 반려동물이 사망하거나 샵을 옮겨 데이터를 삭제할 때 발생하는 데이터 손실 문제.  
**Solution:** 단순 삭제가 아닌 `is_deleted` 기반의 **Soft Delete**와 **Grooming History Snapshot** 전략을 도입했습니다. 반려동물 정보가 삭제되더라도 보호자에게는 과거의 미용 사진과 추억을 제공하며, 샵의 매출 장부에는 당시의 기록이 그대로 보존되도록 설계하여 비즈니스 일관성과 사용자 감동을 동시에 잡았습니다.

### 5. 한글 IME 및 포커스 트랩 최적화
**Challenge:** Radix UI Dialog 사용 시 한글 조합 중 포커스가 소실되는 고질적인 UX 이슈 해결.  
**Solution:** `onFocusOutside` 이벤트를 인터셉트하고 IME 상태를 체크하는 커스텀 핸들러를 개발하여, 한국 사용자에게 최적화된 입력 경험을 제공합니다.

---

## 📈 Development History & Evolution

Hugroomer는 단순한 기능 구현을 넘어, 실제 운영 환경에서의 확장성과 유지보수성을 확보하기 위해 점진적인 아키텍처 개선을 거쳤습니다.

| 버전 | 주요 마일스톤 | 기술적 고찰 및 해결 방안 |
| :--- | :--- | :--- |
| **v0.1.0** | **Monorepo Foundation** | Web(Next.js)과 Mobile(Expo) 간의 **코드 중복을 최소화**하기 위해 Turborepo 기반 모노레포를 구축하고, 공통 타입 패키지를 통해 데이터 정합성을 확보함. |
| **v0.5.0** | **Scheduling Engine** | 1인샵 원장의 편의성을 위해 Gantt 차트 기반 타임라인을 구현함. SSR 환경에서의 **KST/UTC 시간대 일관성** 문제를 `mounted` 훅과 전용 시간 유틸리티로 해결함. |
| **v0.8.0** | **AI & Multi-care Logic** | 기존 개별 테이블(hotel_*)의 데이터 산재 문제를 해결하기 위해 **통합 케어 테이블(`care_*`)**로 마이그레이션함. Claude API를 연동하여 반복적인 문서 작성 업무를 자동화함. |
| **v0.9.5** | **Real-time & Security** | Supabase Realtime을 도입하여 별도의 소켓 서버 없이 실시간 채팅을 구현함. 비즈니스 로직 전반에 **RLS(Row Level Security)**를 적용하여 테넌트 간 보안을 강화함. |
| **v1.0.0** | **Build Optimization** | Vercel 및 EAS(Expo Application Services) 빌드 파이프라인을 구축함. **Android New Architecture**를 활성화하여 모바일 앱의 런타임 성능을 극대화함. |

### 💡 주요 설계 고찰 (Design Decisions)

1.  **왜 Supabase인가?**
    *   1인 개발 환경에서 인증, 실시간 통신, 스토리지 인프라 구축 비용을 최소화하고 비즈니스 로직 개발에 집중하기 위해 선택했습니다. 특히 PostgreSQL의 강력한 RLS 기능을 통해 보안 로직을 DB 레벨로 이관했습니다.

2.  **Server Actions vs API Routes**
    *   Next.js 14의 Server Actions를 적극 활용하여 클라이언트-서버 간의 타입 세이프티를 강화하고, API 엔드포인트 관리 공수를 줄였습니다. 복잡한 유효성 검사는 Zod와 결합하여 처리했습니다.

3.  **Cross-Platform UI Strategy**
    *   Web 대시보드는 생산성을 위해 `shadcn/ui`를, Mobile은 네이티브 성능을 위해 `React Native`를 사용하되, `packages/ui`에서 디자인 시스템의 핵심 로직과 테마를 공유하여 브랜드 아이덴티티를 유지했습니다.

---

## 🏗 시스템 아키텍처 및 DB 설계

Hugroomer는 확장성과 유지보수성을 고려하여 관계형 데이터베이스를 설계했습니다.

- **shops**: 테넌트 마스터 정보 및 요금제 관리
- **reservations**: 비즈니스 핵심 트랜잭션 및 타임라인 연동
- **care_logs**: Claude AI 연동을 통한 자동 리포트 생성 기반 데이터
- **chat_rooms**: Supabase Realtime 기반의 실시간 소통 인프라
- **Soft Delete**: 반려동물 삭제 시 `is_deleted` 처리로 과거 기록(추억 앨범) 보존 정책 적용

*※ 상세 DB ERD 및 API 명세서는 내부 문서로 관리 중이며, 요청 시 별도 제공 가능합니다.*

---

## 🛡️ 소스코드 보호 및 이용 안내 (Source Code Policy)

### 1. 상세 소스코드 비공개 (Private Repository)
본 저장소의 상세 구현 소스코드(Server Actions, 컴포넌트 내부 로직, API 연동부 등)는 **지식재산권 보호 및 보안상의 이유로 비공개(Private)**로 관리되고 있습니다. 본 `README_PUBLIC.md`는 개발자의 아키텍처 설계 역량과 기술적 문제 해결 방안을 보여주기 위한 요약 문서입니다.

### 2. 저작권 보호
본 프로젝트의 기획, UI/UX 디자인, 고유 알고리즘, 데이터베이스 스키마 설계 및 비즈니스 모델 아이디어는 개발자 개인의 창작물입니다. **무단 전재, 복제, 상업적 이용 및 재배포를 엄격히 금지합니다.**

### 3. 기술 문의
아키텍처 설계나 특정 기술 구현 방식에 대한 상세한 논의가 필요하신 경우, 아래 연락처를 통해 문의해 주시기 바랍니다.

---

## 📬 Contact & Portfolio

- **Email**: yoonani2720@naver.com
- **LinkedIn**: https://www.linkedin.com/in/hayunan/
- **GitHub**: https://github.com/HAYUNAN

<p align="center">© 2026 Hugroomer. All rights reserved.</p>
