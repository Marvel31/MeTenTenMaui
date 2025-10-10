# Firebase 보안 규칙 설정 가이드

## 📋 개요

Firebase Realtime Database의 보안 규칙을 설정하여 사용자별 데이터 접근을 제한하고 데이터 무결성을 보장합니다.

## 🔒 보안 규칙 적용 방법

### 1. Firebase Console 접속

1. https://console.firebase.google.com 접속
2. "MeTenTen" 프로젝트 선택
3. 왼쪽 메뉴에서 "Build" → "Realtime Database" 클릭
4. 상단 탭에서 "규칙" 클릭

### 2. 보안 규칙 적용

현재 규칙을 `database.rules.json` 파일의 내용으로 교체합니다:

```json
{
  "rules": {
    "topics": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "$userId === auth.uid",
        ".indexOn": ["topicDate", "createdAt"],
        "$topicId": {
          ".validate": "newData.hasChildren(['subject', 'topicDate', 'createdAt', 'isActive']) && newData.child('subject').isString() && newData.child('topicDate').isString() && newData.child('createdAt').isString() && newData.child('isActive').isBoolean()"
        }
      }
    },
    "tentens": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "$userId === auth.uid",
        ".indexOn": ["createdAt", "topicId"],
        "$tentenId": {
          ".validate": "newData.hasChildren(['content', 'createdAt', 'topicId']) && newData.child('content').isString() && newData.child('createdAt').isString() && newData.child('topicId').isString()"
        }
      }
    },
    "users": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "$userId === auth.uid",
        "$field": {
          ".validate": "$field === 'email' || $field === 'displayName' || $field === 'createdAt'"
        }
      }
    }
  }
}
```

### 3. "게시" 버튼 클릭

규칙을 입력한 후 우측 상단의 "게시" 버튼을 클릭하여 적용합니다.

---

## 🛡️ 보안 규칙 설명

### 사용자별 데이터 격리

```json
"$userId": {
  ".read": "$userId === auth.uid",
  ".write": "$userId === auth.uid"
}
```

- **읽기 권한**: 본인의 데이터만 읽을 수 있음
- **쓰기 권한**: 본인의 데이터만 생성/수정/삭제 가능
- **$userId**: URL 경로의 사용자 ID
- **auth.uid**: 현재 인증된 사용자의 Firebase UID

### 데이터 유효성 검증

```json
".validate": "newData.hasChildren(['subject', 'topicDate', 'createdAt', 'isActive'])"
```

- **필수 필드**: 특정 필드가 반드시 존재해야 함
- **타입 검증**: 각 필드의 데이터 타입 확인
- **잘못된 데이터**: 규칙을 위반하면 쓰기 거부

### 인덱스 설정

```json
".indexOn": ["topicDate", "createdAt"]
```

- **성능 최적화**: 정렬 및 필터링 쿼리 가속화
- **topicDate**: 월별 필터링에 사용
- **createdAt**: 시간순 정렬에 사용

---

## 🧪 보안 규칙 테스트

### Firebase Console에서 테스트

1. Realtime Database 페이지 → "규칙" 탭
2. 우측 상단 "Simulator" 클릭
3. 시뮬레이터에서 다양한 시나리오 테스트:

**테스트 시나리오 1: 본인 데이터 읽기 (성공)**
```
위치: /topics/[YOUR_USER_ID]/topic123
인증: Authenticated (uid: [YOUR_USER_ID])
작업: Read
결과: ✅ Allowed
```

**테스트 시나리오 2: 타인 데이터 읽기 (실패)**
```
위치: /topics/OTHER_USER_ID/topic123
인증: Authenticated (uid: [YOUR_USER_ID])
작업: Read
결과: ❌ Denied
```

**테스트 시나리오 3: 미인증 사용자 (실패)**
```
위치: /topics/[YOUR_USER_ID]/topic123
인증: Unauthenticated
작업: Read
결과: ❌ Denied
```

### 앱에서 테스트

1. **정상 동작 확인**
   - 로그인 후 본인의 Topics/TenTens 조회
   - 데이터 생성/수정/삭제

2. **권한 확인**
   - 다른 계정으로 로그인하여 데이터 격리 확인
   - Firebase Console에서 직접 데이터 조회 시도

---

## 📊 인덱스 설정 확인

Firebase Console에서 인덱스가 자동으로 생성되었는지 확인:

1. Realtime Database → "데이터" 탭
2. 쿼리 실행 시 인덱스 필요 경고가 나타나면:
   - 경고 메시지의 "인덱스 추가" 링크 클릭
   - 자동으로 인덱스 생성

또는 "규칙" 탭에서 `.indexOn` 설정이 포함되어 있으면 자동으로 인덱스가 생성됩니다.

---

## ⚠️ 주의사항

1. **테스트 모드에서 프로덕션 규칙으로 전환**
   - 초기 설정 시 "테스트 모드"로 시작했다면 반드시 위 규칙으로 교체
   - 테스트 모드는 30일 후 자동으로 모든 접근 차단

2. **규칙 백업**
   - 중요한 규칙 변경 전 현재 규칙 복사/저장
   - Firebase Console에서 "이전 버전" 기능 사용 가능

3. **인증 필수**
   - 모든 데이터 접근에 Firebase Authentication 필요
   - 로그인하지 않은 사용자는 데이터 접근 불가

---

## 🚀 다음 단계

보안 규칙 적용 후:

1. ✅ 앱에서 정상 동작 확인
2. ✅ 보안 시뮬레이터에서 테스트
3. ✅ 프로덕션 배포 전 최종 검토

---

## 📚 참고 자료

- [Firebase 보안 규칙 문서](https://firebase.google.com/docs/database/security)
- [규칙 시뮬레이터 사용법](https://firebase.google.com/docs/database/security/rules-conditions)
- [인덱스 최적화 가이드](https://firebase.google.com/docs/database/security/indexing-data)

