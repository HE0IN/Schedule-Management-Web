# 📊 팀 일정 관리 시스템 (Team Schedule Manager)

Streamlit 기반의 팀별 프로젝트 및 작업 일정 관리 웹 애플리케이션입니다.  
프로젝트, Main Task, Sub Task를 계층적으로 관리하고, 작업 시작일 하루 전 담당자에게 자동으로 이메일 알림을 발송합니다.

## ✨ 주요 기능

- **팀별 일정 관리**: Team_1, Team_2, Team_3 각 팀별 독립적인 프로젝트 관리
- **계층적 작업 구조**: 프로젝트 → Main Task → Sub Task 3단계 관리
- **실시간 데이터 편집**: Streamlit `data_editor`를 활용한 인라인 CRUD
- **자동 날짜 계산**: Main Task 시작일 기준 offset_days로 Sub Task 일정 자동 계산
- **이메일 알림**: APScheduler를 통해 매일 오전 6시 다음날 시작 작업 담당자에게 알림 발송
- **팀원 명단 관리**: 사이드바에서 팀별 멤버 확인

## 🛠 기술 스택

| 구분 | 기술 |
|------|------|
| Frontend | Streamlit |
| Backend | Python |
| Database | MySQL |
| DB Connector | PyMySQL |
| Scheduler | APScheduler |
| Email | smtplib (SMTP) |
| Config | python-dotenv |

## 📁 프로젝트 구조

```
├── Home.py              # 메인 엔트리포인트, 전체 데이터 조회
├── config.py            # DB 연결, 공통 함수, 스케줄러 설정
├── day_email.py         # 이메일 알림 기능
├── Team_1_page.py       # Team_1 일정 관리 페이지
├── Team_2_page.py       # Team_2 일정 관리 페이지
├── Team_3_page.py       # Team_3 일정 관리 페이지
├── DB_START.sql         # 데이터베이스 초기화 스크립트
├── .env                 # 환경 변수 (DB, SMTP 설정)
└── README.md
```

## 🗄 데이터베이스 구조

```
schedule_groups (팀/그룹)
    └── schedule_employees (직원)
    └── schedule_projects (프로젝트)
            └── schedule_Main_Task (메인 작업)
            └── schedule_Sub_Task (세부 작업)
```

## ⚙️ 설치 및 실행

### 1. 의존성 설치

```bash
pip install streamlit pandas pymysql python-dotenv apscheduler
```

### 2. 환경 변수 설정

`.env` 파일을 생성하고 아래 내용을 설정합니다:

```env
# Database
dbhost="localhost"
user="root"
password="your_password"
database="your_database"
charset="utf8mb4"

# Email (SMTP)
smtp_server="smtp.your-provider.com"
smtp_port="465"
email_address="your_email@example.com"
email_password="your_app_password"
```

### 3. 데이터베이스 초기화

```bash
mysql -u root -p < DB_START.sql
```

### 4. 애플리케이션 실행

```bash
streamlit run Home.py
```

## 📸 화면 구성

### Home
- 전체 그룹, 인원, 프로젝트, Main Task, Sub Task 조회/편집

### Team 페이지 (Team_1, Team_2, Team_3)
- 프로젝트 추가/삭제
- 프로젝트별 탭으로 Main Task & Sub Task 관리
- 저장 버튼으로 DB 반영

## 📧 이메일 알림 기능

- **발송 시점**: 매일 오전 6시
- **발송 대상**: 다음날(D+1) 시작되는 Sub Task 담당자
- **발송 내용**: 프로젝트명, 작업명, 시작일 안내

## 📝 참고사항

- Streamlit의 `data_editor`는 행 추가/삭제 시 즉시 반영되지 않고, 저장 버튼을 눌러야 DB에 반영됩니다.
- 프로젝트 삭제 시 관련 Main Task, Sub Task가 함께 삭제됩니다.
- Sub Task의 `calculated_start_date`는 Main Task 시작일 + offset_days로 자동 계산됩니다.

## 🔧 향후 개선 사항

- [ ] 간트 차트 시각화 추가
- [ ] 사용자 인증/로그인 기능
- [ ] 팀 동적 추가/삭제 기능
- [ ] 중복 코드 리팩토링 (Team 페이지 통합)

## 📄 License

MIT License#