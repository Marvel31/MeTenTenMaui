# MeTenTen v1.2 마이그레이션 가이드

## 🔐 암호화 기능 추가 (DEK 방식)

v1.2부터는 사용자의 10&10 내용이 **DEK(Data Encryption Key) 방식**으로 AES-256 암호화되어 Firebase에 저장됩니다.

## 💡 DEK 방식의 장점

### 비밀번호 변경 시에도 데이터 유지!
- ✅ **비밀번호 변경해도 기존 데이터 그대로 사용 가능**
- ✅ 모든 데이터를 재암호화할 필요 없음
- ✅ 비밀번호 변경은 1번의 작업만 필요 (DEK 재암호화)

### 동작 원리
1. **DEK (Data Encryption Key)**: 실제 데이터를 암호화하는 랜덤 키
2. **비밀번호**: DEK를 암호화하는 용도로만 사용
3. **비밀번호 변경 시**: DEK만 새 비밀번호로 재암호화 (데이터는 그대로)

---

## ⚠️ 중요: 기존 데이터에 대한 안내

### 기존 평문 데이터 처리

v1.1.0 이하 버전에서 작성한 10&10 데이터는 **평문(암호화되지 않음)**으로 Firebase에 저장되어 있습니다.

v1.2로 업데이트 후:
- **신규로 작성하는 모든 10&10은 자동으로 암호화됩니다**
- **기존 평문 데이터는 암호화되지 않은 상태로 유지됩니다**
- 기존 평문 데이터를 수정하면 암호화되어 저장됩니다

### 권장 사항

보안을 위해 다음 중 하나를 선택하시기 바랍니다:

#### 옵션 1: 기존 데이터 삭제 (권장)
1. v1.2 업데이트 전에 필요한 데이터를 백업
2. Firebase Console에서 기존 데이터 삭제
3. v1.2로 업데이트 후 새로 작성

#### 옵션 2: 기존 데이터 유지
- 기존 평문 데이터를 읽을 수 있습니다
- 보안이 중요한 내용은 다시 작성하여 암호화하세요

#### 옵션 3: 수동 마이그레이션
1. 앱에서 기존 10&10을 하나씩 열어서 내용 복사
2. 삭제 후 다시 작성 (자동으로 암호화됨)

---

## 🔑 암호화 작동 방식 (DEK)

### 1. 회원가입 시
```
1. 랜덤한 DEK(256-bit) 생성
2. DEK를 사용자 비밀번호로 암호화 (PBKDF2 + AES-256)
3. 암호화된 DEK를 Firebase users/{userId}/encryptedDEK에 저장
4. DEK를 메모리에 로드 (데이터 암호화에 사용)
```

### 2. 로그인 시
```
1. Firebase에서 암호화된 DEK 가져오기
2. 사용자 비밀번호로 DEK 복호화
3. 복호화된 DEK를 메모리에 로드
4. 이후 모든 데이터는 DEK로 암호화/복호화
```

### 3. 데이터 저장 시
```
1. TenTen 내용을 DEK로 암호화
2. 암호화된 데이터를 Firebase에 저장
3. DEK는 비밀번호로 보호됨
```

### 4. 비밀번호 변경 시 (미래 기능)
```
1. 기존 비밀번호로 DEK 복호화
2. 새 비밀번호로 DEK 재암호화
3. 새 암호화된 DEK를 Firebase에 저장
4. 모든 TenTen 데이터는 그대로 유지! (재암호화 불필요)
```

### 암호화 알고리즘
- **DEK 생성**: 랜덤 256-bit 키 (회원가입 시 1회만)
- **DEK 암호화**: PBKDF2 (100,000 iterations, SHA-256) + AES-256-CBC
- **데이터 암호화**: AES-256-CBC (DEK 사용)
- **IV**: 각 암호화마다 랜덤 생성
- **저장 형식**: Base64(IV + EncryptedData)

### 보안 특성
- ✅ Firebase 관리자도 암호화된 내용을 읽을 수 없음
- ✅ 사용자의 비밀번호를 알아야만 DEK 복호화 가능
- ✅ DEK를 알아야만 데이터 복호화 가능
- ✅ 로그아웃 시 메모리에서 DEK 자동 삭제
- ✅ **비밀번호 변경 가능** (데이터 유지)

---

## ⚠️ 주의 사항

### 비밀번호 관리
- **비밀번호를 잊어버리면 DEK를 복호화할 수 없어 데이터를 복구할 수 없습니다**
- 비밀번호는 안전한 곳에 별도로 기록해두세요
- ✅ **v1.2부터는 비밀번호 변경 시에도 데이터 유지 가능** (DEK 방식)

### 데이터 백업
- 중요한 내용은 별도로 백업하세요
- Firebase Console에서도 암호화된 데이터는 읽을 수 없습니다

