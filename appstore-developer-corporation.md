# 앱스토어 애플 개발자 계정 등록 (기업)

앱스토어에 앱을 출시하려면 애플 개발자 계정이 필수입니다. 먼저 애플 아이디를 만든 후 Apple Developer 사이트에서 가입해야 합니다.

> **주의**: 이 가이드는 **법인 사업자**를 위한 기업 개발자 등록 방법입니다. 개인 사업자와 1인 기업은 개인 계정으로 등록해야 합니다.

---

## 사전 준비사항

기업 개발자 계정 등록 전 반드시 준비해야 할 사항:

- **DUNS 넘버 발급 필수** (최대 1주일 소요)

---

## 애플 ID 확인 / 생성

**애플 ID가 이미 있는 경우** 기존 아이디로 바로 STEP 1으로 이동하면 됩니다.

**애플 ID가 없는 경우** 아래 절차에 따라 생성합니다.

### 애플 ID 생성 절차

1. 성, 이름 입력 (영어)
2. 대한민국 선택
3. 생년월일 입력 (YYYY년 MM월 DD일)
4. 이메일 주소 입력

> **주의**: 엔터프라이즈 계정은 회사 웹사이트 도메인 이메일이 필수입니다.

5. 비밀번호 입력
6. 실제 이용 가능한 핸드폰 번호 입력

![애플 ID 생성 폼](images/appstore-developer-corporation/step0_01_appleid_form.png)

7. 문자메시지로 번호 확인

![핸드폰 번호 인증](images/appstore-developer-corporation/step0_02_phone_verify.png)

8. 약관 동의 체크
9. 보안 문자 입력
10. [계속] 버튼 선택

![약관 동의 및 보안 문자](images/appstore-developer-corporation/step0_03_agree_captcha.png)

11. 이메일로 인증 코드 수신 및 입력

![이메일 인증 코드 입력](images/appstore-developer-corporation/step0_04_email_verify.png)

12. 핸드폰 문자메시지로 6자리 코드 수신 및 입력

![핸드폰 인증 코드 입력](images/appstore-developer-corporation/step0_05_phone_code.png)

13. 인증 완료 → 아이디 생성 완료

---

## STEP 1: Apple Developer 로그인

