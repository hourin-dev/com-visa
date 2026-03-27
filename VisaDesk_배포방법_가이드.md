# VisaDesk 외부 배포 방법 가이드
> 법무법인 대림 | VisaDesk 운영 실무 가이드

---

## 📌 배포 방식 3가지 비교

| 방식 | 난이도 | 비용 | 추천 시점 | 특징 |
|------|--------|------|-----------|------|
| **① Netlify (HTML 무료 호스팅)** | ⭐ 쉬움 | 무료 | 1차 HTML 데모 운영 | 파일 업로드만 하면 끝 |
| **② Railway (백엔드 포함)** | ⭐⭐ 중간 | 월 $5~20 | 2차 버전 (DB 포함) | FastAPI + PostgreSQL 올인원 |
| **③ VPS 직접 운영 (가비아/카페24)** | ⭐⭐⭐ 어려움 | 월 2~5만원 | 정식 운영 | 완전 제어, 보안 최적화 |

---

## 방법 1️⃣ Netlify — HTML 파일 즉시 배포 (1차 버전)

### 특징
- 회원가입만 하면 HTML 파일 드래그&드롭으로 배포 완료
- HTTPS 자동 적용
- 무료 도메인 예: `visadesk-daelim.netlify.app`

### 배포 순서
```
1. https://netlify.com 접속 → 무료 회원가입
2. 대시보드에서 "Add new site" → "Deploy manually"
3. VisaDesk_v1.html 파일을 드래그앤드롭
4. 자동으로 URL 생성됨 (예: abc123.netlify.app)
5. Site settings → Change site name → "visadesk-daelim" 으로 변경
```

### 도메인 연결 (선택)
```
- 가비아에서 도메인 구매 (예: visadesk.kr → 연 1~2만원)
- Netlify → Domain settings → Add custom domain
- DNS 설정을 가비아에서 Netlify 쪽으로 변경
```

---

## 방법 2️⃣ Railway — 백엔드+DB 포함 전체 배포 (2차 버전 권장)

### 특징
- FastAPI(파이썬) + PostgreSQL 을 한 플랫폼에서 운영
- GitHub 연동 → 코드 수정하면 자동 배포
- 한국 서버 선택 가능 (속도 빠름)
- 무료 $5 크레딧 제공, 이후 사용량에 따라 과금

### 배포 순서
```
1. https://railway.app 접속 → GitHub 계정으로 가입
2. "New Project" → "Deploy from GitHub repo"
3. VisaDesk 코드를 GitHub에 올린 후 연결
4. "Add PostgreSQL" 클릭 → DB 자동 생성
5. 환경변수 설정 (DATABASE_URL 자동 주입됨)
6. 도메인 자동 생성: visadesk-daelim.up.railway.app
```

---

## 방법 3️⃣ VPS 직접 운영 — 정식 서비스용

### 추천 서비스
| 서비스 | 가격 | 특징 |
|--------|------|------|
| **가비아 클라우드** | 월 2~3만원 | 국내, 한국어 지원 |
| **카페24 호스팅** | 월 1~2만원 | 국내, 비교적 저렴 |
| **Vultr/DigitalOcean** | 월 $6~ | 해외, 안정적 |

### 서버 구성 (Ubuntu 22.04 기준)
```bash
# 1. 서버 접속 후 기본 설치
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip nginx postgresql -y

# 2. 앱 배포
git clone https://github.com/your-repo/visadesk
cd visadesk
pip install -r requirements.txt

# 3. 서비스 등록
sudo systemctl enable visadesk
sudo systemctl start visadesk

# 4. Nginx 설정 (리버스 프록시)
# /etc/nginx/sites-available/visadesk 파일 설정

# 5. SSL 인증서 (HTTPS) - 무료
sudo apt install certbot
sudo certbot --nginx -d visadesk.kr
```

---

## 🔐 운영 전 필수 보안 체크리스트

### 외국인 개인정보 처리 관련 (중요!)
- [ ] **개인정보처리방침 페이지** 필수 게시 (개인정보보호법)
- [ ] 여권번호, 외국인등록번호는 **암호화 저장** (AES-256)
- [ ] SSL/HTTPS 필수 적용
- [ ] 관리자 비밀번호 영문+숫자+특수문자 8자 이상
- [ ] 로그인 실패 5회 시 계정 잠금
- [ ] 접속 로그 최소 6개월 보관

### 서비스 안정성
- [ ] 매일 자동 DB 백업 설정
- [ ] 업타임 모니터링 (UptimeRobot 무료 사용 가능)
- [ ] 에러 알림 이메일 설정

---

## 📱 추천 배포 시나리오 (단계별)

```
현재 (1차 데모)
  → Netlify 무료 배포로 내부 테스트
  → URL 공유해서 업체 파일럿 테스트

2~3개월 후 (2차 정식)
  → Railway에 FastAPI + PostgreSQL 배포
  → 도메인 구매 (visadesk.kr 등)
  → 업체별 계정 발급 운영 시작

6개월 후 (안정화)
  → 국내 VPS로 이전 (가비아 클라우드)
  → 전용 서버, 보안 강화, 백업 자동화
```

---

## 💡 가장 빠른 시작 방법 (오늘 바로)

1. [netlify.com](https://netlify.com) 접속
2. 무료 가입
3. VisaDesk_v1.html 드래그앤드롭
4. **5분 만에 외부 접속 가능한 URL 생성 완료**

---

*법무법인 대림 내부 문서 | 기술 문의: 사무장실*