### 계정 공유 금지
- DEK는 비밀번호로 보호됩니다
- 다른 사람과 계정을 공유하지 마세요

---

## 🔧 문제 해결

### "[복호화 실패]" 메시지가 표시됨
- **원인**: 잘못된 비밀번호로 로그인했거나 데이터가 손상됨
- **해결**: 
  1. 로그아웃 후 올바른 비밀번호로 다시 로그인
  2. 문제가 계속되면 해당 10&10을 삭제하고 다시 작성

### 기존 데이터를 읽을 수 없음
- **원인**: v1.1.0 이하에서 작성한 평문 데이터는 IsEncrypted=false로 표시되어야 함
- **해결**: Firebase Console에서 해당 데이터의 `isEncrypted` 필드를 `false`로 설정

### 비밀번호를 잊어버렸음
- **결과**: DEK를 복호화할 수 없어 암호화된 데이터를 복구할 수 없습니다
- **대응**: 
  1. Firebase Authentication에서 비밀번호 재설정
  2. 기존 암호화 데이터는 포기
  3. 새로 작성하는 데이터는 새 DEK와 새 비밀번호로 암호화됨

### v1.1 사용자가 v1.2로 업데이트 시
- **자동 처리**: 첫 로그인 시 자동으로 DEK 생성
- 기존 평문 데이터는 `isEncrypted: false`로 읽기 가능
- 이후 작성하는 데이터는 DEK로 암호화

---

## 📝 개발자 참고

### Firebase 데이터 구조
```json
{
  "users": {
    "userId123": {
      "email": "user@example.com",
      "displayName": "사용자",
      "encryptedDEK": "base64EncodedEncryptedDEK...",  // 비밀번호로 암호화된 DEK
      "createdAt": "2025-01-12T10:00:00.000Z",
      "updatedAt": "2025-01-12T11:00:00.000Z"
    }
  },
  "tentens": {
    "userId123": {
      "tentenId456": {
        "content": "DEK로 암호화된 내용...",  // DEK로 암호화
        "isEncrypted": true,
        "topicId": "1",
        "createdAt": "2025-01-12T10:30:00.000Z",
        "updatedAt": "2025-01-12T11:45:00.000Z"
      }
    }
  }
}
```

### DEK 암호화 코드 예시
```csharp
// 1. 회원가입: DEK 생성 및 암호화
var dek = await _encryptionService.GenerateRandomDEKAsync();
var encryptedDEK = await _encryptionService.EncryptDEKAsync(dek, email, password);
await _firebaseDataService.SaveUserDEKAsync(userId, email, name, encryptedDEK);
_encryptionService.SetDEK(dek);

// 2. 로그인: DEK 복호화 및 로드
var encryptedDEK = await _firebaseDataService.GetUserDEKAsync(userId);
var dek = await _encryptionService.DecryptDEKAsync(encryptedDEK, email, password);
_encryptionService.SetDEK(dek);

// 3. 데이터 암호화 (DEK 사용)
var encryptedContent = await _encryptionService.EncryptAsync(plainText);

// 4. 데이터 복호화 (DEK 사용)
var plainText = await _encryptionService.DecryptAsync(encryptedContent);
```

### 비밀번호 변경 시 (미래 구현)
```csharp
// DEK만 재암호화 (데이터는 그대로!)
var encryptedDEK = await _firebaseDataService.GetUserDEKAsync(userId);
var dek = await _encryptionService.DecryptDEKAsync(encryptedDEK, email, oldPassword);
var newEncryptedDEK = await _encryptionService.EncryptDEKAsync(dek, email, newPassword);
await _firebaseDataService.SaveUserDEKAsync(userId, email, name, newEncryptedDEK);
// 완료! 모든 TenTen 데이터는 동일한 DEK로 그대로 복호화 가능
```

---

## 📞 지원

문제가 지속되면:
- GitHub Issues에 버그 리포트
- 로그 파일 첨부 (민감한 정보 제거 후)
- Firebase Console에서 데이터 확인

---

## 🔄 v1.1 → v1.2 차이점 요약

| 항목 | v1.1 이하 | v1.2 (DEK 방식) |
|------|-----------|-----------------|
| 데이터 암호화 | ❌ 평문 저장 | ✅ AES-256 암호화 |
| 암호화 키 | - | DEK (랜덤 256-bit) |
| DEK 보호 | - | 비밀번호로 암호화 |
| 비밀번호 변경 | - | ✅ **가능** (DEK만 재암호화) |
| 기존 데이터 | 평문 | 평문 유지 (호환성) |
| 신규 데이터 | 평문 | DEK로 암호화 |
| Firebase 관리자 | 👀 읽기 가능 | 🔒 읽기 불가 |

**MeTenTen v1.2.0**  
**릴리스 날짜**: 2025-01-12  
**DEK 방식으로 안전하고 유연하게! 📝🔐✨**
