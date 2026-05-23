# 💰 수입·예산 관리 앱

Google 계정으로 로그인해서 수입과 지출을 관리하고, 모든 기기에서 자동으로 동기화되는 개인 예산 관리 앱입니다.

---

## ✅ 바로 사용하기 (가장 쉬운 방법)

이미 배포된 앱을 그대로 사용할 수 있습니다.

👉 **https://kimminkuy70-coder.github.io/Budget/**

1. 위 주소로 접속
2. **"Google 계정으로 로그인"** 클릭
3. 본인 Google 계정 선택

> 각자 Google 계정으로 로그인하면 **데이터가 완전히 분리**됩니다. 다른 사람의 데이터는 볼 수 없습니다.

---

## 🔧 나만의 독립 앱으로 배포하기

다른 사람과 아예 다른 서버·데이터베이스를 사용하고 싶다면 아래 가이드를 따라 본인만의 앱을 배포할 수 있습니다.  
**코딩 지식 없어도 가능합니다. 클릭만으로 설정할 수 있어요.**

---

### 필요한 것

- Google 계정 (Gmail 있으면 됩니다)
- GitHub 계정 (없으면 https://github.com 에서 무료 가입)

---

### STEP 1 — 이 저장소 Fork하기

> "Fork"란 이 프로젝트를 내 GitHub 계정으로 복사하는 것입니다.

1. 이 페이지 오른쪽 상단의 **Fork** 버튼 클릭
2. **"Create fork"** 버튼 클릭
3. 잠시 기다리면 내 계정에 동일한 저장소가 생깁니다  
   주소: `https://github.com/내아이디/Budget`

---

### STEP 2 — Firebase 프로젝트 만들기

1. https://console.firebase.google.com 접속 (Google 계정으로 로그인)
2. **"프로젝트 추가"** 클릭
3. 프로젝트 이름 입력 (예: `my-budget`) → **계속**
4. Google Analytics: 사용 안 함으로 꺼도 됨 → **"프로젝트 만들기"**
5. 프로젝트 생성 완료 → **"계속"** 클릭

---

### STEP 3 — Realtime Database 만들기

1. 왼쪽 메뉴에서 **"빌드"** → **"Realtime Database"** 클릭
2. **"데이터베이스 만들기"** 클릭
3. 위치: **`asia-southeast1 (싱가포르)`** 선택 → **다음**
4. **"테스트 모드에서 시작"** 선택 → **"완료"**

---

### STEP 4 — Google 로그인 활성화

1. 왼쪽 메뉴에서 **"빌드"** → **"Authentication"** 클릭
2. **"시작하기"** 클릭
3. **"Google"** 클릭
4. 오른쪽 상단 토글을 **켜기(파란색)** 로 변경
5. 프로젝트 지원 이메일 선택 (본인 Gmail)
6. **"저장"** 클릭

---

### STEP 5 — Firebase 보안 규칙 설정

> 이 설정을 해야 본인 데이터만 접근 가능하고 다른 사람은 볼 수 없습니다.

1. 왼쪽 메뉴 **"Realtime Database"** → 상단 탭 **"규칙"** 클릭
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

### STEP 6 — Firebase 앱 등록 및 설정값 복사

1. 왼쪽 상단 ⚙️ **"프로젝트 설정"** 클릭
2. 아래로 스크롤 → **"앱 추가"** 클릭 → 웹 아이콘 **`</>`** 클릭
3. 앱 닉네임 입력 (예: `budget`) → **"앱 등록"**
4. **`<script> 태그 사용`** 탭 클릭
5. 아래와 같이 생긴 코드 블록을 **메모장에 복사** 해두기:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "my-budget-xxxxx.firebaseapp.com",
  databaseURL: "https://my-budget-xxxxx-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "my-budget-xxxxx",
  storageBucket: "my-budget-xxxxx.firebasestorage.app",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

6. **"콘솔로 이동"** 클릭

---

### STEP 7 — index.html 수정 (설정값 교체)

1. 내 GitHub 저장소(`https://github.com/내아이디/Budget`)로 이동
2. **`index.html`** 파일 클릭
3. 오른쪽 상단 연필(✏️) 아이콘 클릭 (Edit this file)
4. `Ctrl+F` (맥: `Cmd+F`) 로 `apiKey` 검색
5. 아래처럼 생긴 7줄을 **STEP 6에서 복사한 내 값으로 교체**:

```javascript
firebase.initializeApp({
  apiKey: "여기를 내 값으로 교체",
  authDomain: "여기를 내 값으로 교체",
  databaseURL: "여기를 내 값으로 교체",
  projectId: "여기를 내 값으로 교체",
  storageBucket: "여기를 내 값으로 교체",
  messagingSenderId: "여기를 내 값으로 교체",
  appId: "여기를 내 값으로 교체"
});
```

6. 오른쪽 상단 녹색 **"Commit changes"** 버튼 클릭
7. 팝업에서 다시 **"Commit changes"** 클릭

---

### STEP 8 — GitHub Pages 배포

1. 내 저장소에서 상단 **"Settings"** 탭 클릭
2. 왼쪽 메뉴 **"Pages"** 클릭
3. **Source**: `Deploy from a branch` 선택
4. **Branch**: `main` 선택, 폴더는 `/ (root)` → **"Save"**
5. 1~3분 기다리면 상단에 주소가 나타납니다  
   👉 `https://내아이디.github.io/Budget/`

---

### STEP 9 — Firebase에 내 앱 주소 등록

> 이 설정을 안 하면 로그인이 안 됩니다.

1. Firebase 콘솔 → ⚙️ **"프로젝트 설정"**
2. **"Authentication"** 탭 클릭
3. 스크롤 내려서 **"승인된 도메인"** 섹션 찾기
4. **"도메인 추가"** 클릭
5. `내아이디.github.io` 입력 → **"추가"**

---

### 완료! 🎉

`https://내아이디.github.io/Budget/` 접속 → Google 로그인 → 사용 시작!

---

## 자주 묻는 질문

**Q. 요금이 드나요?**  
개인 사용 수준은 Firebase 무료 플랜(Spark)으로 충분합니다. 저장공간 1GB, 월 다운로드 10GB까지 무료입니다.

**Q. 앱이 업데이트되면 어떻게 하나요?**  
GitHub 저장소에서 **"Sync fork"** 버튼을 클릭하면 최신 코드로 자동 반영됩니다. (단, STEP 7에서 수정한 firebaseConfig는 다시 입력해야 할 수 있습니다)

**Q. 데이터가 사라졌어요.**  
같은 Google 계정으로 로그인하면 어느 기기에서든 데이터가 복원됩니다.

**Q. 다른 사람이 내 데이터를 볼 수 있나요?**  
STEP 5 보안 규칙을 설정하면 본인 Google 계정으로만 접근 가능합니다.
