# MeTenTen v1.2.0 설치 가이드

## 📦 패키지 내용

### Android
- `com.metenten.app-Signed.apk` (~30 MB) - 설치 파일
- `com.metenten.app-Signed.aab` (~28 MB) - Google Play Store 배포용

### Windows
- `MeTenTenMaui.exe` - 메인 실행 파일
- 모든 종속성 DLL 및 리소스 (전체 ~100 MB)

### 문서
- `RELEASE_NOTES_v1.2.0.md` - 릴리스 노트 (DEK 암호화 설명)
- `MIGRATION_v1.2.md` - 마이그레이션 가이드
- `DEPLOYMENT.md` - 상세 배포 가이드
- `README.md` - 프로젝트 개요
- `CHANGELOG.md` - 변경 이력

---

## 🔐 v1.2.0 주요 변경사항

### DEK 암호화 시스템 도입

v1.2.0부터는 **모든 10&10 내용이 AES-256으로 암호화**되어 저장됩니다!

- ✅ **완전한 프라이버시**: Firebase 관리자도 내용을 읽을 수 없음
- ✅ **비밀번호 변경 가능**: 데이터 재암호화 없이 비밀번호 변경 가능
- ✅ **자동 보안 관리**: 로그인 시 자동 암호화 키 로드

### ⚠️ v1.1 사용자 주의사항

**기존 평문 데이터 삭제 권장:**
- v1.1에서 작성한 데이터는 **평문(암호화 안 됨)**으로 저장되어 있음
- 보안을 위해 Firebase Console에서 기존 데이터 삭제 권장
- v1.2 설치 후 새로 작성하는 데이터는 자동 암호화

자세한 내용은 `MIGRATION_v1.2.md` 참조

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

### v1.1에서 업그레이드 시

1. **기존 앱 제거** (선택사항)
   - 설정 → 앱 → MeTenTen → 제거
   - 또는 그대로 설치 (덮어쓰기)

2. **Firebase 데이터 정리** (권장)
   - Firebase Console에서 기존 평문 데이터 삭제
   - 또는 `MIGRATION_v1.2.md` 참조

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

3. **바로가기 생성** (선택사항)
   - `MeTenTenMaui.exe` 우클릭 → 바로가기 만들기
   - 바탕화면이나 시작 메뉴로 이동

### v1.1에서 업그레이드 시

1. **기존 앱 삭제** (선택사항)
   - 기존 MeTenTen 폴더 삭제
   - 또는 새 폴더에 설치 (예: `MeTenTen_v1.2`)

2. **Firebase 데이터 정리** (권장)
   - Firebase Console에서 기존 평문 데이터 삭제

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

4. **v1.2 보안 규칙 업데이트** (중요!)
   - `Firebase/database.rules.json` 파일의 내용을 Firebase Console에 적용
   - users 노드 규칙이 추가되어야 함 (encryptedDEK 저장용)

자세한 내용은 `Firebase/SECURITY_RULES_SETUP.md` 참조

---

## ✅ 설치 확인 및 첫 실행

### 1. 앱 실행
- Android: 앱 서랍에서 MeTenTen 아이콘 탭
- Windows: `MeTenTenMaui.exe` 실행

### 2. 회원가입 (신규 사용자)
1. 이메일 주소 입력
2. 사용자 이름 입력
3. 비밀번호 입력 (최소 6자)
4. 회원가입 버튼 클릭

**자동 처리:**
- ✅ 랜덤 DEK(암호화 키) 자동 생성
- ✅ DEK를 비밀번호로 암호화하여 Firebase 저장
- ✅ 로그인 완료

### 3. 로그인 (기존 사용자)
1. 이메일 주소 입력
2. 비밀번호 입력
3. 로그인 버튼 클릭

**자동 처리 (v1.2):**
- ✅ Firebase에서 암호화된 DEK 가져오기
- ✅ 비밀번호로 DEK 복호화
- ✅ DEK를 메모리에 로드 (데이터 암호화/복호화에 사용)

