# 우리집 노트 (AH Note Share)

## 📋 프로젝트 개요

우리집 노트는 요양원 직원들을 위한 노트 공유 시스템입니다. 직원들이 센터장 전달사항, 개인 메모, 전체 공지사항 등을 효율적으로 작성하고 관리할 수 있도록 설계된 웹 애플리케이션입니다.

### 🎯 주요 목적
- 요양원 내 정보 공유 체계화
- 직원 간 효율적인 소통 지원
- 업무 관련 노트 및 메모 중앙화 관리
- 권한별 정보 접근 제어

## ✨ 주요 기능

### 📝 노트 작성 및 관리
- **구분별 노트 작성**: 센터장, 개인, 전체 카테고리
- **자동 노트번호 생성**: `직원번호-날짜-순번` 형식
- **태그 시스템**: 회원상담, 보호자요청, 회원요청, 회원이슈, 준비사항 등
- **회원상담 템플릿**: 회원상담 태그 선택 시 자동 템플릿 삽입
- **게시여부 설정**: 공개/비공개 설정 가능
- **모바일 최적화**: 터치 친화적 입력 및 자동 스크롤 기능

### 🔍 검색 및 필터링
- **날짜 범위 검색**: 시작일~종료일 설정 (기본 2주 범위)
- **다중 조건 검색**: 태그, 내용, 작성자명으로 검색
- **실시간 필터링**: 게시만/전체 보기 토글
- **테이블 정렬**: 컬럼별 오름차순/내림차순 정렬
- **클라이언트 사이드 필터링**: 빠른 응답성을 위한 로컬 필터링

### 👥 권한 관리
- **일반 직원**: 본인 작성 노트 + 전체 공개 노트 조회
- **센터장(s25001)**: 모든 센터장 노트 + 본인 개인 노트 + 전체 노트 조회
- **관리자(admin)**: 모든 노트 조회 및 관리 권한

### 📱 반응형 디자인
- **데스크톱 버전**: 좌우 분할 레이아웃으로 효율적인 작업 환경
- **모바일 버전**: 터치 친화적인 카드 형태의 노트 리스트
- **자동 디바이스 감지**: 화면 크기에 따른 최적화된 UI 제공
- **모바일 UX 개선**: 
  - 더블클릭으로 편집 모드 진입 (실수 클릭 방지)
  - 노트 내용 입력 시 자동 스크롤
  - 일관된 입력 필드 높이

## 🏗️ 기술 스택

### Frontend
- **HTML5**: 시맨틱 마크업
- **CSS3**: 
  - CSS Variables를 활용한 일관된 디자인 시스템
  - Flexbox 및 Grid 레이아웃
  - 반응형 미디어 쿼리
  - 커스텀 스크롤바 및 애니메이션
- **Vanilla JavaScript**: 
  - ES6+ 문법 활용
  - Promise 기반 비동기 처리
  - 이벤트 위임 패턴
  - 모듈화된 함수 구조

### Backend & Database
- **Supabase**: 
  - PostgreSQL 데이터베이스
  - Real-time 구독 기능
  - Row Level Security (RLS)
  - RESTful API 자동 생성

### 보안
- **Service Role Key 사용**: 데이터베이스 직접 접근
- **클라이언트 사이드 권한 검증**: 다중 권한 레벨 지원
- **URL 민감정보 자동 제거**: 보안 강화 기능

## 📁 프로젝트 구조

```
AH_noteshare/
├── index.html              # 데스크톱 메인 페이지
├── index_m.html            # 모바일 전용 페이지
├── styles.css              # 데스크톱 스타일시트
├── styles_m.css            # 모바일 스타일시트
├── supabase.js             # 데이터베이스 연동 스크립트
└── README.md               # 프로젝트 문서
```

## 🗄️ 데이터베이스 스키마

