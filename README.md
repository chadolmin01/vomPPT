# V.O.M (Voice of Mother)

<div align="center">
  <h3>글을 모르는 엄마를 위한 음성 기반 육아 도우미</h3>
  <p>Hult Prize 2026 해커톤 프로젝트</p>

  [![Vercel](https://img.shields.io/badge/Web-Deployed-success?logo=vercel)](https://vom-web.vercel.app)
  [![Flutter](https://img.shields.io/badge/Flutter-3.x-blue?logo=flutter)](https://flutter.dev)
  [![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
</div>

---

## 📖 프로젝트 소개

V.O.M은 글을 읽지 못하는 엄마들을 위한 음성 기반 육아 도우미 앱입니다.
NFC/QR 카드를 스캔하면 음성으로 물건 사용법을 안내하고, AI 챗봇이 육아 질문에 답변합니다.

### 🎯 해결하는 문제
- 글을 읽지 못해 약 설명서, 제품 사용법을 이해하기 어려운 엄마들
- 응급 상황에서 빠른 대응이 필요한 순간
- 육아 지식 접근성 문제

### 💡 핵심 기능
1. **NFC/QR 스캔** → 음성 안내 (체온계, 기저귀, 분유 등)
2. **AI 음성 챗봇** → 육아 질문 답변 (Gemini AI)
3. **단계별 그래픽 애니메이션** → 시각적 학습 지원
4. **위험 키워드 감지** → 실시간 알림 시스템

---

## 🏗️ 프로젝트 구조

```
VOM/
├── vom-web/                    # Next.js 관리자 대시보드
│   ├── src/
│   │   ├── app/               # App Router
│   │   └── components/        # React 컴포넌트
│   ├── package.json
│   └── .env.local             # 환경 변수 (생성 필요)
│
├── vom_user_flutter/          # 사용자/가족용 앱
│   ├── lib/
│   │   ├── screens/          # 화면 (Home, Learning, VoiceChat 등)
│   │   ├── services/         # 서비스 (AI, TTS, NFC, Supabase)
│   │   └── constants/        # 상수, 색상, 콘텐츠
│   ├── android/              # Android 설정
│   └── pubspec.yaml          # Flutter 의존성
│
├── vom_admin_flutter/         # 관리자용 앱
│   ├── lib/
│   │   ├── screens/          # Dashboard, Users 등
│   │   └── services/         # Supabase 서비스
│   └── pubspec.yaml
│
├── VOM_User_v2.apk           # 사용자 앱 (63MB)
├── VOM_Admin_v2.apk          # 관리자 앱 (17MB)
├── supabase_schema.sql       # DB 스키마
├── DEPLOYMENT.md             # 배포 가이드
└── GEMINI_API_SETUP.md       # AI 설정 가이드
```

---

## 🚀 빠른 시작

### 1️⃣ 저장소 Clone
```bash
git clone https://github.com/chadolmin01/VOM.git
cd VOM
```

### 2️⃣ 웹 대시보드 (vom-web)

#### 설치
```bash
cd vom-web
npm install
```

#### 환경 변수 설정
`.env.local` 파일 생성:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
```

#### 실행
```bash
npm run dev
# http://localhost:3000
```

#### 배포
```bash
npm run build
vercel --prod
```

### 3️⃣ Flutter 사용자 앱 (vom_user_flutter)

#### 설치
```bash
cd vom_user_flutter
flutter pub get
```

#### API 키 설정
`lib/services/ai_chat_service.dart` 파일 수정:
```dart
static const String _apiKey = 'YOUR_GEMINI_API_KEY';
```

**⚠️ 중요**: GitHub에 공개된 키(`AIzaSyBT1psDP7a2AqYPTzWW_tAyrsbKAbvBRNY`)는 보안상 교체 필요
- [Google AI Studio](https://aistudio.google.com/app/apikey)에서 새 키 발급

#### 실행
```bash
# 개발 모드
flutter run

# APK 빌드
flutter build apk --release
# 출력: build/app/outputs/flutter-apk/app-release.apk
```

### 4️⃣ Flutter 관리자 앱 (vom_admin_flutter)

```bash
cd vom_admin_flutter
flutter pub get
flutter run
```

---

## 🛠️ 기술 스택

### 웹 (vom-web)
- **Framework**: Next.js 14.2.35 (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Database**: Supabase (PostgreSQL)
- **Deployment**: Vercel
- **Features**:
  - 실시간 구독 (Supabase Realtime)
  - 서버 컴포넌트
  - 반응형 디자인

### 모바일 (Flutter)
- **Framework**: Flutter 3.x
- **Language**: Dart
- **주요 패키지**:
  - `google_generative_ai` - Gemini AI 챗봇
  - `flutter_nfc_kit` - NFC 태그 읽기 (백그라운드 지원)
  - `mobile_scanner` - QR 코드 스캔
  - `speech_to_text` - 음성 인식 (STT)
  - `flutter_tts` - 음성 출력 (TTS)
  - `supabase_flutter` - 백엔드 연동
  - `audioplayers` - 오디오 재생
  - `record` - 음성 녹음

### 백엔드
- **Database**: Supabase (PostgreSQL)
- **Storage**: Supabase Storage
- **Realtime**: Supabase Realtime Subscriptions
- **Tables**:
  - `users` - 사용자 정보
  - `card_contents` - 학습 콘텐츠
  - `nfc_card_mappings` - NFC/QR 매핑
  - `learning_logs` - 학습 기록
  - `risk_keywords` - 위험 키워드

---

## 📱 주요 기능 상세

### 1. NFC/QR 스캔 시스템

#### 포그라운드 스캔
- 앱 실행 중 NFC 카드 태그 또는 QR 스캔
- 즉시 학습 화면으로 전환 (다이얼로그 없음)

#### 백그라운드 NFC (Android only)
- 앱이 백그라운드에 있을 때도 NFC 감지
- 시스템이 자동으로 앱 실행 → 학습 화면
- 구현 위치: `vom_user_flutter/android/`
  - `AndroidManifest.xml` - NFC intent-filter
  - `MainActivity.kt` - NFC Intent 처리
  - `lib/services/nfc_intent_service.dart` - Flutter 연동

### 2. 음성 기반 학습

#### TTS (Text-to-Speech)
- 한국어 음성 출력
- 속도: 0.4배 (천천히, 또박또박)
- 단계별 스크립트 재생

#### 단계별 애니메이션
- 이모지 기반 시각적 가이드
- 예시 (체온계): 🌡️ → 💪 → ⏱️ → ✅
- 현재 단계 강조, 완료 체크 표시

#### 학습 흐름
```
1. Loading   → 준비 중
2. Playing   → TTS 음성 + 애니메이션
3. Recording → 따라 말하기 (선택)
4. Quiz      → 퀴즈 (선택)
5. Complete  → 학습 완료
```

### 3. AI 음성 챗봇 (Gemini)

#### 기능
- 육아 관련 질문 답변 (음성 입력/출력)
- 컨텍스트 인식: 현재 학습 중인 카드 내용 반영
- 주제 제한: 육아/건강/아이돌봄만 답변

#### 사용 방법
1. 홈 화면 또는 학습 화면에서 로봇 아이콘(🤖) 클릭
2. 마이크 버튼 **꾹 누르고** 말하기
3. 버튼에서 손 떼면 자동 인식
4. AI가 음성으로 답변

#### 오프라인 모드
- API 키 없어도 키워드 기반 응답 가능
- 예시:
  - "열이 많아요" → 해열제 안내
  - "숨 못 쉬어요" → 119 전화 안내

### 4. 관리자 대시보드

#### 실시간 모니터링
- 사용자 수, 학습 횟수 통계
- 최근 학습 활동 로그
- 위험 키워드 감지 알림

#### 카드 관리
- NFC/QR 카드 등록/수정/삭제
- 학습 콘텐츠 관리
- 매핑 관리 (카드 ID ↔ 콘텐츠)

---

## 🔧 개발 가이드

### 코드 구조

#### vom_user_flutter 주요 파일
```
lib/
├── main.dart                          # 앱 진입점
├── screens/
│   ├── tag_wait_screen.dart          # NFC/QR 대기 화면
│   ├── learning_screen.dart          # 학습 화면 (TTS + 애니메이션)
│   ├── voice_chat_screen.dart        # AI 챗봇 화면
│   └── scan_screen.dart              # QR 스캔 화면
├── services/
│   ├── ai_chat_service.dart          # Gemini AI (⚠️ API 키 포함)
│   ├── stt_service.dart              # 음성 인식
│   ├── tts_service.dart              # 음성 출력
│   ├── nfc_intent_service.dart       # 백그라운드 NFC
│   ├── supabase_service.dart         # DB 연동
│   └── vibration_service.dart        # 진동 피드백
└── constants/
    ├── nfc_contents.dart             # 폴백 콘텐츠 (오프라인)
    └── app_colors.dart               # 컬러 팔레트
```

### 새 기능 추가하기

#### 1. 새 NFC 카드 추가
```dart
// lib/constants/nfc_contents.dart
final fallbackContents = [
  CardContent(
    id: 'new_card',
    name: '새 물건',
    icon: '🆕',
    scripts: ['1단계 설명', '2단계 설명'],
    quizQuestion: '퀴즈 질문',
    quizOptions: ['선택1', '선택2', '선택3'],
    quizCorrectIndex: 0,
  ),
];
```

#### 2. 단계별 이모지 추가
```dart
// lib/screens/learning_screen.dart - _getStepEmojis()
case '새 물건':
  return ['🆕', '📝', '✅'];
```

#### 3. AI 챗봇 오프라인 응답 추가
```dart
// lib/services/ai_chat_service.dart - _getOfflineResponse()
if (message.contains('새키워드')) {
  return '새로운 답변 내용';
}
```

### 디버깅

#### Flutter 로그 확인
```bash
flutter logs
```

#### NFC 디버깅
```dart
// lib/screens/tag_wait_screen.dart
debugPrint('NFC 태그 ID: $tagId');
```

#### AI 챗봇 디버깅
```dart
// lib/services/ai_chat_service.dart
debugPrint('Gemini 응답: $text');
```

---

## 📊 데이터베이스 스키마

### Supabase 테이블

#### users
```sql
id              uuid PRIMARY KEY
name            text
email           text UNIQUE
phone           text
role            text (user/admin)
created_at      timestamp
```

#### card_contents
```sql
id              text PRIMARY KEY
name            text
icon            text
scripts         text[]
quiz_question   text
quiz_options    text[]
quiz_correct_index integer
audio_url       text
video_url       text
created_at      timestamp
```

#### nfc_card_mappings
```sql
id              uuid PRIMARY KEY
nfc_tag_id      text UNIQUE
qr_code         text UNIQUE
card_id         text (FK → card_contents)
created_at      timestamp
```

#### learning_logs
```sql
id              uuid PRIMARY KEY
user_id         uuid (FK → users)
card_id         text (FK → card_contents)
completed       boolean
duration_seconds integer
speech_text     text
created_at      timestamp
```

#### risk_keywords
```sql
id              uuid PRIMARY KEY
keyword         text
severity        text (low/medium/high)
created_at      timestamp
```

### 초기 데이터 세팅
```bash
# Supabase SQL Editor에서 실행
psql -f supabase_schema.sql
```

---

## 🐛 문제 해결

### 자주 발생하는 이슈

#### 1. Flutter 빌드 에러 (Kotlin 버전)
```
error: Module was compiled with an incompatible version of Kotlin
```

**해결**:
```bash
# android/settings.gradle 확인
id "org.jetbrains.kotlin.android" version "2.1.0" apply false

# Clean 후 재빌드
flutter clean
flutter pub get
flutter build apk --release
```

#### 2. NFC가 인식 안 됨
- **포그라운드**: 앱 실행 → TagWaitScreen에서 카드 태그
- **백그라운드**: Android only, iOS는 미지원
- **권한 확인**: AndroidManifest.xml에 NFC 권한 있는지 확인

#### 3. AI 챗봇이 응답 안 함
- API 키 확인: `lib/services/ai_chat_service.dart`
- 인터넷 연결 확인
- Gemini 크레딧 확인 (1,500 requests/day)

#### 4. TTS 음성이 안 나옴
```bash
# 권한 확인
flutter run --verbose

# TTS 엔진 확인 (Android)
# 설정 → 접근성 → 텍스트 음성 변환 출력
```

#### 5. Vercel 배포 실패
```bash
# 환경 변수 확인
vercel env ls

# 환경 변수 추가
vercel env add NEXT_PUBLIC_SUPABASE_URL
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY
```

---

## 🔐 보안 주의사항

### ⚠️ 긴급: API 키 보안

**문제**: Gemini API 키가 GitHub에 공개되었습니다
```dart
// vom_user_flutter/lib/services/ai_chat_service.dart
static const String _apiKey = 'AIzaSyBT1psDP7a2AqYPTzWW_tAyrsbKAbvBRNY';
```

**해결 방법**:
1. **즉시 키 교체**
   - https://aistudio.google.com/app/apikey
   - 기존 키 삭제
   - 새 키 발급

2. **환경 변수 사용** (권장)
   ```dart
   // 새로운 방식
   static const String _apiKey = String.fromEnvironment('GEMINI_API_KEY');
   ```

3. **.gitignore 추가**
   ```
   **/ai_chat_service.dart
   *.env
   ```

### 권장 보안 설정
- API 키는 환경 변수로 관리
- `.env` 파일은 `.gitignore`에 추가
- Supabase RLS (Row Level Security) 활성화

---

## 📝 작업 상태

### ✅ 완료된 기능
- [x] NFC/QR 스캔 시스템
- [x] 백그라운드 NFC 지원 (Android)
- [x] 음성 기반 학습 (TTS)
- [x] 단계별 그래픽 애니메이션
- [x] AI 음성 챗봇 (Gemini)
- [x] 오프라인 키워드 응답
- [x] 학습 기록 저장
- [x] 관리자 대시보드
- [x] 실시간 모니터링
- [x] 위험 키워드 알림
- [x] Vercel 배포
- [x] GitHub 배포

### 🚧 개선 필요 (우선순위)
1. **보안**: API 키 환경 변수화
2. **테스트**: 실제 NFC 카드로 테스트
3. **콘텐츠**: 더 많은 육아 물품 추가
4. **UI/UX**: 사용자 피드백 반영
5. **성능**: APK 크기 최적화 (현재 63MB)

### 🎯 향후 계획
- [ ] 음성 싱크 애니메이션 (TTS와 그래픽 동기화)
- [ ] 다국어 지원 (영어, 베트남어)
- [ ] 오프라인 모드 개선
- [ ] 학습 통계 그래프
- [ ] 푸시 알림 (위험 상황)
- [ ] Google Play 배포

---

## 🤝 기여 가이드

### 브랜치 전략
```
main/master  - 프로덕션 (Vercel 자동 배포)
develop      - 개발 브랜치
feature/*    - 새 기능
bugfix/*     - 버그 수정
```

### 커밋 메시지 규칙
```
feat: 새 기능 추가
fix: 버그 수정
docs: 문서 수정
style: 코드 포맷팅
refactor: 리팩토링
test: 테스트 추가
chore: 빌드/설정 변경
```

### Pull Request
1. Fork 후 브랜치 생성
2. 기능 구현 및 테스트
3. PR 생성 (상세한 설명 포함)
4. 리뷰 후 Merge

---

## 📞 연락처 및 링크

- **GitHub**: https://github.com/chadolmin01/VOM
- **웹 대시보드**: https://vom-web.vercel.app
- **Gemini API**: https://aistudio.google.com/app/apikey
- **Supabase**: https://supabase.com
- **Flutter Docs**: https://docs.flutter.dev

---

## 📄 라이선스

MIT License - 자유롭게 사용, 수정, 배포 가능

---

## 🙏 감사의 말

이 프로젝트는 Hult Prize 2026 해커톤을 위해 개발되었습니다.
글을 읽지 못하는 엄마들이 자신감을 갖고 아이를 돌볼 수 있도록 돕는 것이 목표입니다.

**"모든 엄마는 훌륭한 엄마입니다."**

---

<div align="center">
  <p>Made with ❤️ by V.O.M Team</p>
  <p>
    <a href="https://vom-web.vercel.app">🌐 Web</a> •
    <a href="https://github.com/chadolmin01/VOM">💾 GitHub</a> •
    <a href="DEPLOYMENT.md">📘 Deployment Guide</a>
  </p>
</div>
