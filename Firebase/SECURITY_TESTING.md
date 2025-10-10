# Firebase 보안 규칙 테스트 가이드

## 🧪 개요

Firebase 보안 규칙이 올바르게 작동하는지 테스트하는 방법을 안내합니다.

---

## 📋 테스트 체크리스트

### ✅ 1. Firebase Console 시뮬레이터 테스트

#### 접속 방법
1. Firebase Console → Realtime Database → "규칙" 탭
2. 우측 상단 "시뮬레이터" 버튼 클릭

#### 테스트 시나리오

**시나리오 1: 인증된 사용자가 본인 데이터 읽기 (✅ 성공)**
```
위치: /topics/guf814Xme1bAcDlN4zknclFLP8Q2/topic123
인증: Authenticated
인증 UID: guf814Xme1bAcDlN4zknclFLP8Q2
작업: Read
결과: ✅ Allow
```

**시나리오 2: 인증된 사용자가 타인 데이터 읽기 (❌ 거부)**
```
위치: /topics/OTHER_USER_ID/topic123
인증: Authenticated
인증 UID: guf814Xme1bAcDlN4zknclFLP8Q2
작업: Read
결과: ❌ Deny
이유: "$userId === auth.uid" 조건 불일치
```

**시나리오 3: 미인증 사용자 데이터 읽기 (❌ 거부)**
```
위치: /topics/guf814Xme1bAcDlN4zknclFLP8Q2/topic123
인증: Unauthenticated
작업: Read
결과: ❌ Deny
이유: auth.uid가 null
```

**시나리오 4: 인증된 사용자가 본인 데이터 쓰기 (✅ 성공)**
```
위치: /topics/guf814Xme1bAcDlN4zknclFLP8Q2/topic123
인증: Authenticated
인증 UID: guf814Xme1bAcDlN4zknclFLP8Q2
작업: Write
데이터: {"subject": "테스트", "topicDate": "2025-01-01", "createdAt": "2025-01-01T00:00:00Z", "isActive": true}
결과: ✅ Allow
```

**시나리오 5: 잘못된 데이터 형식 쓰기 (❌ 거부)**
```
위치: /topics/guf814Xme1bAcDlN4zknclFLP8Q2/topic123
인증: Authenticated
인증 UID: guf814Xme1bAcDlN4zknclFLP8Q2
작업: Write
데이터: {"subject": "테스트"} // 필수 필드 누락
결과: ❌ Deny
이유: ".validate" 조건 불일치 (필수 필드 누락)
```

---

### ✅ 2. 앱에서 실제 테스트

#### 테스트 1: 정상 동작 확인

1. **로그인**
   - 이메일/비밀번호로 로그인
   - `[Auth] 로그인 성공` 로그 확인

2. **데이터 읽기**
   - 10&10 주제 목록 조회
   - `[Firebase] Loaded X topics` 로그 확인

3. **데이터 쓰기**
   - 새 주제 생성
   - `[Firebase] Created topic` 로그 확인

4. **데이터 수정**
   - 기존 주제 수정
   - `[Firebase] Updated topic` 로그 확인

5. **데이터 삭제**
   - 주제 삭제 (IsActive = false)
   - `[Firebase] Updated topic` 로그 확인

#### 테스트 2: 권한 오류 시뮬레이션

**방법 1: 로그아웃 후 데이터 접근 시도**

1. 로그아웃
2. 브라우저 개발자 도구에서 직접 Firebase API 호출
3. 예상 결과: 401 Unauthorized 또는 Permission Denied

**방법 2: Firebase Console에서 직접 테스트**

1. Firebase Console → Realtime Database → "데이터" 탭
2. 다른 사용자의 데이터 경로로 이동
3. 데이터 수정 시도
4. 예상 결과: "Permission Denied" 오류

#### 테스트 3: 데이터 격리 확인

1. **계정 A로 로그인**
   - 10&10 주제 생성: "계정 A의 주제"
   - Firebase에서 확인: `/topics/USER_A_ID/...`

2. **로그아웃 후 계정 B로 로그인**
   - 10&10 주제 목록 조회
   - 예상 결과: 계정 A의 주제가 보이지 않음
   - Firebase에서 확인: `/topics/USER_B_ID/...`

3. **계정 B에서 계정 A 데이터 접근 시도**
   - 브라우저 개발자 도구에서 직접 API 호출
   - 예상 결과: Permission Denied

---

### ✅ 3. 네트워크 로그 확인

#### 브라우저 개발자 도구 (Windows 앱)

1. F12 → Network 탭
2. 앱에서 데이터 조회/생성
3. Firebase API 호출 확인:
   ```
   GET https://metenten-d4a6f-default-rtdb.asia-southeast1.firebasedatabase.app/topics/USER_ID.json?auth=TOKEN
   Status: 200 OK
   ```

#### 권한 오류 시:
```
GET https://...topics/OTHER_USER_ID.json?auth=TOKEN
Status: 401 Unauthorized
Response: {"error": "Permission denied"}
```

---

## 🔍 문제 해결

### 문제 1: "Permission Denied" 오류

**증상**: 로그인했는데도 데이터 접근 불가

**원인**:
- 보안 규칙이 잘못 설정됨
- `auth.uid`와 `$userId`가 일치하지 않음
- 토큰 만료

**해결 방법**:
1. Firebase Console에서 규칙 확인
2. 로그아웃 후 재로그인
3. Debug 로그에서 `CurrentUserId` 확인

### 문제 2: "Index not defined" 경고

**증상**: Firebase Console에 인덱스 경고

**원인**:
- `.indexOn` 설정이 규칙에 없음
- 규칙 게시 안됨

**해결 방법**:
1. `database.rules.json` 파일 확인
2. Firebase Console에서 규칙 게시
3. 몇 분 후 재시도

### 문제 3: "Validation failed" 오류

**증상**: 데이터 쓰기 시 거부

**원인**:
- 필수 필드 누락
- 데이터 타입 불일치

**해결 방법**:
1. Firebase Console → "규칙" 탭에서 `.validate` 조건 확인
2. 앱 코드에서 모든 필수 필드 전송 확인
3. 데이터 타입 확인 (string, boolean 등)

---

## 📊 성능 테스트

### 인덱스 성능 비교

**인덱스 없을 때**:
```
[Performance] Topics loaded in 1500ms (1.5초)
```

**인덱스 있을 때**:
```
[Performance] Topics loaded in 150ms (0.15초)
```

### 테스트 방법

```csharp
var stopwatch = System.Diagnostics.Stopwatch.StartNew();
var topics = await _topicService.GetTopicsAsync();
stopwatch.Stop();
System.Diagnostics.Debug.WriteLine($"[Performance] Loaded {topics.Count} topics in {stopwatch.ElapsedMilliseconds}ms");
```

---

## 🚀 프로덕션 체크리스트

배포 전 확인사항:

- [ ] 보안 규칙이 프로덕션 환경에 적용됨
- [ ] 모든 인덱스가 생성됨
- [ ] 권한 테스트 통과
- [ ] 데이터 격리 확인
- [ ] 성능 테스트 통과
- [ ] 오류 처리 테스트 완료

---

## 📚 추가 참고 자료

- [Firebase 보안 베스트 프랙티스](https://firebase.google.com/docs/rules/best-practices)
- [Realtime Database 제한사항](https://firebase.google.com/docs/database/usage/limits)
- [보안 규칙 디버깅](https://firebase.google.com/docs/database/security/rules-conditions#debugging)