### noteshare 테이블
```sql
CREATE TABLE noteshare (
    id SERIAL PRIMARY KEY,
    직원번호 VARCHAR(50) NOT NULL,
    직원명 VARCHAR(100),
    구분 VARCHAR(20) NOT NULL,        -- '센터장', '개인', '전체'
    노트날짜 VARCHAR(8) NOT NULL,      -- YYYYMMDD 형식
    노트번호 VARCHAR(100) NOT NULL,    -- 직원번호-날짜-순번
    태그 TEXT,
    노트내용 TEXT NOT NULL,
    게시여부 VARCHAR(1) DEFAULT 'Y',   -- 'Y' 또는 'N'
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### employeesinfo 테이블
```sql
CREATE TABLE employeesinfo (
    직원번호 VARCHAR(50) PRIMARY KEY,
    직원명 VARCHAR(100) NOT NULL
);
```

## 🔧 설치 및 설정

### 1. 프로젝트 클론
```bash
git clone [repository-url]
cd AH_noteshare
```

### 2. Supabase 설정
1. [Supabase](https://supabase.com)에서 새 프로젝트 생성
2. `supabase.js` 파일에서 URL과 API 키 설정
```javascript
const SUPABASE_URL = "your-supabase-url";
const SUPABASE_KEY = "your-service-role-key";
```

### 3. 데이터베이스 테이블 생성
- Supabase SQL 에디터에서 위의 스키마 실행
- 필요한 초기 직원 데이터 입력

### 4. 웹 서버 실행
```bash
# 로컬 서버 실행 (예: Live Server, http-server 등)
npx http-server .
# 또는
python -m http.server 8000
```

## 🖥️ 화면 구성

### 데스크톱 버전 (index.html)
```
┌─────────────────────────────────────────────────────────┐
│ 헤더 (제목, 모바일버전 버튼, 사용자 정보)                      │
├─────────────────┬───────────────────────────────────────┤
│ 좌측 섹션        │ 우측 섹션                              │
│ ┌─────────────┐ │ ┌───────────────────────────────────┐ │
│ │ 검색 영역    │ │ │ 노트 작성/편집 폼                   │ │
│ └─────────────┘ │ │                                   │ │
│ ┌─────────────┐ │ │ - 구분 선택                        │ │
│ │ 노트 테이블  │ │ │ - 날짜/번호                        │ │
│ │             │ │ │ - 태그 입력                        │ │
│ │ (정렬 가능)  │ │ │ - 내용 작성                        │ │
│ │             │ │ │ - 게시여부                         │ │
│ │             │ │ │ - 버튼(초기화/저장/수정/삭제)         │ │
│ └─────────────┘ │ └───────────────────────────────────┘ │
└─────────────────┴───────────────────────────────────────┘
```

### 모바일 버전 (index_m.html)
```
┌─────────────────────────────┐
│ 네비게이션 바                │
│ [노트작성/편집] [노트조회]    │
├─────────────────────────────┤
│ 헤더 (제목, 사용자 정보)      │
├─────────────────────────────┤
│ 활성 페이지 영역             │
│                             │
│ 노트조회 페이지:             │
│ ┌─────────────────────────┐ │
│ │ 검색 컨트롤              │ │
│ └─────────────────────────┘ │
│ ┌─────────────────────────┐ │
│ │ 노트 카드 리스트         │ │
│ │ (터치 친화적)            │ │
│ └─────────────────────────┘ │
│                             │
│ 노트작성 페이지:             │
│ ┌─────────────────────────┐ │
│ │ 작성 폼                  │ │
│ └─────────────────────────┘ │
└─────────────────────────────┘
```

## 🎨 디자인 시스템

### 색상 팔레트
```css
:root {
    --main-color: #235135;      /* 진한 초록색 (메인) */
    --light-color: #e9f2ef;     /* 연한 민트색 (배경) */
    --accent-color: #5a8f7b;    /* 중간 초록색 (포인트) */
    --border-color: #cccccc;    /* 회색 (테두리) */
    --danger-color: #dc3545;    /* 빨간색 (삭제) */
    --success-color: #28a745;   /* 녹색 (저장) */
}
```

### 타이포그래피
- **Primary Font**: "Noto Sans KR", "맑은 고딕", "Malgun Gothic"
- **Font Sizes**: 0.7rem ~ 1.5rem (반응형 조정)
- **Font Weights**: 300, 400, 500, 700

### 컴포넌트
- **버튼**: 다양한 상태별 스타일 (hover, active, disabled)
- **입력 필드**: 포커스 시 테두리 강조 효과
- **모달**: 부드러운 fade-in 애니메이션
- **태그**: 동적 추가/제거 가능한 칩 형태

## 🔑 핵심 기능 설명

### 1. 권한 기반 데이터 접근
```javascript
// 구분별 조회 규칙
if (currentUserNumber === 'admin' || currentUserNumber === 's25001') {
    // 센터장/관리자: 모든 노트 접근 가능
    query = query.or('구분.eq.전체,구분.eq.센터장,and(구분.eq.개인,직원번호.eq.${currentUserNumber})');
} else {
    // 일반 직원: 제한적 접근
    query = query.or('구분.eq.전체,and(구분.eq.개인,직원번호.eq.${currentUserNumber})');
}
```

### 2. 모바일 더블클릭 시스템
```javascript
// 더블클릭으로 편집 모드 진입
noteCard.addEventListener('dblclick', () => {
    selectNoteForEdit(note);
});

