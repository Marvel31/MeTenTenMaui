# Changelog

All notable changes to this project will be documented in this file.

## [1.2.0] - 2025-01-12

### 🔐 주요 기능: DEK 암호화 시스템

#### 새로운 기능
- **DEK (Data Encryption Key) 방식 End-to-End 암호화**
  - AES-256-CBC 암호화로 모든 TenTen 데이터 보호
  - Firebase 관리자도 데이터 읽기 불가
  - 사용자 비밀번호로만 복호화 가능
- **비밀번호 변경 가능** (기존 데이터 유지)
  - DEK만 재암호화하면 되므로 모든 데이터 그대로 유지
  - v1.1과 달리 비밀번호 변경 시 데이터 재암호화 불필요
- **Firebase users 노드 추가**
  - 암호화된 DEK를 사용자별로 저장
  - 이메일, 표시 이름, 생성/수정 시간 관리

#### 암호화 상세
- **DEK 생성**: 회원가입 시 랜덤 256-bit 키 생성
- **DEK 보호**: PBKDF2 (100,000 iterations, SHA-256) + 비밀번호로 DEK 암호화
- **데이터 암호화**: DEK를 사용한 AES-256-CBC 암호화
- **IV**: 각 데이터마다 랜덤 IV 생성 및 첨부
- **저장 형식**: Base64(IV + EncryptedData)

#### 보안 규칙 업데이트
- Firebase Realtime Database 보안 규칙 강화
- users 노드에 encryptedDEK 필드 검증 추가
- tentens 노드에 isEncrypted 필드 검증 추가

#### 마이그레이션 가이드
- MIGRATION_v1.2.md 문서 제공
- v1.1 이하 평문 데이터와 호환성 유지
- 기존 데이터 삭제 권장 (보안)

### ⚠️ Breaking Changes
- 암호화 시스템 완전 재설계 (단순 비밀번호 기반 → DEK 방식)
- v1.1 이하 사용자는 첫 로그인 시 자동으로 DEK 생성
- 기존 평문 데이터는 isEncrypted=false로 읽기 가능 (호환성)

### 개선사항
- 암호화 키 관리 구조 개선
- 로그인/회원가입 시 DEK 자동 처리
- 메모리 보안 강화 (로그아웃 시 DEK 안전 삭제)

### 알려진 제한사항
- 비밀번호 분실 시 데이터 복구 불가 (복구 키 없음)
- 비밀번호 변경 기능 UI 미구현 (백엔드만 준비됨)
- 파트너 공유 기능 미구현 (v1.3 예정)
- 기존 평문 데이터 자동 마이그레이션 미구현

### 참고 문서
- MIGRATION_v1.2.md: 마이그레이션 가이드
- RELEASE_NOTES_v1.2.0.md: 상세 릴리스 노트

---

## [1.1.0] - 2025-01-12

### 새로운 기능
- Firebase Authentication 통합 (로그인/회원가입)
- Firebase Realtime Database 연동
- Topic 및 TenTen CRUD 기능
- Feeling, Prayer, Partner 페이지 구현
- Android/Windows Back 키 커스텀 네비게이션
- 종료 확인 다이얼로그

### 개선사항
- 사용자 정보 표시 및 로그아웃 기능
- Firebase 보안 규칙 강화
- 네비게이션 안정성 개선
- 코드 품질 개선 (빌드 경고 수정)

### 버그 수정
- NavigationManager 초기화 오류 수정
- Back 키 네비게이션 안정화
- 미사용 예외 변수 경고 수정
- 접근 불가능한 코드 제거

### 알려진 제한사항
- 데이터가 평문으로 저장됨 (v1.2에서 End-to-End 암호화 예정)
- 파트너 연결 기능 미구현 (향후 추가 예정)
- 오프라인 지원 미구현

---

## [1.0.0] - 2024-XX-XX

### 초기 릴리스
- 기본 프로젝트 구조
- 로컬 파일 시스템 기반 데이터 저장
- Topic 및 TenTen 기본 CRUD

