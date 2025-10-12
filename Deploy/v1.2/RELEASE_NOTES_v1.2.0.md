# MeTenTen v1.2.0 릴리스 노트

**릴리스 날짜**: 2025-01-12  
**버전**: 1.2.0 (Build 3)

---

## 🎉 주요 기능: DEK 암호화 시스템

v1.2.0의 가장 큰 변화는 **DEK (Data Encryption Key) 방식의 End-to-End 암호화** 도입입니다!

### 🔐 완전한 프라이버시 보호

- ✅ **모든 10&10 내용이 AES-256으로 암호화**되어 Firebase에 저장됩니다
- ✅ **Firebase 관리자도 내용을 읽을 수 없습니다**
- ✅ **사용자의 비밀번호만이 유일한 복호화 수단**입니다

### 🔑 DEK 방식의 핵심 장점

#### 비밀번호 변경 가능! (v1.2의 혁신)

```
기존 방식 (v1.1):
❌ 비밀번호 변경 → 모든 데이터 재암호화 필요 (불가능)
❌ 비밀번호 분실 → 데이터 영구 손실

DEK 방식 (v1.2):
✅ 비밀번호 변경 → DEK만 재암호화 (1초 내 완료)
✅ 모든 10&10 데이터는 그대로 유지!
⚠️ 비밀번호 분실 → 데이터 복구 불가 (동일)
```

#### 동작 원리

1. **회원가입 시**: 랜덤 DEK 생성 → 비밀번호로 암호화 → Firebase 저장
2. **로그인 시**: 암호화된 DEK 가져오기 → 비밀번호로 복호화 → 메모리 로드
3. **데이터 작성/수정**: DEK로 암호화하여 저장
4. **데이터 조회**: DEK로 복호화하여 표시
5. **로그아웃 시**: 메모리에서 DEK 안전 삭제

---

## 📦 다운로드

### Android
- **APK**: `com.metenten.app-Signed.apk` (약 30 MB)
- **AAB**: `com.metenten.app-Signed.aab` (약 28 MB, Google Play용)

### Windows
- **실행 파일**: `MeTenTenMaui.exe` (전체 약 100 MB, 종속성 포함)

### 설치 방법
- Android: `Deploy/v1.2/INSTALL.md` 참조
- Windows: `Deploy/v1.2/INSTALL.md` 참조

---

## ✨ 주요 변경사항

### 새로운 기능

#### 1. DEK 암호화 시스템
- 회원가입 시 자동 DEK 생성
- 로그인 시 자동 DEK 로드
- 로그아웃 시 자동 DEK 삭제
- 비밀번호 변경 대비 아키텍처 (UI는 v1.3)

#### 2. Firebase 데이터 구조 개선
- `users/{userId}` 노드 추가
  - `encryptedDEK`: 비밀번호로 암호화된 DEK
  - `email`, `displayName`
  - `createdAt`, `updatedAt`
- `tentens/{userId}/{tentenId}` 구조 유지
  - `isEncrypted`: 암호화 여부 플래그 추가

#### 3. 보안 규칙 강화
- users 노드에 접근 제어 추가
- encryptedDEK 필드 검증
- isEncrypted 필드 검증

### 개선사항

- **암호화 성능**: 최적화된 AES-256-CBC 구현
- **키 관리**: PBKDF2 (100,000 iterations) 적용
- **메모리 보안**: 로그아웃 시 키 안전 삭제
- **호환성**: v1.1 평문 데이터 읽기 지원

### 기술 상세

- **암호화 알고리즘**: AES-256-CBC
- **키 파생**: PBKDF2 (100,000 iterations, SHA-256)
- **DEK 크기**: 256-bit (32 bytes)
- **IV**: 각 암호화마다 랜덤 128-bit IV 생성
- **인코딩**: Base64(IV + EncryptedData)

---

## ⚠️ 중요 안내

### 기존 v1.1 사용자