**v1.1 사용자:**
- ✅ DEK가 없으면 자동으로 새 DEK 생성
- ✅ 이후 작성하는 데이터는 자동 암호화

### 4. 10&10 작성 테스트
1. Topic 생성
2. 10&10 작성
3. 저장

**확인:**
- ✅ Firebase Console → tentens/{userId}/{id}/content
- ✅ 암호화된 문자열이 보여야 함 (평문 아님!)
- ✅ `isEncrypted: true` 확인

### 5. Firebase 데이터 구조 확인

```json
{
  "users": {
    "userId123": {
      "email": "user@example.com",
      "displayName": "사용자",
      "encryptedDEK": "암호화된문자열...",  // ← 이게 있어야 함!
      "createdAt": "2025-01-12T10:00:00Z"
    }
  },
  "tentens": {
    "userId123": {
      "tentenId456": {
        "content": "암호화된내용...",  // ← 평문이 아님!
        "isEncrypted": true,
        "topicId": "1",
        "createdAt": "2025-01-12T10:30:00Z"
      }
    }
  }
}
```

---

## 🔧 문제 해결

### 공통 문제

**"Firebase 연결 오류"**
- 인터넷 연결 확인
- Firebase 프로젝트 설정 확인
- API Key가 올바른지 확인

**"로그인 실패"**
- 이메일 주소 형식 확인
- 비밀번호는 최소 6자 이상
- Firebase Authentication이 활성화되어 있는지 확인

**"[복호화 실패]" 메시지**
- 잘못된 비밀번호로 로그인했을 가능성
- 로그아웃 후 올바른 비밀번호로 재로그인
- 문제가 지속되면 해당 10&10 삭제 후 재작성

### v1.2 특화 문제

**"DEK를 찾을 수 없습니다" (로그)**
- v1.1 사용자의 첫 로그인: 정상 (자동으로 DEK 생성됨)
- 확인: Firebase Console → users/{userId}/encryptedDEK 존재 확인

**"DEK 복호화 실패"**
- 비밀번호가 잘못되었을 가능성
- 올바른 비밀번호로 재로그인
- 문제 지속 시: 계정 재생성 필요

### Android 문제

**"앱을 설치할 수 없습니다"**
- 저장 공간 확인
- 알 수 없는 소스 허용 확인
- 이전 버전 삭제 후 재설치

**"앱이 계속 중단됨"**
- 앱 삭제 후 재설치
- Android 버전 확인 (7.0 이상)
- Firebase 데이터 정리 (v1.1 평문 데이터 삭제)

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
- 로그 파일 첨부 (민감한 정보 제거 후)
- `RELEASE_NOTES_v1.2.0.md`의 알려진 제한사항 확인
- `MIGRATION_v1.2.md`의 문제 해결 섹션 참조

---

## 🔒 보안 권장사항

### 비밀번호 관리
- ⚠️ **비밀번호를 잊어버리면 데이터를 복구할 수 없습니다**
- 비밀번호는 안전한 곳에 별도로 기록해두세요
- 강력한 비밀번호 사용 (8자 이상, 대소문자/숫자 조합)

### 계정 보안
- 다른 사람과 계정을 공유하지 마세요
- 공용 기기에서 사용 후 꼭 로그아웃하세요
- 정기적으로 비밀번호 변경 (v1.3에서 UI 제공 예정)

### 데이터 백업
- 중요한 내용은 별도로 백업하세요
- Firebase Console에서도 암호화된 데이터는 읽을 수 없습니다
- 복구 키 백업 기능은 v1.3에 추가 예정

---

**MeTenTen v1.2.0**  
**릴리스 날짜**: 2025-01-12  
**DEK 암호화로 더욱 안전하게! 🔐✨**  
**Made with ❤️ for Marriage Encounter**