// 싱글클릭으로 선택만
noteCard.addEventListener('click', () => {
    noteCard.classList.add('selected');
});
```

### 3. 자동 노트번호 생성
```javascript
// 형식: 직원번호-YYYYMMDD-001
async function generateNoteNumber(employeeNumber, noteDate) {
    // 해당 직원의 동일 날짜 노트 개수 조회 후 시퀀스 증가
    return `${employeeNumber}-${formattedDate}-${sequenceStr}`;
}
```

### 4. 모바일 자동 스크롤
```javascript
// 텍스트 영역 포커스 시 자동 스크롤
mainContentTextarea.addEventListener('focus', function() {
    setTimeout(() => {
        scrollToBottomSmooth();
    }, 500); // 키보드 올라온 후 스크롤
});

// 뷰포트 변경 감지로 키보드 대응
window.addEventListener('resize', function() {
    if (window.innerHeight < initialViewportHeight * 0.8) {
        scrollToBottomSmooth();
    }
});
```

### 5. 날짜 범위 최적화
```javascript
// 기본 2주 범위로 성능 최적화
function initializeDateRange() {
    const today = getKoreaDateString();
    const twoWeeksAgo = getKoreaDateString(getDateDaysAgo(14));
    return { startDate: twoWeeksAgo, endDate: today };
}
```

### 6. 태그 시스템
```javascript
// 동적 태그 추가/제거
function setupTagInput(inputId, chipsId, hiddenId) {
    // Enter 키 또는 쉼표로 태그 추가
    // 백스페이스로 태그 제거
    // 칩 형태로 시각적 표현
}
```

## 📱 모바일 최적화

### 터치 인터페이스
- **터치 친화적 버튼 크기**: 최소 44px
- **스와이프 제스처**: 가로 스크롤 지원
- **햅틱 피드백**: -webkit-tap-highlight-color 제거
- **더블탭 방식**: 실수 클릭 방지를 위한 더블클릭 편집

### 성능 최적화
- **이미지 최적화**: SVG 아이콘 사용
- **CSS 애니메이션**: GPU 가속 transform 활용
- **지연 로딩**: 필요시에만 데이터 로드
- **데이터 최적화**: 기본 2주 범위로 초기 로딩 속도 향상
- **스마트 스크롤**: 키보드 올라옴 감지 및 자동 스크롤

### 반응형 브레이크포인트
```css
/* 모바일 */
@media (max-width: 767px) { ... }

/* 태블릿 */
@media (min-width: 768px) { ... }

