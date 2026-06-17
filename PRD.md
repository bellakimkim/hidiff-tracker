# HIDIF 피드백 트래커 — PRD

## 1. 프로젝트 개요
- **프로젝트명**: HIDIF 피드백 트래커
- **목적**: 제품별 내용물 / 디자인 / 부자재 샘플링 차수와 피드백을 팀 공용으로 관리
- **스택**: HTML + CSS + JS (Vanilla) / Supabase (DB) / Vercel (배포)
- **접근**: 로그인 없이 URL 접속만으로 팀 전원 사용

---

## 2. 핵심 기능

### 2-1. 제품 관리
- 제품 등록 / 삭제
- 카테고리: 스킨케어 / 라이프스타일 / 메이크업 / 헤어케어
- 날짜 4개 필드: 런칭일 / 런칭 목표일 / 발주 예정일 / 발주일
- 타겟 제품 (기획 의도)
- 주요 성분 및 특이사항

### 2-2. 샘플링 트랙 (제품당 3개, 독립 운영)
| 트랙 | 특이사항 |
|------|---------|
| 내용물 | 피드백을 신디 / 팀 2칸으로 분리, 랩 넘버 입력 |
| 디자인 | 단일 피드백 |
| 부자재 | 단일 피드백 |

- 각 트랙 상태: 대기 → 진행중 → 완료 (클릭으로 순환)
- 차수별 기록: 차수 / 수령일 / 피드백 / 개선 액션
- 타임라인 형태로 누적 표시

### 2-3. 통계
- 상단 3개 카드: 내용물 / 디자인 / 부자재 트랙별 진행중 / 완료 / 대기 현황

### 2-4. 데이터
- Supabase PostgreSQL에 저장
- 팀원 누구나 실시간으로 같은 데이터 조회 / 입력

---

## 3. DB 스키마

### products 테이블
| 컬럼 | 타입 | 설명 |
|------|------|------|
| id | uuid | PK, auto |
| name | text | 제품명 |
| category | text | 카테고리 |
| launch | date | 런칭일 |
| launch_target | date | 런칭 목표일 |
| order_plan | date | 발주 예정일 |
| order_date | date | 발주일 |
| goal | text | 타겟 제품 |
| notes | text | 주요 성분 및 특이사항 |
| created_at | timestamptz | 생성일 |

### rounds 테이블
| 컬럼 | 타입 | 설명 |
|------|------|------|
| id | uuid | PK, auto |
| product_id | uuid | FK → products.id |
| track | text | contents / design / material |
| track_status | text | 대기 / 진행중 / 완료 |
| round_label | text | 차수 (예: 1차) |
| date | date | 수령일 |
| lab_number | text | 랩 넘버 (내용물만) |
| fb_sindy | text | 신디 피드백 (내용물만) |
| fb_team | text | 팀 피드백 (내용물만) |
| feedback | text | 피드백 (디자인/부자재) |
| result | text | 개선 액션 |
| created_at | timestamptz | 생성일 |

---

## 4. 파일 구조
```
hidif-tracker/
├── index.html        # 메인 (전체 UI + JS)
├── supabase.js       # Supabase 클라이언트 설정
└── .env              # 환경변수 (키값, Git에 올리지 않음)
```

---

## 5. 배포
- **GitHub**: hidif-tracker 레포지토리
- **Vercel**: GitHub 연동 자동 배포
- **환경변수 (Vercel에 입력)**:
  - `SUPABASE_URL`
  - `SUPABASE_ANON_KEY`