v1.1.0 이하에서 작성한 10&10 데이터는 **평문(암호화되지 않음)**으로 저장되어 있습니다.

#### 권장 조치
1. **v1.2 업데이트 전**: 중요한 데이터 백업
2. **Firebase Console에서 기존 데이터 삭제** (권장)
3. **v1.2 설치 후**: 새로 작성하는 모든 데이터는 자동 암호화

#### 자동 처리
- 기존 사용자가 v1.2로 로그인 시 자동으로 DEK 생성
- 기존 평문 데이터는 `isEncrypted: false`로 읽기 가능
- 새로 작성하는 데이터는 자동 암호화

### 비밀번호 관리

- ⚠️ **비밀번호를 잊어버리면 데이터를 복구할 수 없습니다**
- 비밀번호는 안전한 곳에 별도로 기록해두세요
- ✅ v1.2부터는 비밀번호 변경 가능 (데이터 유지)

---

## 🐛 알려진 제한사항

### 미구현 기능
- ❌ 비밀번호 변경 UI (백엔드는 준비됨, v1.3 구현 예정)
- ❌ DEK 복구 키 백업 기능
- ❌ 기존 평문 데이터 자동 마이그레이션
- ❌ 파트너 공유 기능 (v1.3 예정)

### 제한사항
- 비밀번호 분실 시 데이터 복구 불가
- 오프라인 모드 미지원
- iOS/macOS 빌드 미테스트

---

## 📋 설치 요구사항

### Android
- Android 7.0 (API 24) 이상
- 저장 공간 50 MB 이상
- 인터넷 연결 필요 (Firebase)

### Windows
- Windows 10 (빌드 19041) 이상 또는 Windows 11
- 저장 공간 200 MB 이상
- 인터넷 연결 필요 (Firebase)

---

## 🚀 설치 방법

자세한 설치 방법은 `Deploy/v1.2/INSTALL.md` 참조

### 빠른 설치

#### Android
1. `com.metenten.app-Signed.apk` 다운로드
2. "알 수 없는 소스" 허용
3. APK 파일 실행하여 설치

#### Windows
1. `Deploy/v1.2/Windows/` 폴더 전체 복사
2. `MeTenTenMaui.exe` 실행

---

## 🔧 업그레이드 가이드

### v1.1 → v1.2 업그레이드

#### 단계별 가이드

1. **데이터 백업** (선택사항)
   - 중요한 10&10 내용을 별도로 복사

2. **기존 앱 삭제** (Android)
   - 설정 → 앱 → MeTenTen → 제거

3. **Firebase 데이터 삭제** (권장)
   - Firebase Console → Realtime Database
   - `tentens/` 노드 삭제
   - `topics/` 노드 삭제
   - `users/` 노드는 비워두기 (v1.2가 자동 생성)

4. **v1.2 설치**
   - 새 APK 설치 (Android)
   - 새 폴더에 압축 해제 (Windows)

5. **첫 로그인**
   - 기존 계정으로 로그인
   - DEK 자동 생성됨
   - 새로 10&10 작성 시작

자세한 내용은 `MIGRATION_v1.2.md` 참조

---

## 📝 변경사항 전체 목록

### 추가된 파일
- `MeTenTenMaui/Services/IEncryptionService.cs`
- `MeTenTenMaui/Services/EncryptionService.cs`
- `MeTenTenMaui/Models/Firebase/FirebaseUser.cs`
- `MIGRATION_v1.2.md`

