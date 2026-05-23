# 💰 수입·예산 관리 앱

Google 계정으로 로그인해서 수입과 지출을 관리하고, 모든 기기에서 자동으로 동기화되는 개인 예산 관리 앱입니다.

---

## ✅ 바로 사용하기 (가장 쉬운 방법)

이미 배포된 앱을 그대로 사용할 수 있습니다.

👉 **https://kimminkuy70-coder.github.io/Budget/**

1. 위 주소로 접속
2. **"Google 계정으로 로그인"** 클릭
3. 본인 Google 계정 선택

> 각자 Google 계정으로 로그인하면 **데이터가 완전히 분리**됩니다.  
> 다른 사람의 데이터는 볼 수 없습니다. but 관리자는 접근 가능하기 때문에 완전히 독립된 데이터를 사용하고 싶다면 "🔧 나만의 독립 앱으로 배포하기" 사용

---

## 🔧 나만의 독립 앱으로 배포하기

> **이 방법을 선택하면 데이터가 완전히 독립됩니다.**  
> 본인이 직접 만든 Firebase 프로젝트에만 저장되므로, 앱 원본 관리자도 접근할 수 없습니다.

코딩 지식 없어도 가능합니다. 아래 순서대로 클릭만 하면 됩니다.

**필요한 것:** Google 계정(Gmail) + GitHub 계정 ([없으면 무료 가입](https://github.com))

> **전체 단계 요약**
> 1. GitHub에서 Fork → 2. Firebase 프로젝트 생성 → 3. Realtime Database 만들기
> 4. **Google 로그인 활성화** ⚠️ → 5. 보안 규칙 설정 → 6. Firebase 설정값 복사
> 7. firebase-config.js 수정 → 8. GitHub Pages 배포 → 9. **앱 도메인 등록** ⚠️

---

### STEP 1 — 이 저장소 Fork하기

> Fork = 이 프로젝트를 내 GitHub 계정으로 복사하는 것

1. 이 페이지 오른쪽 상단 **Fork** 버튼 클릭
2. **"Create fork"** 클릭
3. 완료되면 내 계정에 동일한 저장소가 생깁니다  
   → `https://github.com/내아이디/Budget`

---

### STEP 2 — Firebase 프로젝트 만들기

1. [https://console.firebase.google.com](https://console.firebase.google.com) 접속
2. **"프로젝트 추가"** 클릭
3. 프로젝트 이름 입력 (예: `my-budget`) → **계속**
4. Google Analytics: 꺼도 됨 → **"프로젝트 만들기"** → **"계속"**

---

### STEP 3 — Realtime Database 만들기

1. 왼쪽 메뉴 **"빌드" → "Realtime Database"** 클릭
2. **"데이터베이스 만들기"** 클릭
3. 위치: **`asia-southeast1 (싱가포르)`** → **다음**
4. **"테스트 모드에서 시작"** → **"완료"**

---

### STEP 4 — Google 로그인 활성화 ⚠️ 필수 (건너뛰면 로그인 불가)

1. 왼쪽 메뉴 **"빌드" → "Authentication"** 클릭
2. **"시작하기"** 클릭
3. **"Sign-in method"** 탭 → **"Google"** 클릭
4. 오른쪽 상단 토글을 **켜기(파란색)** 로 변경
5. 프로젝트 지원 이메일 선택 (본인 Gmail) → **"저장"**

> 이 단계를 건너뛰면 `auth/configuration-not-found` 오류가 발생합니다.

---

### STEP 5 — Firebase 보안 규칙 설정

> 이 설정으로 본인 데이터를 완전히 잠급니다. 다른 사람은 접근 불가능합니다.

1. 왼쪽 메뉴 **"Realtime Database" → 상단 탭 "규칙"** 클릭
2. 아래 내용으로 **전체 교체** 후 **"게시"** 클릭:

```json
{
  "rules": {
    "budgetApp": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        ".write": "auth != null && auth.uid === $uid"
      }
    }
  }
}
```

---

### STEP 6 — Firebase 설정값 복사

1. 왼쪽 상단 ⚙️ **"프로젝트 설정"** 클릭
2. 아래로 스크롤 → **"앱 추가"** → 웹 아이콘 **`</>`** 클릭
3. 앱 닉네임 입력 → **"앱 등록"**
4. **`<script> 태그 사용`** 탭 클릭
5. 아래와 같이 생긴 값들을 **메모장에 복사**해 두기:

```
apiKey: "AIzaSy..."
authDomain: "my-budget-xxxxx.firebaseapp.com"
databaseURL: "https://my-budget-xxxxx-default-rtdb...."
projectId: "my-budget-xxxxx"
storageBucket: "my-budget-xxxxx.firebasestorage.app"
messagingSenderId: "123456789"
appId: "1:123456789:web:abcdef"
```

6. **"콘솔로 이동"** 클릭

---

### STEP 7 — firebase-config.js 수정 (⭐ 딱 이 파일만 수정하면 됩니다)

1. 내 GitHub 저장소(`https://github.com/내아이디/Budget`)로 이동
2. **`firebase-config.js`** 파일 클릭
3. 오른쪽 상단 연필(✏️) 아이콘 클릭
4. 파일 안의 값들을 **STEP 6에서 복사한 내 값으로 교체**:

```javascript
window.FIREBASE_CONFIG = {
  apiKey: "여기를 내 값으로",
  authDomain: "여기를 내 값으로",
  databaseURL: "여기를 내 값으로",
  projectId: "여기를 내 값으로",
  storageBucket: "여기를 내 값으로",
  messagingSenderId: "여기를 내 값으로",
  appId: "여기를 내 값으로"
};
```

5. 오른쪽 상단 녹색 **"Commit changes"** → 팝업에서 **"Commit changes"** 클릭

---

### STEP 8 — GitHub Pages 배포

1. 내 저장소 상단 **"Settings"** 탭 클릭
2. 왼쪽 메뉴 **"Pages"** 클릭
3. **Branch**: `main` 선택, 폴더 `/ (root)` → **"Save"**
4. 1~3분 후 주소가 나타납니다  
   👉 `https://내아이디.github.io/Budget/`

---

### STEP 9 — Firebase에 내 앱 주소 등록 ⚠️ 필수 (건너뛰면 로그인 불가)

> 이 설정을 해야 Google 로그인이 내 앱에서 동작합니다.

1. 왼쪽 메뉴 **"Authentication"** 클릭
2. 상단 **"Settings"** 탭 클릭
3. 스크롤 내려서 **"승인된 도메인(Authorized domains)"** 섹션 찾기
4. **"도메인 추가"** 클릭
5. `내아이디.github.io` 입력 → **"추가"**

> 이 단계를 건너뛰면 Google 로그인 화면으로 이동했다가 오류와 함께 돌아옵니다.

---

### 🎉 완료!

`https://내아이디.github.io/Budget/` 접속 → Google 로그인 → 사용 시작!

---

## 🔄 앱 업데이트 방법 (Sync fork)

원본 앱이 업데이트되면 내 앱에도 반영할 수 있습니다.

1. 내 저장소(`https://github.com/내아이디/Budget`)로 이동
2. **"Sync fork"** 버튼 클릭 → **"Update branch"** 클릭
3. GitHub Pages가 자동으로 재배포됩니다 (2~3분)

> ⚠️ **Sync fork 후 firebase-config.js가 초기화될 수 있습니다.**  
> 이 경우 **STEP 7을 다시 한 번** 해주세요. (7줄짜리 파일만 수정하면 됩니다)

---

## 자주 묻는 질문

**Q. 요금이 드나요?**  
개인 사용 수준은 Firebase 무료 플랜(Spark)으로 충분합니다. 저장공간 1GB, 월 다운로드 10GB까지 무료입니다.

**Q. 내 데이터를 다른 사람이 볼 수 있나요?**  
STEP 5 보안 규칙을 설정하면 본인 Google 계정으로만 접근 가능합니다.  
독립 배포(방법 2) 사용자는 Firebase 프로젝트 자체가 분리되어 있으므로 원본 관리자도 접근할 수 없습니다.

**Q. 데이터가 사라졌어요.**  
같은 Google 계정으로 로그인하면 어느 기기에서든 데이터가 복원됩니다.

**Q. 핸드폰에서도 사용할 수 있나요?**  
브라우저에서 접속하면 됩니다. Chrome에서 "홈 화면에 추가"를 하면 앱처럼 사용할 수 있습니다.