/* 데스크톱 */
@media (min-width: 1024px) { ... }
```

## 🔒 보안 고려사항

### 데이터 보호
- **Service Role Key 사용**: 클라이언트에서 직접 DB 접근
- **권한별 데이터 필터링**: 서버 레벨에서 권한 검증
- **민감정보 URL 제거**: 자동 파라미터 정리

### 사용자 인증
```javascript
// 다중 방식 사용자 정보 획득
async function getUserFromParent() {
    // 1. sessionStorage 확인
    // 2. DOM 요소에서 추출
    // 3. parent window 접근
    // 4. URL 파라미터 확인
    // 5. localStorage 백업
}
```

## 🧪 테스트 및 디버깅

### 디버깅 도구
```javascript
// 직원 캐시 디버깅
window.debugEmployeeCache = function() {
    console.log('현재 사용자:', currentUser);
    console.log('캐시 상태:', employeesCacheLoaded);
    // 상세 디버깅 정보 출력
};
```

### 로그 시스템
- **상세한 콘솔 로그**: 각 단계별 상태 추적
- **에러 핸들링**: 사용자 친화적 에러 메시지
- **성능 모니터링**: 데이터 로딩 시간 측정

## 🚀 배포 가이드

### 1. 프로덕션 빌드 준비
```bash
# 코드 압축 및 최적화
npm run build
```

### 2. 환경 변수 설정
```javascript
// production 환경용 설정
const SUPABASE_URL = process.env.SUPABASE_URL;
const SUPABASE_KEY = process.env.SUPABASE_KEY;
```

### 3. 웹 서버 설정
```nginx
# Nginx 설정 예시
server {
    listen 80;
    server_name your-domain.com;
    root /path/to/AH_noteshare;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## 📈 성능 최적화

### 캐싱 전략
- **직원 정보 캐싱**: 메모리 캐시로 반복 요청 방지
- **노트 데이터 캐싱**: 날짜 범위별 캐싱
- **브라우저 캐싱**: CSS/JS 파일 캐시 헤더 설정

### 데이터베이스 최적화
```sql
-- 인덱스 생성
CREATE INDEX idx_noteshare_employee_date ON noteshare(직원번호, 노트날짜);
CREATE INDEX idx_noteshare_category ON noteshare(구분);
CREATE INDEX idx_noteshare_publish ON noteshare(게시여부);
```

## 🔧 문제 해결

### 자주 발생하는 문제

1. **로그인 정보 인식 안됨**
   ```javascript
   // 해결: 다중 소스에서 사용자 정보 획득
   await getUserFromParent();
   ```

2. **노트 번호 중복**
   ```javascript
   // 해결: 트랜잭션 처리로 동시성 제어
   await generateNoteNumber(employeeNumber, noteDate);
   ```

3. **모바일 입력 필드 높이 불일치**
   ```css
   /* 해결: 명시적 높이 지정 */
   .form-group input {
       height: 44px;
       box-sizing: border-box;
   }
   ```

4. **모바일 키보드로 인한 입력 영역 가림**
   ```javascript
   // 해결: 자동 스크롤 및 뷰포트 변경 감지
   window.addEventListener('resize', handleKeyboardToggle);
   ```

5. **모바일에서 실수 클릭**
   ```javascript
   // 해결: 더블클릭 방식으로 편집 모드 진입
   noteCard.addEventListener('dblclick', selectNoteForEdit);
   ```

## 📞 지원 및 연락처

### 기술 지원
- **이슈 리포팅**: [GitHub Issues]
- **문서 업데이트**: 정기적인 README 갱신
- **버전 관리**: Semantic Versioning 적용

### 기여 가이드라인
1. Fork 프로젝트
2. Feature 브랜치 생성
3. 코드 작성 및 테스트
4. Pull Request 제출

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 🔄 업데이트 히스토리

### v1.2.0 (2024-12-XX)
- **모바일 UX 대폭 개선**
  - 더블클릭으로 편집 모드 진입 (실수 클릭 방지)
  - 노트 내용 입력 시 자동 스크롤 기능
  - 일관된 입력 필드 높이 (44px)
- **성능 최적화**
  - 기본 날짜 범위 30일 → 14일로 변경
  - 클라이언트 사이드 필터링 개선
- **데스크톱 반응형 개선**
  - 라디오 버튼 가로 스크롤 지원
  - 입력 필드 크기 동일 비율 조정

### v1.1.0 (2024-12-XX)
- **커스텀 모달 시스템 도입**
  - 네이티브 alert/confirm 대체
  - 모바일 친화적 디자인
- **보안 기능 강화**
  - URL 민감정보 자동 제거
  - 다중 사용자 정보 소스 지원

### v1.0.0 (2024-XX-XX)
- 초기 릴리스
- 기본 노트 작성/조회 기능
- 권한별 데이터 접근 제어
- 반응형 디자인 적용

---

**우리집 노트**는 요양원 직원들의 효율적인 업무 소통을 위해 지속적으로 개선되고 있습니다.

