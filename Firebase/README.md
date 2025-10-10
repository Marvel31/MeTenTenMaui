# Firebase 설정 및 보안 가이드

## 📁 이 폴더의 파일들

### 1. `database.rules.json`
Firebase Realtime Database 보안 규칙 파일입니다.

**적용 방법**:
1. Firebase Console → Realtime Database → 규칙
2. 파일 내용을 복사하여 붙여넣기
3. "게시" 버튼 클릭

### 2. `SECURITY_RULES_SETUP.md`
보안 규칙 설정 방법 및 설명을 담은 상세 가이드입니다.

**내용**:
- 보안 규칙 적용 방법
- 각 규칙의 의미 설명
- 주의사항

### 3. `INDEXES_SETUP.md`
Firebase 인덱스 설정 및 성능 최적화 가이드입니다.

**내용**:
- 인덱스 설정 방법
- 성능 최적화 팁
- 쿼리 최적화 예시

### 4. `SECURITY_TESTING.md`
보안 규칙 테스트 방법 및 체크리스트입니다.

**내용**:
- 시뮬레이터 테스트 방법
- 앱에서 실제 테스트 시나리오
- 문제 해결 가이드

---

## 🚀 빠른 시작

### 1단계: 보안 규칙 적용

```bash
# Firebase Console 접속
https://console.firebase.google.com

# Realtime Database → 규칙 탭
# database.rules.json 내용을 붙여넣고 "게시"
```

### 2단계: 인덱스 확인

보안 규칙에 `.indexOn` 설정이 포함되어 있으므로 자동으로 생성됩니다.

### 3단계: 테스트

1. 앱 실행
2. 로그인
3. 10&10 주제 생성/조회/수정
4. Debug 로그 확인

---

## 🔒 보안 규칙 요약

### 핵심 원칙

1. **사용자 격리**: 각 사용자는 본인 데이터만 접근 가능
2. **인증 필수**: 모든 작업에 Firebase Authentication 필요
3. **데이터 유효성**: 필수 필드 및 타입 검증

### 규칙 구조

```
/topics
  /$userId (사용자 UID)
    읽기: $userId === auth.uid
    쓰기: $userId === auth.uid
    인덱스: topicDate, createdAt

/tentens
  /$userId (사용자 UID)
    읽기: $userId === auth.uid
    쓰기: $userId === auth.uid
    인덱스: createdAt, topicId

/users
  /$userId (사용자 UID)
    읽기: $userId === auth.uid
    쓰기: $userId === auth.uid
```

---

## 📊 현재 Firebase 설정

### 프로젝트 정보
- **프로젝트 ID**: metenten-d4a6f
- **Database URL**: https://metenten-d4a6f-default-rtdb.asia-southeast1.firebasedatabase.app/
- **리전**: asia-southeast1 (싱가포르)

### 인증 방법
- ✅ 이메일/비밀번호
- ❌ Google (비활성화)
- ❌ 기타 SNS (비활성화)

### 무료 플랜 제한
- **Realtime Database**: 1GB 저장공간, 동시 연결 100개
- **Authentication**: 무제한 사용자
- **데이터 다운로드**: 10GB/월

---

## ⚠️ 중요 보안 체크

### 프로덕션 배포 전 확인

- [ ] 테스트 모드 규칙 제거 완료
- [ ] 사용자별 데이터 격리 확인
- [ ] 모든 API 엔드포인트 권한 테스트
- [ ] 인증 토큰 검증 확인
- [ ] 데이터 유효성 검증 테스트

### 정기적인 보안 점검

- 월 1회: 보안 규칙 리뷰
- 분기 1회: 접근 로그 분석
- 이상 징후 모니터링

---

## 📞 문제 발생 시

1. **Firebase Console → Realtime Database → 사용량**
   - 비정상적인 트래픽 패턴 확인

2. **Debug 로그 확인**
   - `[Firebase]`, `[Auth]` 태그로 검색
   - 오류 메시지 분석

3. **보안 규칙 시뮬레이터**
   - 예상과 다른 결과가 나오면 규칙 재검토

---

## 📚 참고 자료

- [SECURITY_RULES_SETUP.md](./SECURITY_RULES_SETUP.md) - 보안 규칙 설정 가이드
- [INDEXES_SETUP.md](./INDEXES_SETUP.md) - 인덱스 설정 가이드
- [SECURITY_TESTING.md](./SECURITY_TESTING.md) - 보안 테스트 가이드
- [Firebase 공식 문서](https://firebase.google.com/docs)

