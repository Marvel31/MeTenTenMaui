# Firebase 인덱스 설정 가이드

## 📊 개요

Firebase Realtime Database의 인덱스를 설정하여 쿼리 성능을 최적화합니다.

## 🚀 인덱스 필요성

### 인덱스가 없을 때

- 데이터가 많아지면 쿼리가 느려짐
- Firebase Console에서 "인덱스 필요" 경고 발생
- 정렬 및 필터링 성능 저하

### 인덱스가 있을 때

- 빠른 쿼리 실행
- 효율적인 정렬 및 필터링
- 대량 데이터 처리 가능

---

## 🔧 인덱스 설정 방법

### 방법 1: 보안 규칙에 포함 (권장)

`database.rules.json` 파일에 이미 포함되어 있습니다:

```json
{
  "rules": {
    "topics": {
      "$userId": {
        ".indexOn": ["topicDate", "createdAt"]
      }
    },
    "tentens": {
      "$userId": {
        ".indexOn": ["createdAt", "topicId"]
      }
    }
  }
}
```

보안 규칙을 적용하면 인덱스도 자동으로 생성됩니다.

### 방법 2: Firebase Console에서 직접 추가

1. Firebase Console → Realtime Database → "데이터" 탭
2. 쿼리 실행 시 "인덱스 필요" 경고 표시
3. 경고 메시지의 "인덱스 추가" 링크 클릭
4. 자동으로 인덱스 생성

---

## 📋 MeTenTen 앱의 인덱스 설정

### Topics 인덱스

**경로**: `/topics/$userId`

**인덱스 필드**:
- `topicDate`: 월별 필터링에 사용
- `createdAt`: 생성 시간순 정렬에 사용

**사용 예시**:
```csharp
// 특정 월의 Topics 조회
var topics = await _firebaseClient
    .Child("topics")
    .Child(userId)
    .OrderBy("topicDate")
    .StartAt("2025-01-01")
    .EndAt("2025-01-31")
    .OnceAsync<FirebaseTopic>();
```

### TenTens 인덱스

**경로**: `/tentens/$userId`

**인덱스 필드**:
- `createdAt`: 최신순 정렬에 사용
- `topicId`: 특정 Topic의 TenTens 필터링에 사용

**사용 예시**:
```csharp
// 특정 Topic의 TenTens 조회
var tentens = await _firebaseClient
    .Child("tentens")
    .Child(userId)
    .OrderBy("topicId")
    .EqualTo(topicId)
    .OnceAsync<FirebaseTenTen>();
```

---

## ✅ 인덱스 확인 방법

### Firebase Console에서 확인

1. Realtime Database → "규칙" 탭
2. `.indexOn` 설정이 포함되어 있는지 확인
3. "게시" 버튼 클릭 시 인덱스 생성됨

### 앱에서 확인

1. 앱 실행 후 Topics 조회
2. Debug 로그에 인덱스 경고가 없으면 정상
3. 경고 발생 시:
   ```
   [Firebase] Warning: Using unindexed query on path /topics/...
   Consider adding an index for better performance
   ```

---

## 🎯 성능 최적화 팁

### 1. 필요한 인덱스만 생성

- 사용하지 않는 필드에는 인덱스 불필요
- 인덱스가 많으면 쓰기 성능 저하

### 2. 복합 인덱스 고려

여러 필드로 정렬/필터링하는 경우:

```json
".indexOn": ["topicDate", "isActive"]
```

### 3. 페이지네이션 사용

대량 데이터 조회 시:

```csharp
var topics = await _firebaseClient
    .Child("topics")
    .Child(userId)
    .OrderBy("createdAt")
    .LimitToLast(20) // 최근 20개만 조회
    .OnceAsync<FirebaseTopic>();
```

---

## 🔍 인덱스 모니터링

### Firebase Console에서 모니터링

1. Realtime Database → "사용량" 탭
2. 쿼리 성능 지표 확인
3. 느린 쿼리 식별

### 앱에서 성능 측정

```csharp
var stopwatch = System.Diagnostics.Stopwatch.StartNew();
var topics = await GetTopicsAsync();
stopwatch.Stop();
System.Diagnostics.Debug.WriteLine($"[Performance] Topics loaded in {stopwatch.ElapsedMilliseconds}ms");
```

---

## 📚 참고 자료

- [Firebase 인덱싱 가이드](https://firebase.google.com/docs/database/security/indexing-data)
- [쿼리 최적화](https://firebase.google.com/docs/database/rest/retrieve-data)
- [성능 모니터링](https://firebase.google.com/docs/database/usage/limits)

---

## ⚠️ 주의사항

1. **인덱스 변경 후**
   - 규칙 게시 후 몇 분 정도 지연 가능
   - 캐시 클리어 후 테스트 권장

2. **무료 플랜 제한**
   - Realtime Database: 동시 연결 100개
   - 데이터 다운로드: 10GB/월
   - 인덱스 개수: 제한 없음

3. **프로덕션 배포 전**
   - 모든 쿼리가 인덱스를 사용하는지 확인
   - 성능 테스트 필수

