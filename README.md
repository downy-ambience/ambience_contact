# Ambience Inc. — 사내 연락망

회사 Google 계정으로 로그인하는 사내 구성원 연락처 디렉토리입니다.

## 접근 허용 도메인
- `@ambienceseoul.com`
- `@thepairshow.com`

---

## 🚀 처음 배포 설정 (1회만)

### 1. Firebase 프로젝트 생성

1. [Firebase Console](https://console.firebase.google.com/) 접속
2. **프로젝트 추가** 클릭 → 프로젝트 이름 입력 (예: `ambience-contact`)
3. Google Analytics는 선택사항 (없어도 됨)

### 2. Google 로그인 설정

1. 좌측 메뉴 → **Authentication** → **시작하기**
2. **Sign-in method** 탭 → **Google** 클릭 → 사용 설정 ON
3. 프로젝트 공개용 이름 입력 → **저장**

### 3. Firestore 데이터베이스 생성

1. 좌측 메뉴 → **Firestore Database** → **데이터베이스 만들기**
2. **프로덕션 모드**로 시작 → 위치 선택 (`asia-northeast3` = 서울)
3. **규칙** 탭에서 아래로 교체 후 **게시**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 4. Firebase 앱 등록 및 설정값 복사

1. 프로젝트 설정(⚙️) → **앱 추가** → 웹(`</>`) 클릭
2. 앱 닉네임 입력 → **앱 등록**
3. 표시되는 `firebaseConfig` 값을 복사

### 5. index.html에 설정값 붙여넣기

`index.html` 파일에서 아래 부분을 찾아 실제 값으로 교체:

```js
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",         // ← 실제 값으로
  authDomain:        "YOUR_PROJECT_ID...",   // ← 실제 값으로
  projectId:         "YOUR_PROJECT_ID",      // ← 실제 값으로
  storageBucket:     "YOUR_PROJECT_ID...",   // ← 실제 값으로
  messagingSenderId: "YOUR_SENDER_ID",       // ← 실제 값으로
  appId:             "YOUR_APP_ID"           // ← 실제 값으로
};
```

### 6. GitHub 저장소 생성 및 배포

```bash
# 저장소 초기화
cd "path/to/사내\ 연락망"
git init
git add index.html README.md
git commit -m "Initial commit"

# GitHub에 올리기 (GitHub에서 새 저장소 생성 후)
git remote add origin https://github.com/{USERNAME}/{REPO}.git
git push -u origin main
```

GitHub 저장소 → **Settings** → **Pages** → Branch: `main` → **Save**

### 7. GitHub Pages 도메인을 Firebase에 등록

1. Firebase Console → Authentication → **Settings** → **승인된 도메인**
2. `{USERNAME}.github.io` 추가

---

## 사용법

- Google 로그인 → 회사 도메인 자동 확인
- 카드 클릭 → 연락처 수정
- 우측 상단 **구성원 추가** 버튼 → 새 구성원 등록
- 변경사항은 **모든 팀원에게 실시간 반영**