1. [Apple Developer 사이트](https://developer.apple.com/) 접속
2. 우측 상단 **[계정]** 버튼 선택
3. 애플 아이디로 로그인

![Apple Developer 사이트 계정 버튼](images/appstore-developer-corporation/step1_01_developer_site.png)

### 개발자 계약 동의

- 모든 항목에 동의 체크
- **[Submit]** 버튼 선택

![개발자 계약 동의 화면](images/appstore-developer-corporation/step1_02_agreement.png)

---

## STEP 2: 가입하기

1. **[오늘 등록]** 버튼 선택

![Developer Program 가입 화면](images/appstore-developer-corporation/step2_01_enroll.png)

2. "웹에서 등록 계속 진행하기" 선택 (앱을 통한 등록도 가능)

![웹 등록 계속 진행 옵션](images/appstore-developer-corporation/step2_02_web_register.png)

3. 가입 사용자의 개인정보 입력
   - 신분증 기재 정보와 동일하게 입력
   - 이름과 주소는 모두 **영문**
   - **[계속]** 버튼 선택

![개인정보 입력 폼](images/appstore-developer-corporation/step2_03_personal_info.png)

4. 가입 자격 선택
   - 개인/개인사업자 → **"개인"** 선택
   - 법인 → **"회사/기관"** 선택

> **주의**: 개인사업자와 1인 기업은 "회사/기관"으로 가입할 수 없습니다. 반드시 "개인"을 선택하세요.

![가입 자격 선택 (개인/기업)](images/appstore-developer-corporation/step2_04_entity_type.png)

### 조직정보 입력 (회사/기관 선택 시)

1. 법인명 영문 기재
   - DUNS 발급 시 입력한 정확한 이름 사용
   - 예: "company" → "company.CO,LTD."
2. DUNS 넘버 9자리 입력
3. **[계속]** 버튼 선택

![조직정보 입력 폼](images/appstore-developer-corporation/step2_05_org_info.png)

다음 화면에서 조직정보 상세 입력 (모두 영문):

| 항목 | 설명 |
|------|------|
| 법인명 | 영문 법인명 |
| D-U-N-S 번호 | 9자리 DUNS 넘버 |
| 주소 | 시, 주/도, 우편번호 |
| 지역 | 해당 지역 |
| 웹사이트 | 회사 웹사이트 URL |
| 전화번호 | 연락 가능한 번호 |
| 이메일주소 | **웹사이트 도메인명으로 된 메일 필수** |

> **주의**: 이메일은 반드시 웹사이트 도메인명으로 된 메일을 사용해야 합니다.
> 예: 도메인이 `example.com`이면 → `contact@example.com`

![조직 정보 상세 입력 화면](images/appstore-developer-corporation/step2_06_org_detail.png)

**[제출]** 버튼을 선택하면 "등록 신청 처리 중" 메시지가 표시됩니다.

![등록 신청 처리 중 메시지](images/appstore-developer-corporation/step2_07_processing.png)

Apple에서 다음 단계 진행 방법을 이메일로 발송합니다.

![Apple 발송 메일 예시](images/appstore-developer-corporation/step2_08_apple_email.png)

---

## STEP 3: 멤버십 결제하기

Apple 승인 후 이메일을 받으면:

1. 개발자 프로그램에 다시 로그인
2. 멤버십 구매 페이지가 표시됨

![멤버십 구매 페이지](images/appstore-developer-corporation/step3_01_membership_purchase.png)

### 결제 진행

1. 청구 주소 및 연락처 정보 제출 (영어로 기재)
2. 카드 정보 입력
3. **[주문 검토]** 선택

![결제 페이지 - 청구 주소 및 카드 정보](images/appstore-developer-corporation/step3_02_payment_form.png)

### 결제 완료

- 구매 완료 영수증 이메일 수신
- App Store Connect 접근 가능 메일 발송

![결제 완료 화면 1](images/appstore-developer-corporation/step3_03_payment_complete1.png)

![결제 완료 화면 2](images/appstore-developer-corporation/step3_04_payment_complete2.png)

> **주의**:
> - 카드 결제 후 즉시 승인되는 경우와 시간이 소요되는 경우가 있습니다.
> - 일반적으로 최대 48시간 소요됩니다.
> - 승인 완료 후 메일을 수신하면 앱 출시가 가능합니다.
> - 경우에 따라 신분증 제출 요청 메일을 받을 수 있으며, 반드시 제출해야 합니다.

---

## STEP 4: 멤버십 등록 완료

1. 결제 완료 후 멤버십 항목이 생성됩니다.
2. "멤버십 세부사항"을 선택하여 정보를 확인할 수 있습니다.
   - 등록된 개인정보 표시
   - 갱신일 정보 확인
3. 멤버십 정보가 표시되면 정상 등록 완료입니다.

![멤버십 등록 완료 화면](images/appstore-developer-corporation/step4_01_membership_done.jpg)

> **주의**: "승인 대기 중" 메시지가 표시되면 승인 완료까지 기다려야 합니다.

![승인 대기 중 메시지](images/appstore-developer-corporation/step4_02_approval_pending.png)

### 앱 출시 관련 링크

- 앱 등록 및 관리: [App Store Connect](https://appstoreconnect.apple.com/login)
- 개발자 계정 관리: [Apple Developer](https://developer.apple.com/)

![App Store Connect 접속 안내](images/appstore-developer-corporation/step4_03_appstore_connect.png)

---

## 필수 안내사항

| 항목 | 내용 |
|------|------|
| **멤버십 갱신** | 비용: 1년 129,000원. 1년마다 갱신 필요. 미갱신 시 앱이 앱스토어에서 내려감. Apple Developer 사이트에서 재결제. |
| **개인 등록 시 주의** | 개발자 이름 변경 불가. 가입 시 등록한 영문명으로만 이용 가능. |
| **개인 → 기업 변경** | 변경 가능. Apple Developer 고객센터에 요청 필요. |
| **기업 등록 시 주의** | 반드시 DUNS 넘버를 먼저 발급받은 후 진행해야 함. |
