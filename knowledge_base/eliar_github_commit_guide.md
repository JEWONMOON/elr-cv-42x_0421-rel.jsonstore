
# 엘리아르를 위한 GitHub API 커밋 절차 및 방법

## 🔹 1️⃣ 준비 단계
- GitHub 레포지토리에 접근할 수 있는 API 권한이 설정되어 있어야 합니다.
- 커밋하려는 **파일 경로**와 **업데이트할 내용**이 준비되어야 합니다.
- 파일이 이미 존재하는지 여부를 확인하기 위해 `GET` 요청으로 조회합니다.

---

## 🔹 2️⃣ 파일 조회 단계 (GET Request)
- 업데이트할 파일이 현재 레포지토리에 있는지 확인하기 위해 `getElrRootManifestContentByPath` API를 사용합니다.

### 예시:
```python
api_github_com__jit_plugin.getElrRootManifestContentByPath({
    "path": "manifests/conversation_log.txt"
})
```

- **응답 결과**로 `sha` 값을 얻어야 합니다.
    - `sha`: 파일의 특정 커밋 상태를 나타내며, 업데이트 시 반드시 포함해야 합니다.

---

## 🔹 3️⃣ 내용 준비 및 Base64 인코딩
- UTF-8로 내용을 작성한 후, Base64로 인코딩합니다.
### 예시:
```python
import base64

content = "업데이트할 텍스트 내용"
encoded_content = base64.b64encode(content.encode('utf-8')).decode('utf-8')
```

---

## 🔹 4️⃣ 업데이트 요청 (PUT Request)
- 업데이트할 파일의 경로와 내용을 전달하여 `PUT` 요청을 실행합니다.

### 예시:
```python
api_github_com__jit_plugin.putElrRootManifestContentByPath({
    "path": "manifests/conversation_log.txt",
    "message": "대화 내용 기록 업데이트 (UTF-8 인코딩 수정)",
    "content": encoded_content,
    "sha": "<파일 조회 시 얻은 sha 값>",
    "branch": "main"
})
```

---

## 🔹 5️⃣ 성공 여부 확인
- 응답에서 `html_url`이 반환되면, 커밋이 정상적으로 반영된 것입니다.
- 예시: [conversation_log.txt](https://github.com/JEWONMOON/elr-root-manifest/blob/main/manifests/conversation_log.txt)

---

## 🔹 6️⃣ 최종 확인 및 테스트
- 브라우저를 통해 GitHub 레포지토리의 해당 파일이 정상적으로 업데이트되었는지 확인합니다.
- 혹시 인코딩 오류가 있다면, Base64 인코딩 부분을 재검토합니다.

---

## 📌 예시 커밋 시나리오 요약
1. `GET` 요청으로 SHA 값을 조회한다.
2. 내용을 UTF-8로 작성하고 Base64로 인코딩한다.
3. `PUT` 요청으로 업데이트한다.
4. 커밋 URL을 통해 최종 반영 상태를 확인한다.
