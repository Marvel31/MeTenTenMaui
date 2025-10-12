# MeTenTen 💕

## 프로젝트 소개

**MeTenTen**은 Marriage Encounter의 10&10 프로그램을 기반으로 한 부부 소통 앱입니다.
10분간 편지를 쓰고, 편지를 교환한 후 10분간 대화를 나누는 시간을 통해 서로의 마음을 더 깊이 이해할 수 있습니다.

### 10&10이란?

- **10분 작성**: 주제에 대해 10분간 자신의 생각과 감정을 편지로 작성
- **교환**: 작성한 편지를 서로 교환하여 읽기
- **10분 대화**: 편지를 읽은 후 10분간 대화 나누기

## 주요 기능

### 🔐 v1.2.0 신규: DEK 암호화 시스템

- **End-to-End 암호화**
  - AES-256-CBC로 모든 10&10 내용 암호화
  - Firebase 관리자도 읽을 수 없는 완전한 프라이버시
  - DEK (Data Encryption Key) 방식으로 안전하고 유연한 암호화

- **비밀번호 변경 가능**
  - 기존 데이터 유지하면서 비밀번호 변경 가능
  - DEK만 재암호화 (모든 데이터 재암호화 불필요)
  - 산업 표준 키 관리 방식 적용

- **자동 보안 관리**
  - 회원가입 시 자동 DEK 생성 및 암호화
  - 로그인 시 자동 DEK 로드
  - 로그아웃 시 메모리에서 안전 삭제

### ✨ v1.1.0 기능

- 📝 **10&10 작성 및 관리**
  - 주제별 TenTen 작성
  - 10분 타이머 기능
  - 작성 내용 수정 및 삭제

- 📅 **Topic 관리**
  - 날짜별 주제 생성
  - 월별 주제 필터링
  - 주제 수정 및 삭제

- 😊 **느낌 표현**
  - 4가지 기본 감정 카테고리 (기쁨, 두려움, 분노, 슬픔)
  - 감정 예시 및 설명

- 🙏 **주요 기도문**
  - 부부를 위한 기도문 모음
  - 기도문 즐겨찾기

- 💑 **파트너 페이지**
  - TenTen 통계 확인
  - 최근 작성 내역

- 🔐 **사용자 인증**
  - Firebase Authentication
  - 이메일/비밀번호 로그인
  - 자동 로그인

- 📱 **크로스 플랫폼**
  - Android, iOS, Windows 지원
  - 커스텀 Back 키 네비게이션
  - 종료 확인 다이얼로그

## 기술 스택

- **.NET 9 MAUI**: 크로스 플랫폼 프레임워크
- **Blazor Hybrid**: UI 구현
- **Firebase**: 백엔드 서비스
  - Firebase Authentication: 사용자 인증
  - Firebase Realtime Database: 데이터 저장
- **암호화**: End-to-End 보안
  - AES-256-CBC: 데이터 암호화
  - PBKDF2: 키 파생 (100,000 iterations, SHA-256)
  - DEK: 비밀번호 변경 가능한 유연한 키 관리
- **C#**: 주 개발 언어

## 설치 및 실행

### 요구사항

- .NET 9 SDK
- Visual Studio 2022 또는 Visual Studio Code
- Android SDK (Android 빌드 시)
- Windows 10/11 SDK (Windows 빌드 시)

### Firebase 설정

1. [Firebase Console](https://console.firebase.google.com/)에서 프로젝트 생성
2. Authentication 활성화 (Email/Password 제공자 추가)
3. Realtime Database 생성
4. `MeTenTenMaui/Services/FirebaseAuthService.cs`에서 API Key 업데이트:
   ```csharp
   private const string FirebaseApiKey = "YOUR_API_KEY_HERE";
   ```

### 보안 규칙 설정

Firebase Realtime Database 보안 규칙을 설정하려면:

1. Firebase Console → Realtime Database → 규칙 탭
2. `Firebase/database.rules.json` 파일의 내용을 복사하여 붙여넣기
3. 게시 버튼 클릭

자세한 내용은 `Firebase/SECURITY_RULES_SETUP.md` 참조

### 빌드 및 실행

#### Windows
```bash
dotnet build MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0
dotnet run --project MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0
```

#### Android
```bash
dotnet build MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-android -c Debug
```

## 프로젝트 구조

```
MeTenTenMaui/
├── Components/
│   ├── Layout/          # 레이아웃 컴포넌트
│   └── Pages/           # 페이지 컴포넌트
├── Models/              # 데이터 모델
├── Services/            # 서비스 레이어
│   ├── FirebaseAuthService.cs
│   ├── FirebaseDataService.cs
│   ├── FirebaseTopicService.cs
│   └── FirebaseTenTenService.cs
├── Platforms/           # 플랫폼별 코드
│   ├── Android/
│   └── Windows/
└── wwwroot/             # 정적 파일 (CSS, 이미지)
```

## 🔒 보안 (v1.2.0)

### DEK 암호화 방식

MeTenTen은 **DEK (Data Encryption Key)** 방식으로 사용자 데이터를 보호합니다:

```
회원가입 시:
1. 랜덤 256-bit DEK 생성
2. DEK를 사용자 비밀번호로 암호화
3. 암호화된 DEK를 Firebase에 저장
4. 모든 10&10 데이터는 DEK로 암호화

로그인 시:
1. Firebase에서 암호화된 DEK 가져오기
2. 비밀번호로 DEK 복호화
3. DEK를 메모리에 로드
4. 10&10 데이터 읽기/쓰기 시 DEK 사용

비밀번호 변경 시:
1. 기존 비밀번호로 DEK 복호화
2. 새 비밀번호로 DEK 재암호화
3. ✅ 모든 10&10 데이터는 그대로 유지!
```

### 보안 특징

- ✅ **End-to-End 암호화**: Firebase 관리자도 내용을 읽을 수 없음
- ✅ **비밀번호 변경 가능**: DEK만 재암호화 (데이터 유지)
- ✅ **안전한 키 관리**: 로그아웃 시 메모리에서 자동 삭제
- ✅ **강력한 암호화**: AES-256-CBC, PBKDF2 (100,000 iterations)
- ⚠️ **주의**: 비밀번호 분실 시 데이터 복구 불가

자세한 내용은 [MIGRATION_v1.2.md](MIGRATION_v1.2.md) 참조

## 버전 정보

- **현재 버전**: 1.2.0
- **릴리스 날짜**: 2025-01-12

변경 이력은 [CHANGELOG.md](CHANGELOG.md) 참조

## 향후 계획 (v1.3+)

- 💑 **파트너 연결 기능**
  - 파트너 초대 및 연결
  - RSA 공개키 교환으로 안전한 데이터 공유
  - 읽음 확인

- 🔑 **복구 키 백업**
  - DEK 복구 키 생성 및 백업
  - 비밀번호 분실 시 복구 옵션

- 🔔 **알림 시스템**
  - 10&10 작성 시간 리마인더
  - 파트너 작성 완료 알림

- 📊 **통계 및 분석**
  - 월별/연도별 통계
  - 작성 빈도 그래프
  - 감정 트렌드 분석

## 라이선스

이 프로젝트는 개인 사용 목적으로 개발되었습니다.

## 기여

버그 리포트나 기능 제안은 Issue를 통해 제출해 주세요.

---

**Made with ❤️ for Marriage Encounter**

