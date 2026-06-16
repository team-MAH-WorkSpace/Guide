# FCM 연동 가이드 (웹 개발자용)

iOS 네이티브 앱(WebView 기반)에서 웹페이지로 FCM 토큰과 푸시 데이터를 전달하는 방식 안내 문서입니다.

---

## 1. FCM 토큰 사용 방법

### 전달 방식
페이지 로드가 완료되고 FCM 토큰이 발급되면, 앱이 웹뷰에서 전역 JavaScript 함수 `fcmToken`을 호출해 토큰을 전달합니다.

### 웹에서 구현해야 할 함수

전역 스코프에 아래 함수를 정의해 두면 앱이 토큰 수신 시점에 호출합니다.

```javascript
function fcmToken(token) {
    // token은 FCM 등록 토큰 문자열입니다.
    // 서버로 전송해 사용자 정보와 함께 저장/갱신하세요.
}
```

### 파라미터 형식
- 인자(`token`)는 **FCM 등록 토큰 문자열**입니다.
- 받은 토큰은 웹에서 직접 서버로 전송해 사용자별로 저장/갱신하면 됩니다. (앱은 서버로 직접 보내지 않습니다.)

### 주의 사항
- `fcmToken` 함수는 **반드시 전역(window)에 등록**되어 있어야 합니다. 모듈 스코프 안에 있으면 앱에서 호출할 수 없습니다.
- 앱은 **페이지 로드 완료 + 토큰 수신이 모두 충족된 뒤** 호출하므로, 함수가 가능한 한 빠른 시점에 등록되도록 해주세요.
- FCM 토큰 발급에 실패한 경우(권한 거부 등) `fcmToken`이 **호출되지 않을 수 있습니다**. 토큰이 없는 케이스도 정상 처리해야 합니다.
- FCM 토큰은 디바이스/앱 재설치/일정 주기 등에 따라 **갱신**될 수 있으므로, 갱신된 토큰이 들어오면 기존 토큰을 덮어쓰는 방식으로 관리해 주세요.

### 토큰 만료 / 갱신 정책

> **요약**: 일상적인 사용에서는 토큰이 거의 안 바뀝니다. 다만 특정 이벤트에서 반드시 한 번씩 바뀌므로, "**새 토큰이 오면 덮어쓰기**" 로직은 반드시 구현되어야 합니다.

#### 만료 조건 (FCM 공식)
- **Android**: 270일 이상 비활성(앱 미실행/메시지 미수신) 시 FCM이 토큰을 만료 처리하고, 이후 발송 요청은 거부됩니다.
- **iOS**: FCM이 자체적으로 만료 시간을 강제하지 않으며, APNs 정책을 따릅니다. 명시적 만료 기간은 공개되어 있지 않습니다.

#### 토큰이 바뀌는 트리거 (양 플랫폼 공통)
- 앱 **재설치**, 앱 **데이터 삭제**
- 새 기기로 **백업 복원**(기기 이전)
- FCM 서버의 내부 토큰 회전
- (iOS) APNs 토큰 변경

#### 빈도와 운영 함의
- 평소엔 거의 안 바뀌지만, 위 이벤트는 **사용자별로 비동기적으로** 발생합니다. (기기 교체 평균 2~3년 1회, 휴면 사용자 만료 등)
- 갱신 처리를 빠뜨리면, 평소엔 티가 안 나지만 **특정 사용자에게 푸시가 영구히 안 가는** 누적 손실이 발생합니다.

#### 서버 측 권장사항 (FCM 공식 best practice)
- 토큰을 저장할 때 **갱신 timestamp**도 함께 저장
- **약 2개월(stale window)** 이상 갱신되지 않은 토큰은 stale로 간주해 정리
- 신규/갱신 토큰이 들어오면 **즉시 덮어쓰기**

---

## 2. 푸시 데이터 전달 → 받는 방법

### 전달 방식
사용자가 푸시 알림을 **탭(클릭)했을 때**, 앱이 웹뷰에서 전역 JavaScript 함수 `receivePush`를 호출합니다.

