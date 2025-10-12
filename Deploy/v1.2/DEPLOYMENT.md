# MeTenTen 배포 가이드

## v1.1.0 배포 방법

### Android APK 빌드

#### 방법 1: Debug APK (테스트용)
```bash
dotnet build MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-android -c Debug
```

빌드된 APK 위치:
```
MeTenTenMaui/bin/Debug/net9.0-android/com.metenten.app-Signed.apk
```

#### 방법 2: Release APK (배포용)
```bash
dotnet publish MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-android -c Release
```

빌드된 APK 위치:
```
MeTenTenMaui/bin/Release/net9.0-android/publish/com.metenten.app-Signed.apk
```

#### 방법 3: 서명된 APK (Google Play Store용)

1. 키스토어 생성 (최초 1회):
```bash
keytool -genkey -v -keystore metenten.keystore -alias metenten -keyalg RSA -keysize 2048 -validity 10000
```

2. 서명 설정을 프로젝트 파일에 추가:
```xml
<PropertyGroup Condition="'$(Configuration)' == 'Release'">
  <AndroidKeyStore>True</AndroidKeyStore>
  <AndroidSigningKeyStore>path/to/metenten.keystore</AndroidSigningKeyStore>
  <AndroidSigningKeyAlias>metenten</AndroidSigningKeyAlias>
  <AndroidSigningKeyPass>your-password</AndroidSigningKeyPass>
  <AndroidSigningStorePass>your-password</AndroidSigningStorePass>
</PropertyGroup>
```

3. 빌드:
```bash
dotnet publish MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-android -c Release
```

### Windows 앱 빌드

#### 방법 1: 개발/테스트용
```bash
dotnet build MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0 -c Debug
dotnet run --project MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0
```

#### 방법 2: Release 빌드
```bash
dotnet publish MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0 -c Release
```

빌드된 실행 파일 위치:
```
MeTenTenMaui/bin/Release/net9.0-windows10.0.19041.0/win10-x64/publish/MeTenTenMaui.exe
```

#### 방법 3: MSIX 패키지 (Microsoft Store용)

1. `MeTenTenMaui.csproj`에서 패키지 타입 변경:
```xml
<WindowsPackageType>MSIX</WindowsPackageType>
<WindowsAppSDKSelfContained>true</WindowsAppSDKSelfContained>
```

2. 인증서 생성 또는 추가

3. 빌드:
```bash
dotnet publish MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-windows10.0.19041.0 -c Release -p:RuntimeIdentifierOverride=win10-x64
```

### iOS 앱 빌드 (Mac 필요)

```bash
dotnet build MeTenTenMaui/MeTenTenMaui.csproj -f net9.0-ios -c Release
```

## 빌드 최적화 옵션

### APK 크기 줄이기

`.csproj` 파일에 추가:
```xml
<PropertyGroup Condition="'$(Configuration)' == 'Release'">
  <AndroidLinkMode>Full</AndroidLinkMode>
  <AndroidEnableProguard>true</AndroidEnableProguard>
  <AndroidUseAapt2>true</AndroidUseAapt2>
  <AndroidCreatePackagePerAbi>true</AndroidCreatePackagePerAbi>
</PropertyGroup>
```

### 지원 아키텍처 지정

특정 아키텍처만 빌드:
```bash
dotnet publish -f net9.0-android -c Release -r android-arm64
dotnet publish -f net9.0-android -c Release -r android-x64
```

## 배포 체크리스트

### 배포 전
- [ ] 버전 번호 업데이트 (`ApplicationDisplayVersion`, `ApplicationVersion`)
- [ ] Firebase 프로덕션 설정 확인
- [ ] 보안 규칙 배포 확인
- [ ] API 키 보안 확인
- [ ] 모든 기능 테스트 완료
- [ ] CHANGELOG 업데이트
- [ ] README 업데이트
- [ ] Git 태그 생성

### Android 배포
- [ ] Release APK 빌드 성공
- [ ] APK 서명 확인
- [ ] 설치 및 실행 테스트
- [ ] 권한 확인
- [ ] Back 키 동작 확인

### Windows 배포
- [ ] Release 빌드 성공
- [ ] 실행 파일 테스트
- [ ] 종속성 포함 확인
- [ ] 설치 프로그램 생성 (선택사항)

## 배포 후
- [ ] Git 태그 푸시
- [ ] Release 노트 작성
- [ ] 사용자에게 업데이트 안내
- [ ] 버그 모니터링

## 문제 해결

### 빌드 실패 시
1. 모든 NuGet 패키지 복원:
   ```bash
   dotnet restore MeTenTenMaui/MeTenTenMaui.csproj
   ```

2. 캐시 정리:
   ```bash
   dotnet clean MeTenTenMaui/MeTenTenMaui.csproj
   ```

3. obj 및 bin 폴더 삭제 후 재빌드

### Android SDK 경로 오류
환경 변수 설정:
```bash
$env:ANDROID_HOME = "C:\Program Files (x86)\Android\android-sdk"
```

### 서명 오류
키스토어 비밀번호 확인 또는 새 키스토어 생성

## 참고 자료

- [.NET MAUI 배포 공식 문서](https://learn.microsoft.com/en-us/dotnet/maui/deployment/)
- [Android 서명 가이드](https://learn.microsoft.com/en-us/dotnet/maui/android/deployment/overview)
- [Windows 패키징 가이드](https://learn.microsoft.com/en-us/dotnet/maui/windows/deployment/overview)

