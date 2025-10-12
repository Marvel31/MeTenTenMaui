# MeTenTen v1.1.0 설치 가이드

## 📦 패키지 내용

### Android
- `com.metenten.app-Signed.apk` (28.4 MB) - 설치 파일
- `com.metenten.app-Signed.aab` (27.9 MB) - Google Play Store 배포용

### Windows
- `MeTenTenMaui.exe` - 메인 실행 파일
- 모든 종속성 DLL 및 리소스 (전체 ~100 MB)

### 문서
- `RELEASE_NOTES_v1.1.0.md` - 릴리스 노트
- `DEPLOYMENT.md` - 상세 배포 가이드
- `README.md` - 프로젝트 개요
- `CHANGELOG.md` - 변경 이력

---

## 📱 Android 설치

### 방법 1: APK 직접 설치 (권장)

1. **APK 파일 전송**
   - `Android/com.metenten.app-Signed.apk`를 Android 기기로 전송
   - USB 케이블 연결 후 파일 복사
   - 또는 클라우드 서비스 이용 (Google Drive, OneDrive 등)

2. **알 수 없는 소스 허용**
   - 설정 → 보안 → 알 수 없는 소스 허용
   - Android 8.0 이상: 설치할 때 권한 요청 시 허용

3. **설치 실행**
   - 파일 관리자에서 APK 파일 실행
   - 설치 버튼 클릭
   - 완료 후 MeTenTen 앱 실행

### 방법 2: ADB를 통한 설치

```bash
adb install Android/com.metenten.app-Signed.apk
```

### 최소 요구사항
- Android 7.0 (API 24) 이상
- 저장 공간 50 MB 이상
- 인터넷 연결 (Firebase 사용)

---

## 💻 Windows 설치

### 설치 방법

1. **폴더 전체 복사**
   - `Windows/` 폴더 전체를 원하는 위치에 복사
   - 예: `C:\Program Files\MeTenTen\`

2. **실행**
   - `MeTenTenMaui.exe` 더블클릭하여 실행

3. **바로가기 생성 (선택사항)**
   - `MeTenTenMaui.exe` 우클릭 → 바로가기 만들기
   - 바탕화면이나 시작 메뉴로 이동

### 최소 요구사항
- Windows 10 (빌드 19041) 이상 또는 Windows 11
- .NET Desktop Runtime (앱에 포함되어 있음)
- 저장 공간 200 MB 이상
- 인터넷 연결 (Firebase 사용)

### 문제 해결

**실행 시 오류 발생:**
- Windows Defender SmartScreen 경고가 나타날 수 있습니다
- "추가 정보" → "실행" 클릭

**누락된 DLL 오류:**
- `Windows/` 폴더의 모든 파일이 함께 있는지 확인
- 폴더 전체를 다시 복사

---

## 🔐 Firebase 설정

### 필수 설정

이 앱을 사용하려면 Firebase 프로젝트가 설정되어 있어야 합니다.

1. **Firebase Console 접속**
   - https://console.firebase.google.com/

2. **Authentication 확인**
   - Email/Password 제공자가 활성화되어 있는지 확인

3. **Realtime Database 확인**
   - Database가 생성되어 있는지 확인
   - 보안 규칙이 설정되어 있는지 확인

자세한 내용은 `README.md` 및 `Firebase/SECURITY_RULES_SETUP.md` 참조

---

## ✅ 설치 확인

### Android
1. 앱 아이콘이 앱 서랍에 표시되는지 확인
2. 앱 실행 시 로그인 화면이 나타나는지 확인
3. 회원가입/로그인 기능 테스트

### Windows
1. `MeTenTenMaui.exe` 실행 시 창이 열리는지 확인
2. 로그인 화면이 정상적으로 표시되는지 확인
3. 회원가입/로그인 기능 테스트

---

## 🆘 문제 해결

### 공통 문제

**"Firebase 연결 오류"**
- 인터넷 연결 확인
- Firebase 프로젝트 설정 확인
- API Key가 올바른지 확인

**"로그인 실패"**
- 이메일 주소 형식 확인
- 비밀번호는 최소 6자 이상
- Firebase Authentication이 활성화되어 있는지 확인

### Android 문제

**"앱을 설치할 수 없습니다"**
- 저장 공간 확인
- 알 수 없는 소스 허용 확인
- 이전 버전 삭제 후 재설치

**"앱이 계속 중단됨"**
- 앱 삭제 후 재설치
- Android 버전 확인 (7.0 이상)
- 기기 재시작

### Windows 문제

**"앱이 시작되지 않음"**
- Windows 버전 확인 (10 빌드 19041 이상)
- 폴더 전체가 복사되었는지 확인
- 바이러스 검사 프로그램 예외 추가

**"화면이 제대로 표시되지 않음"**
- 화면 해상도 확인
- DPI 설정 확인
- 그래픽 드라이버 업데이트

---

## 📞 지원

문제가 계속되면:
- GitHub Issues에 버그 리포트
- 프로젝트 저장소 참조
- `RELEASE_NOTES_v1.1.0.md`의 알려진 제한사항 확인

---

**MeTenTen v1.1.0**  
**릴리스 날짜**: 2025-01-12  
**Made with ❤️ for Marriage Encounter**