> 포그라운드 상태에서 푸시가 도착했을 때는 배너만 노출되며, 데이터는 전달되지 않습니다. **사용자가 푸시를 탭한 시점에만** 데이터가 전달됩니다.

### 웹에서 구현해야 할 함수

전역 스코프에 아래와 같은 형태의 함수를 정의해 두면 앱이 푸시 탭 시점에 호출합니다.

```javascript
function receivePush(pushData) {
    // pushData는 푸시 페이로드(JSON)입니다.
    // 전달 형태는 console.log로 한 번 확인한 뒤 그에 맞게 처리하세요.
    // ... 라우팅, 알림 노출 등 처리
}
```

### 파라미터 형식
- 인자(`pushData`)는 앱이 전달하는 **푸시 페이로드(JSON)** 입니다.
- 전달되는 형태(객체/문자열 등)는 플랫폼·구현에 따라 다를 수 있으니, 받은 값을 `console.log` / `typeof`로 **직접 확인한 뒤 처리**하세요. (이미 객체면 키에 바로 접근, 문자열이면 `JSON.parse` 후 사용)
- 키 이름에 점(`.`)이 포함된 경우(예: `gcm.message_id`, `google.c.fid`)는 중첩 객체가 아니라 **키 이름 자체**이므로 대괄호 표기로 접근하세요: `pushData["gcm.message_id"]`.
- 앱은 **APNs/FCM에서 받은 페이로드를 가공 없이 그대로 전달**합니다. 어떤 키를 어떻게 활용할지는 웹 측에서 자유롭게 결정하면 됩니다.
- 페이로드 스키마(키 이름, 구조 등)는 **푸시 발송 측(서버)과 웹이 직접 합의**해 주세요. 앱은 관여하지 않습니다.

> **왜 그대로 전달하나요?**
> 앱에서 페이로드를 파싱·가공해 전달하면, 푸시 종류나 키가 추가/변경될 때마다 앱을 업데이트하고 스토어 심사를 다시 받아야 합니다.
> 받은 전문을 그대로 넘기는 방식이면 **앱 업데이트 없이도 서버와 웹만으로 푸시 데이터 구조를 자유롭게 추가/변경**할 수 있습니다.

### 호출 시점
- 앱이 **백그라운드/종료 상태**에서 푸시를 탭해 앱이 열린 경우 → 웹뷰 로드가 완료된 뒤 `receivePush` 호출
- 앱이 이미 **실행 중**인 상태에서 푸시를 탭한 경우 → 즉시 `receivePush` 호출

### 주의 사항
- `receivePush` 함수는 **반드시 전역(window)에 등록**되어 있어야 합니다. 모듈 스코프 안에 있으면 앱에서 호출할 수 없습니다.
- 웹 로드보다 푸시 탭 이벤트가 먼저 발생할 수 있으므로, **함수가 가능한 한 빠른 시점에 등록**되도록 해주세요.
- 동일 푸시에 대해 함수가 중복 호출되지는 않지만, 사용자가 동일 푸시를 여러 번 탭하면 그만큼 호출됩니다.

### 페이로드 예시 (FCM 기준)

> 아래는 참고용 예시일 뿐, 실제 키 구조는 서버↔웹에서 자유롭게 정의하시면 됩니다.

#### FCM 발송 페이로드 (서버 → FCM)
FCM HTTP v1 API 기준 예시입니다. `data` 필드에 웹에서 사용할 커스텀 키를 담고, OS별 알림 표시 옵션은 `apns` / `android` 블록에서 따로 설정합니다.

```json
{
  "message": {
    "token": "<디바이스 FCM 토큰>",
    "notification": {
      "title": "공지사항",
      "body": "새로운 공지가 등록되었습니다."
    },
    "data": {
      "type": "notice",
      "targetId": "12345",
      "url": "/notice/12345"
    },
    "apns": {
      "payload": {
        "aps": {
          "alert": {
            "title": "공지사항",
            "body": "새로운 공지가 등록되었습니다."
          },
          "sound": "default",
          "badge": 1
        }
      }
    },
    "android": {
      "priority": "high",
      "notification": {
        "title": "공지사항",
        "body": "새로운 공지가 등록되었습니다.",
        "sound": "default"
      }
    }
  }
}
```

