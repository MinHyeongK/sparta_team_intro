# Team_Intro - 팀원 정보 등록 웹 애플리케이션

[👉 배포 링크 바로가기](https://minhyeongk.github.io/sparta_team_intro/)

---

## 📌 프로젝트 개요

내일배움캠프 Kotlin & Spring 백엔드 개발자 양성과정 8기 팀원들을 위한 정보 공유 웹페이지입니다.  
각 팀원이 직접 본인의 정보를 등록하고, **인증 기반으로 수정 및 삭제**할 수 있도록 구현되었습니다.

---

## ✅ 주요 기능

- 🔄 **Firebase 실시간 연동**: 팀원 카드 리스트 자동 업데이트
- 👁️ **상세 보기 모달**: 카드 클릭 시 전체 정보 팝업
- 🔐 **인증번호 기반 등록/수정/삭제 기능**
    - Firestore의 `settings/editkey` 값을 통해 보안 강화
- 🖼️ **이미지 업로드** (Firebase Storage 활용) 및 **기본 이미지 fallback(작업 실패 시 대체 동작)**
- 🧭 **GitHub 주소 자동 처리**
    - URL 입력 또는 ID 입력 모두 지원
- 📱 **반응형 UI 및 부트스트랩 기반 디자인**

---

## 🛠 기술 스택

| 항목          | 사용 기술 |
|---------------|-----------|
| Frontend      | HTML5, CSS3, JavaScript (jQuery), Bootstrap |
| Backend (DB)  | Firebase Firestore |
| File Storage  | Firebase Storage |
| 배포          | GitHub Pages |

---

## 🗂️ 폴더 구조 예시

```
sparta_team_intro/
├── README.md              ← ✅ 최종 버전 (V08)
└── index.html
```

---

## 🔐 인증번호 설정 방식 (v05 기준)

하드코딩된 인증번호가 아니라,  
**Firestore의 설정 문서 `settings/editkey`**의 `value` 값을 기반으로 인증합니다.

```js
// 인증번호 입력 → Firestore 값과 비교
async function checkPassword(inputPw) {
    const docRef = doc(db, "settings", "editkey");
    const docSnap = await getDoc(docRef);
    return docSnap.exists() && inputPw === docSnap.data().value;
}
```

> ✅ 하드코딩했을 때와 달리 개발자 도구를 통해 인증번호를 확인할 수 없으므로 보안성이 한층 강화된 방식.
> ❗하지만 현재 프로젝트는 익명 로그인으로 설계되어 있어 사실상 누구나 인증번호를 읽을 수 있으므로 콘솔을 통해 코드를 실행하여 인증번호 취득이 가능하다는 보안위험이 여전히 존재**
> ❗ 서버(백엔드)를 통해 APIKey, 암호를 분리하여 보안 강화 가능

---

## 🔁 버전 이력

| 버전 | 주요 변경 내용 |
|------|----------------|
| `V01` | 정적 HTML 및 초기 구조 |
| `V02` | 팀원 카드 확인 형태와 모달 구현 |
| `V03` | 이미지 업로드 구현 방식 변경 |
| `V04` | 수정/삭제 기능 |
| `V05` | 인증번호 처리 방식 개선 |
| `V06` | 익명 로그인 기능 추가 |
| `V07` | 리펙터링 |
| `V08` | V06과 V07 병합 |

---

## 📁 Firebase 보안 규칙

**개발용으로만 허용된 설정입니다. 실제 서비스 시 보안 정책 강화 필요!**

**Firestore**
```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /Team_8/{docId} {
      allow read : if true;
      allow write: if request.auth != null;
    }
    
    //  settings는 읽기만 허용
    	match /settings/{docId}{
    		allow read: if request.auth != null; // 익명 로그인 사용자 허용
    		allow write: if false;
    } 
  }
}
```

**Storage**
```js
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read : if true;
      allow write: if request.auth != null;
    }
    
      //  settings는 읽기만 허용
    	match /settings/{docId}{
    		allow read: if request.auth != null; // 익명 로그인 사용자 허용
    		allow write: if false;
    }
  }
}
```

---

## 📝 라이선스

- 본 프로젝트는 **내일배움캠프 학습 목적**으로 제작되었습니다.
- 누구나 자유롭게 포크 및 개선 가능하나, **상업적 사용은 불가**합니다.

---

## 🙋‍♀️ Contact

- 개발/디자인: **Team 8**
- 문의: [GitHub Issues](https://github.com/MinHyeongK/sparta_team_intro)
