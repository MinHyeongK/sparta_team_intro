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
- 🖼️ **이미지 업로드** (Storage 활용) 및 **기본 이미지 fallback**
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
Team_Intro/
├── default.png
├── index.html             ← ✅ 최종 버전 (v05)
└── README.md
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

> ✅ 콘솔에서 인증번호를 직접 노출하지 않으므로, **보안이 한층 강화된 구조입니다.**

---

## 🔁 버전 이력

| 버전 | 주요 변경 내용 |
|------|----------------|
| `v01` | 정적 HTML 및 초기 구조 |
| `v02` | Firebase 연동 및 카드 렌더링 |
| `v03` | 등록 폼, 이미지 업로드 구현 |
| `v04` | 수정/삭제 기능, 인증번호 로직 |
| `v05` | 🔐 인증 보안 개선, GitHub 링크 처리, 디자인 완성도 개선 등 |

---

## 📁 Firebase 보안 규칙 예시

**개발용으로만 허용된 설정입니다. 실제 서비스 시 반드시 인증 제어 필요!**

**Firestore 예시:**
```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /Team_8/{document=**} {
      allow read, write: if true;  // ⚠️ 개발용 (배포 시 제한 필요)
    }
    match /settings/editkey {
      allow read: if true;         // 인증번호 확인용
    }
  }
}
```

**Storage 예시:**
```js
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;  // ⚠️ 개발용 (배포 시 제한 필요)
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
- 문의: [GitHub Issues](https://github.com/xellos216/Team_Intro/issues)
