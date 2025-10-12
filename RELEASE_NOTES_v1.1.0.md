# MeTenTen v1.1.0 Release Notes

**릴리스 날짜**: 2025-01-12  
**빌드 번호**: 2

## 📦 다운로드

### Android
- **파일**: `com.metenten.app-Signed.apk`
- **위치**: `MeTenTenMaui/bin/Release/net9.0-android/publish/`
- **크기**: ~28.4 MB
- **최소 요구사항**: Android 7.0 (API 24) 이상

### Windows
- **파일**: `MeTenTenMaui.exe`
- **위치**: `MeTenTenMaui/bin/Release/net9.0-windows10.0.19041.0/win10-x64/publish/`
- **크기**: ~275 KB (실행 파일만)
- **전체 패키지**: ~100 MB (모든 종속성 포함)
- **최소 요구사항**: Windows 10 (19041) 이상

## ✨ 주요 기능

### 인증 및 사용자 관리
- ✅ Firebase 이메일/비밀번호 인증
- ✅ 자동 로그인 (SecureStorage 활용)
- ✅ 로그아웃 기능

### 10&10 (TenTen) 기능
- ✅ 주제별 TenTen 작성
- ✅ 10분 타이머 기능
- ✅ TenTen 수정 및 삭제
- ✅ 월별 주제 필터링
- ✅ 읽기/편집 모드 전환

### Topic 관리
- ✅ 날짜별 주제 생성
- ✅ 주제 수정 및 삭제
- ✅ 주제별 설명 추가
- ✅ 월별 네비게이션

### 감정 표현 (Feeling)
- ✅ 4가지 기본 감정 카테고리
  - 기쁨 (Joy)
  - 두려움 (Fear)
  - 분노 (Anger)
  - 슬픔 (Sadness)
- ✅ 감정 예시 및 설명
- ✅ 사용자 정의 감정 추가/삭제

### 기도문 (Prayer)
- ✅ 부부를 위한 기도문 모음
- ✅ 기도문 즐겨찾기
- ✅ 클립보드 복사

### 파트너 대시보드
- ✅ TenTen 통계
- ✅ 최근 작성 내역
- ✅ 활동 일수 추적

### 플랫폼 지원
- ✅ Android (ARM64, x64)
- ✅ Windows 10/11
- ✅ 크로스 플랫폼 UI (Blazor Hybrid)

### 네비게이션
- ✅ 커스텀 Back 키 처리
- ✅ 종료 확인 다이얼로그
- ✅ 계층적 네비게이션 (하위 페이지 → Home)

## 🔐 보안

### 현재 구현
- ✅ Firebase Authentication
- ✅ SecureStorage를 통한 자격 증명 저장
- ✅ Firebase 보안 규칙 (사용자별 데이터 격리)
- ✅ 읽기/쓰기 권한 검증

### 알려진 제한사항
⚠️ **데이터 암호화 미구현**: 현재 TenTen 내용이 Firebase에 평문으로 저장됩니다.
- Firebase 관리자가 데이터를 볼 수 있습니다
- v1.2에서 RSA+AES End-to-End 암호화 예정

## 🐛 버그 수정

- ✅ NavigationManager 초기화 오류 수정
- ✅ Back 키 네비게이션 안정화
- ✅ Android Back 키 관련 크래시 수정
- ✅ 모든 빌드 경고 제거

## 📝 코드 품질

- ✅ 모든 컴파일 경고 수정
- ✅ 미사용 예외 변수 제거
- ✅ 접근 불가능한 코드 제거
- ✅ 일관된 에러 처리

## 📦 설치 방법

### Android
1. `com.metenten.app-Signed.apk` 다운로드
2. 기기에서 "알 수 없는 소스" 허용
3. APK 파일 실행하여 설치
4. MeTenTen 앱 실행

### Windows
1. `MeTenTenMaui/bin/Release/net9.0-windows10.0.19041.0/win10-x64/publish/` 폴더 전체 복사
2. `MeTenTenMaui.exe` 실행
3. (선택사항) 바로가기 생성

## 🔧 Firebase 설정 필요사항

이 앱을 사용하려면 Firebase 프로젝트가 필요합니다:

1. Firebase Console에서 프로젝트 생성
2. Authentication 활성화 (Email/Password)
3. Realtime Database 생성
4. 보안 규칙 설정 (Firebase/database.rules.json 참조)
5. API Key 업데이트 필요 시

자세한 내용은 README.md 참조

## 🚫 알려진 제한사항

- ⚠️ 데이터가 평문으로 저장됨 (암호화 미구현)
- ⚠️ 파트너 연결 기능 미구현
- ⚠️ 오프라인 모드 미지원
- ⚠️ iOS 빌드 미제공 (Mac 환경 필요)
- ⚠️ 푸시 알림 미구현

## 🔮 다음 버전 (v1.2) 계획

### 암호화 (최우선)
- 🔐 RSA+AES End-to-End 암호화
- 🔑 공개키/개인키 관리
- 🔓 복구 키 시스템
- 🛡️ Firebase 관리자도 볼 수 없는 완전한 프라이버시

### 파트너 기능
- 👥 파트너 초대 및 연결
- 📖 서로의 TenTen 공유
- ✅ 읽음 확인
- 🔔 파트너 활동 알림

### 추가 기능
- 📊 통계 및 분석
- 🔔 푸시 알림
- 📱 iOS 지원
- 🌐 오프라인 모드

## 🙏 감사의 말

Marriage Encounter 프로그램에서 영감을 받아 개발되었습니다.  
부부 소통의 도구로 유용하게 사용되기를 바랍니다.

---

**Made with ❤️ for Marriage Encounter**

## 📞 문의 및 버그 리포트

- GitHub Issues
- 프로젝트 저장소

## 📜 라이선스

개인 사용 목적