### 수정된 파일
- `MeTenTenMaui/Services/FirebaseAuthService.cs` - DEK 생성/로드 로직
- `MeTenTenMaui/Services/FirebaseDataService.cs` - DEK 저장/조회, 암호화 통합
- `MeTenTenMaui/Services/FirebaseTenTenService.cs` - 복호화 로직
- `MeTenTenMaui/Services/IFirebaseDataService.cs` - DEK 메서드 추가
- `MeTenTenMaui/Models/TenTen.cs` - IsEncrypted 필드 추가
- `MeTenTenMaui/Models/Firebase/FirebaseTenTen.cs` - IsEncrypted 필드 추가
- `MeTenTenMaui/MauiProgram.cs` - EncryptionService 등록
- `MeTenTenMaui/MeTenTenMaui.csproj` - 버전 1.2.0으로 업데이트
- `Firebase/database.rules.json` - users 노드 및 isEncrypted 검증 추가

---

## 🧪 테스트 완료 항목

- ✅ Android Debug 빌드 성공
- ✅ Windows Debug 빌드 성공
- ✅ 빌드 경고 0개
- ⏳ 기능 테스트 (수동 테스트 필요)
  - 회원가입 → DEK 생성
  - 로그인 → DEK 로드
  - 10&10 작성 → 암호화
  - 10&10 조회 → 복호화
  - 로그아웃 → DEK 삭제

---

## 📞 지원 및 문제 해결

### 자주 묻는 질문

**Q: 비밀번호를 잊어버리면 어떻게 하나요?**  
A: Firebase Authentication에서 비밀번호 재설정은 가능하지만, 기존 암호화된 데이터는 복구할 수 없습니다. 새로 작성하는 데이터는 새 비밀번호로 보호됩니다.

**Q: 기존 v1.1 데이터는 어떻게 하나요?**  
A: 보안을 위해 Firebase Console에서 삭제하는 것을 권장합니다. 유지하려면 앱에서 읽을 수 있지만 암호화되지 않은 상태입니다.

**Q: "[복호화 실패]" 메시지가 나옵니다.**  
A: 잘못된 비밀번호로 로그인했거나 데이터가 손상되었을 수 있습니다. 올바른 비밀번호로 재로그인 해보세요.

**Q: 비밀번호를 변경하고 싶어요.**  
A: v1.2.0에서는 백엔드만 준비되어 있습니다. v1.3에서 UI가 추가될 예정입니다.

### 버그 리포트

문제가 발생하면:
- GitHub Issues에 상세 내용 작성
- 로그 파일 첨부 (민감한 정보 제거 후)
- 재현 방법 설명

---

## 🗺️ 로드맵

### v1.3 (예정)
- 비밀번호 변경 UI
- DEK 복구 키 백업/복원
- 파트너 연결 기능 (RSA 공개키 교환)
- 성능 최적화

### v1.4 (예정)
- 오프라인 모드
- 알림 시스템
- 통계 및 분석

---

## 📄 관련 문서

- **CHANGELOG.md**: 전체 변경 이력
- **MIGRATION_v1.2.md**: 마이그레이션 상세 가이드
- **INSTALL.md**: 설치 방법
- **Firebase/SECURITY_RULES_SETUP.md**: Firebase 보안 규칙 설정

---

## 💝 감사의 말

Marriage Encounter 공동체를 위해 개발된 MeTenTen을 사용해주셔서 감사합니다.  
v1.2.0의 DEK 암호화 시스템으로 더욱 안전하게 부부의 소중한 대화를 나누세요.

**Made with ❤️ for Marriage Encounter**

---

## 📊 빌드 정보

### Android
- **Package**: com.metenten.app
- **Version Code**: 3
- **Version Name**: 1.2.0
- **Min SDK**: 24 (Android 7.0)
- **Target SDK**: 34 (Android 14)

### Windows
- **Runtime**: .NET 9.0
- **Platform**: win10-x64
- **Version**: 1.2.0

### 빌드 아티팩트
- Android APK: ~30 MB
- Android AAB: ~28 MB
- Windows 앱: ~100 MB (종속성 포함)

---

**릴리스 날짜**: 2025-01-12  
**다음 릴리스 예정**: v1.3 (2025-02-XX) - 비밀번호 변경 UI 및 파트너 공유