#### iOS에서 웹이 받는 형태 (`receivePush`)
APNs `userInfo`가 그대로(JSON) 전달됩니다. 발송 시 `data`에 담은 키들은 **최상위에 평탄화(flatten)** 되어 들어옵니다. 아래와 같은 구조입니다.

```json
{
  "aps": {
    "alert": {
      "title": "공지사항",
      "body": "새로운 공지가 등록되었습니다."
    },
    "sound": "default",
    "badge": 1
  },
  "type": "notice",
  "targetId": "12345",
  "url": "/notice/12345",
  "gcm.message_id": "1234567890",
  "google.c.fid": "..."
}
```

#### Android에서 웹이 받는 형태 (`receivePush`)
AOS 앱의 구현 정책에 따라 다를 수 있으나, 일반적으로 FCM `RemoteMessage`의 `data` 필드를 그대로 전달합니다. iOS와 달리 `aps` 등 OS별 키는 포함되지 않습니다.

```json
{
  "type": "notice",
  "targetId": "12345",
  "url": "/notice/12345"
}
```

> **중요 — `notification`만 단독 발송하지 마세요.**
> Android에서는 `notification`만 보낸 메시지를 백그라운드 상태에서 탭하면 앱의 `onMessageReceived`가 호출되지 않아, 웹뷰로 데이터가 전달되지 않습니다. 사용자가 알림을 탭했을 때 웹에서 활용해야 할 정보는 **반드시 `data` 필드에 담아** 발송해 주세요.

### `data` 필드 작성 규칙 (FCM 공식 제약)

FCM HTTP v1 API 기준 제약사항입니다.

- **값은 모두 문자열(string)이어야 합니다.** 숫자/불리언/객체/배열은 직접 문자열로 변환해서 넣어야 합니다.
  ```json
  // ❌ 불가
  "data": { "targetId": 12345, "isNew": true, "tags": ["a","b"] }

  // ✅ 가능
  "data": { "targetId": "12345", "isNew": "true", "tags": "[\"a\",\"b\"]" }
  ```
- **예약어는 키로 사용할 수 없습니다.** 다음 키는 금지됩니다.
  - `from`, `message_type`
  - `google` 또는 `gcm`으로 시작하는 모든 키
    (점(`.`) 유무와 무관. 예: `google.c.fid`, `gcm.message_id` 등 FCM 내부 키)
- 전체 메시지 페이로드는 **최대 4096 byte**까지 가능합니다.

---

## 3. 요약 체크리스트

- [ ] `window.fcmToken(token)` 전역 함수 등록
- [ ] 받은 토큰을 서버로 전송해 저장 (FCM 토큰 갱신 시 덮어쓰기)
- [ ] 토큰 저장 시 갱신 timestamp 함께 관리 (stale 토큰 정리용, 권장 2개월)
- [ ] `window.receivePush(pushData)` 전역 함수 등록
- [ ] 받은 값 형태 확인 후 처리 (`console.log` / `typeof`)

---

## 참고 문서

- [FCM 메시지 타입 개요 (notification / data / 페이로드 크기 제한)](https://firebase.google.com/docs/cloud-messaging/concept-options)
- [메시지 타입 설정 가이드](https://firebase.google.com/docs/cloud-messaging/customize-messages/set-message-type)
- [FCM 등록 토큰 관리 모범 사례 (만료/갱신 정책)](https://firebase.google.com/docs/cloud-messaging/manage-tokens)
- [FCM HTTP v1 API — `projects.messages` 레퍼런스](https://firebase.google.com/docs/reference/fcm/rest/v1/projects.messages)
- [Android에서 메시지 수신 (notification vs data, 백그라운드 동작)](https://firebase.google.com/docs/cloud-messaging/android/receive)
- [Apple 플랫폼에서 메시지 수신 (APNs `userInfo` 구조)](https://firebase.google.com/docs/cloud-messaging/ios/receive)
