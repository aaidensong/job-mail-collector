[English](README.md) | [한국어](README.ko.md)

# Job Mail Collector

Job Mail Collector는 Gmail로 받은 채용 알림 메일을 읽고, 개인 커리어 프로필과 비교해 적합한 공고를 선별하고, 허용된 경우 Google Sheets 트래커에 기록하며, 지원 링크를 ChatGPT 안에서 반환하는 재사용 가능한 ChatGPT 예약 작업 워크플로입니다.

공개 저장소에는 워크플로 로직과 템플릿만 포함됩니다. 각 사용자의 커리어 프로필, 이메일 데이터, 지원 이력은 해당 사용자의 연결된 Google 계정 안에만 남습니다.

![Job Mail Collector 작업 흐름](job-mail-collector-flow-ko.png)

> 제품 동작, 연결 가능한 앱, 예약 작업 기능은 변경될 수 있습니다. OpenAI 문서 기준 마지막 확인일: 2026-09-08.

## 사용자가 실제로 하는 일

처음 사용하는 사람은 아래 네 가지만 하면 됩니다.

1. ChatGPT에 Gmail과 Google Drive를 연결합니다.
2. 이 저장소의 `prompts/01-bootstrap.md`를 복사해 새 ChatGPT 대화에 붙여넣습니다.
3. ChatGPT와 찾고 있는 일과 본인의 경력에 대해 이야기합니다.
4. ChatGPT가 추천한 링크를 열어 실제로 지원합니다.

필요한 권한이 있는 경우 나머지는 ChatGPT가 처리합니다.

## 온보딩은 설문 입력이 아니라 대화입니다

사용자가 내부 구조를 이해하거나 질문마다 정확한 값 하나만 입력할 필요는 없습니다.

예를 들어 아래처럼 답해도 됩니다.

> Senior Product Designer를 주로 찾고 있는데, 실제 업무가 제 경험과 맞으면 UX/UI나 Growth, Lead 역할도 같이 보고 싶어요. B2C 제품 디자인 경력은 약 9년이고 디자인 시스템과 Growth 쪽 경험도 많습니다.

ChatGPT는 이 답변에서 필요한 정보를 모두 추출해 내부적으로 정리하고, 이미 답변된 내용은 이후에 다시 묻지 않습니다.

온보딩은 다음 원칙을 따릅니다.

- 한 턴에 질문은 하나씩 합니다.
- 한 답변 안에 여러 정보가 들어 있어도 그대로 이해합니다.
- 답하기 어려울 수 있는 질문에는 자연스러운 답변 예시를 함께 보여줍니다.
- 예시는 참고용이며 같은 형식으로 답할 필요는 없습니다.
- 일반 질문에는 `필수` 표시를 하지 않습니다.
- 선택적인 질문에만 선택 질문임을 표시합니다.
- 선택 질문에는 답변하지 않아도 된다는 안내를 함께 표시합니다.
- 좁은 조건을 묻기 전에 실제 경력과 경험을 먼저 수집합니다.
- 기술적인 설정은 가능한 한 추천 기본값을 사용합니다.

한국어 대화에서 선택 질문에는 아래 문구를 사용합니다.

`선택 질문입니다. 필요하지 않다면 답변하지 않으셔도 됩니다.`

영문에서는:

`Optional question. You can skip this if it is not useful for your search.`

## 직무명과 경력 연수만으로 프로필을 만들지 않습니다

`Senior Product Designer`라는 직함 하나만으로는 좋은 매칭을 만들기 어렵습니다.

특히 경력이 많은 사용자에게는 역할에 맞는 몇 가지 추가 질문을 통해 실제 강점을 더 파악합니다.

예를 들면 다음과 같은 정보를 확인합니다.

- 실제로 어디까지 업무를 맡았는지
- 어떤 종류의 문제를 특히 잘 해결하는지
- 본인의 일로 무엇이 달라졌는지
- 모호한 문제나 의사결정을 어떻게 다뤘는지
- PM, 개발자, 다른 디자이너에게 어떤 영향을 미쳤는지
- 프로젝트 리드, 멘토링, 프로세스 개선 경험
- 다른 비슷한 경력의 후보자와 비교했을 때 두드러지는 경험

Senior Product Designer라면 예를 들어 이런 질문을 받을 수 있습니다.

- 어떤 제품 문제를 특히 잘 해결한다고 생각하시나요?
- 최근 역할에서 처음부터 끝까지 책임졌던 범위는 어디까지였나요?
- 본인의 작업으로 실제로 무엇이 달라졌나요?
- PM, 개발자, 다른 디자이너나 팀 프로세스에 어떤 영향을 미쳤나요?
- 비슷한 연차의 다른 디자이너와 비교했을 때 채용팀이 꼭 알아줬으면 하는 강점은 무엇인가요?

이 질문들도 한 번에 하나씩 진행되고, 이미 앞선 답변에서 충분히 확인된 내용은 다시 묻지 않습니다.

## 직무명보다 실제 적합도를 우선합니다

Job Mail Collector는 사용자가 지원 가능한 모든 직무명이나 직급을 미리 하나씩 정해야 한다고 가정하지 않습니다.

예를 들어 `Senior Product Designer`를 주로 찾는다고 해도 이것을 자동으로 허용 목록처럼 사용하지 않습니다.

`Staff Product Designer`, `Lead Product Designer`, UX/UI 역할 또는 다른 유사 직무라도 실제 업무 범위와 요구 경험이 사용자의 경력과 맞으면 추천 대상이 될 수 있습니다.

반대로 직무명은 같아도 실제 업무가 사용자의 강점과 크게 다르면 낮은 적합도로 판단할 수 있습니다.

공고를 판단할 때는 다음을 함께 봅니다.

- 실제 업무와 역할 범위
- 요구 경력과 오너십 수준
- 의사결정과 리더십 요구
- People Management 요구 여부
- 사용자의 특장점과 전문 영역
- 정량적 또는 구체적인 성과
- 제품과 산업 경험
- 지역과 근무 형태 조건
- 고용 형태, 취업 자격, 스폰서십, 급여 조건

Strong 또는 Possible match를 설명할 때도 단순히 `직무명이 같다`가 아니라 실제 경력 근거를 사용하도록 설계했습니다.

## People Management와 Leadership은 다르게 봅니다

Leadership에는 프로젝트 리드, 멘토링, 디자인 방향 설정, 협업 주도, 팀 프로세스 개선 등이 포함될 수 있습니다.

People Management는 direct report를 두고 1:1, 평가, 채용, 팀원 관리 등을 맡는 역할을 뜻합니다.

두 경험을 따로 기록하기 때문에, hands-on Lead 역할을 자동으로 사람 관리 역할로 판단하지 않습니다.

## Gmail과 Google Drive 연결 위치

ChatGPT에서 계정 화면에 따라 `Settings > Apps` 또는 `Settings > Plugins`로 이동합니다.

채용 알림 메일을 받는 Google 계정과 Job Mail Collector에서 사용할 Drive가 있는 계정을 연결합니다.

앱을 연결한 뒤에는 일반 ChatGPT 대화에서 Bootstrap 프롬프트를 실행합니다.

로컬 프로그램, 터미널 명령어, Python 스크립트, GitHub Action을 실행할 필요가 없습니다.

## 최초 1회 설정

1. ChatGPT에 Gmail과 Google Drive를 연결합니다.
2. GitHub에서 `prompts/01-bootstrap.md`를 엽니다.
3. 프롬프트 전체를 복사해 새 ChatGPT 대화에 붙여넣습니다.
4. ChatGPT가 질문을 하나씩 하고, 사용자는 편한 방식으로 답합니다.
5. ChatGPT가 실제 경력, 업무 범위, 성과와 특장점을 충분히 파악합니다.
6. ChatGPT가 현재 이해한 검색 방향을 요약하면, 필요한 부분만 수정하거나 좁힙니다.
7. ChatGPT가 연결된 Google Drive에 개인 커리어 프로필과 새 Google Sheet Tracker를 생성합니다.
8. 기존 지원 이력이 있다면 선택적으로 새 Tracker에 가져올 수 있습니다.
9. ChatGPT가 사용자가 선택한 시간에 반복 실행되는 예약 작업을 생성합니다.
10. 설정이 완료되기 전에 ChatGPT가 검증 테스트를 실행합니다.

이 프로젝트를 처음 사용하는 데 기존 Tracker는 필요하지 않습니다.

### 기존 지원 이력은 선택 사항입니다

Job Mail Collector는 최초 접근 확인 단계에서 사용자의 Drive를 뒤져 기존 Tracker를 임의로 선택하지 않습니다.

사용자가 과거 지원 이력을 가져오고 싶다고 명시한 경우에만 기존 Tracker를 찾을 수 있고, 호환 가능한 이력을 새 `Job_Mail_Collector` Tracker로 가져옵니다.

기존 Tracker가 없어도 아무 문제 없이 새 빈 Tracker로 설정을 계속합니다.

## 일일 자동화

예약된 시간이 되면 ChatGPT가 자동으로 다음 작업을 수행합니다.

1. 사용자의 비공개 커리어 프로필을 불러옵니다.
2. Gmail에서 설정된 채용 알림 메일을 읽습니다.
3. Digest 형식의 메일을 개별 공고로 분리합니다.
4. 가능한 경우 회사, 직무, 지역, 급여, 근무 형태, 출처, 지원 링크를 추출합니다.
5. 명백한 하드 제외 공고와 중복 공고를 제거합니다.
6. 남은 공고를 사용자의 실제 경력과 특장점에 비교합니다.
7. Google Drive 쓰기 권한이 허용되면 적합한 새 공고를 Tracker Sheet에 기록합니다.
8. 예약 작업 결과에서 가장 적합한 공고와 지원 링크를 반환합니다.
9. 명확한 지원 확인 메일과 회사 또는 리크루터 회신을 확인합니다.
10. 근거가 명확하면 Tracker 상태 필드를 자동으로 업데이트합니다.
11. 사람이 확인해야 하는 사항은 별도로 보고합니다.

기본 이메일 확인 범위는 선택한 시간대 기준 전날 00:00부터 23:59까지입니다.

### 자동화되지 않는 것

실제 지원서 제출은 일반적으로 사용자가 회사 채용 페이지, ATS, LinkedIn, Indeed 등 외부 지원 페이지에서 직접 진행합니다.

Job Mail Collector는 지원 확인 메일 같은 근거가 있거나 사용자가 직접 Tracker를 수정한 경우가 아니라면 지원 완료라고 판단하지 않습니다.

지원 확인 메일이 오지 않는 경우에는 사용자가 해당 행을 `Applied`로 직접 변경해야 할 수 있습니다.

## Tracker 자동 업데이트와 fallback

기본 동작은 Tracker 자동 기록입니다.

Google Drive 쓰기 작업이 가능하고 권한이 허용된 경우 예약 작업은 다음을 수행합니다.

- 새 적합 공고를 `Candidate` 상태로 추가합니다.
- 명확한 지원 확인 메일이 오면 해당 행을 `Applied`로 변경합니다.
- 회사 또는 리크루터의 명확한 회신이 오면 `RespondedAt`을 기록합니다.
- 근거가 충분한 경우 거절 결과와 거절 단계를 기록합니다.
- 애매한 경우 추측하지 않고 사람이 확인하도록 남깁니다.

일부 ChatGPT 계정이나 관리형 워크스페이스에서는 외부 데이터 변경 전에 승인이 필요할 수 있습니다.

예약 실행 중 쓰기 작업이 승인 없이 진행될 수 없다면 조용히 실패해서는 안 됩니다. 대신 변경 예정 내용을 바로 붙여넣을 수 있는 TSV 형식으로 반환하고 Tracker 쓰기 승인이 불가능했다고 명확히 알려야 합니다.

## 개인 데이터 구조

이 워크플로는 사용자가 소유하는 두 개의 비공개 리소스를 사용합니다.

### 1. 커리어 프로필

권장 파일명:

`Job_Mail_Collector_Profile.md`

주요 내용:

- 경력 요약
- 적합도 기반 검색 방향
- 사용자의 특장점과 실제 업무 범위
- 경력 하이라이트
- 정량적 또는 구체적 성과
- 핵심 스킬과 강점
- 전문 영역
- Leadership과 People Management 경험
- 지역, 근무 형태, 취업 자격 및 기타 선호 조건

raw Markdown 파일을 직접 생성하거나 읽는 기능이 지원되지 않는 경우, ChatGPT가 동일한 Markdown 내용을 담은 비공개 Google Doc을 생성하고 해당 문서 참조를 Sheet 설정에 저장합니다.

### 2. Tracker Sheet

기본 이름:

`Job_Mail_Collector`

탭:

- `Config`: 운영 설정과 자동화 모드
- `Sources`: 확인된 Gmail 채용 알림 발신자
- `Tracker`: 채용 후보와 지원 이력
- `Control`: 읽기 완전성 검증

새 Tracker는 최초 설정 중 자동으로 생성됩니다.

커리어 프로필이 매칭 판단의 기준입니다. Sheet에는 사용자의 전체 경력 내용을 중복 저장하지 않습니다.

## 핵심 기능

- 한 번에 질문 하나씩 진행하는 대화형 온보딩
- 자연어 자유 입력
- 답변 예시 제공
- 선택 질문에만 선택 표시
- 경력자의 특장점을 끌어내는 역할별 심화 질문
- 비공개 커리어 프로필 생성
- 직무명보다 실제 적합도를 우선하는 매칭
- 새 Google Sheet Tracker 자동 생성
- 기존 지원 이력 선택적 가져오기
- Gmail 채용 알림 소스 탐색 및 확인
- 반복 Scheduled Task 생성
- Digest 메일 분리
- 하드 필터링과 근거 기반 매칭
- 존재하지 않는 URL을 만들지 않는 지원 링크 추출
- 현재 실행과 기존 이력을 모두 고려한 중복 제거
- 권한이 있을 때 Tracker 자동 기록
- 지원 확인 메일 감지
- 회사 및 리크루터 회신 감지
- 무응답 경과 확인
- 근거가 있을 때 거절 단계 추정
- 확인 가능한 발신 도메인을 이용한 ATS 추정
- 불완전한 읽기, 권한 누락, 파싱 실패, 애매한 근거에 대한 진단

## 저장소 구조

```text
job-mail-collector/
├── LICENSE
├── README.md
├── README.ko.md
├── .gitignore
├── job-mail-collector-flow.png
├── job-mail-collector-flow-ko.png
├── prompts/
│   ├── 01-bootstrap.md
│   ├── 02-daily-job-mail-collector.md
│   ├── 03-test-run.md
│   └── 04-update-profile.md
├── profiles/
│   └── profile.template.md
├── docs/
│   ├── user-flow.md
│   ├── architecture.md
│   ├── profile-file.md
│   ├── profile-questionnaire.md
│   ├── sheet-schema.md
│   ├── matching-rules.md
│   └── troubleshooting.md
└── examples/
    ├── profile.example.md
    ├── sources.example.tsv
    └── output.example.md
```

## 설계 원칙

- **설문 입력이 아니라 대화.** 여러 정보를 한 번에 자연스럽게 말할 수 있습니다.
- **예시는 제공하되 형식을 강제하지 않음.** 무엇을 답해야 할지 알려주되 그대로 따라 쓸 필요는 없습니다.
- **선택 질문에만 표시.** 일반 질문을 필수 폼처럼 보이게 하지 않습니다.
- **좁은 조건보다 경력 깊이를 먼저 파악.** 직함과 연차만이 아니라 범위, 성과, 문제 해결 방식과 특장점을 수집합니다.
- **직무명보다 실제 적합도 우선.** 직함이 다르더라도 실제 역할이 맞으면 검토합니다.
- **People Management와 Leadership을 구분.**
- **새 Tracker를 자동 생성.**
- **개인 프로필은 공개 프롬프트 밖에 보관.**
- **지원 링크를 임의로 생성하지 않음.**
- **하드 필터를 매칭보다 먼저 적용.**
- **모든 판단에 근거 사용.**
- **부분 데이터로 부재를 판단하지 않음.**
- **자동 쓰기와 안전한 fallback.**
- **지원 상태를 추측하지 않음.**

## 버전 정보

현재 저장소 구조:

- `config_version = 3`
- `profile_version = 1`

이번 온보딩 및 매칭 개선은 YAML key set과 Sheet schema를 바꾸지 않으므로 버전은 그대로 유지합니다.

## OpenAI 참고 문서

- Scheduled Tasks: https://help.openai.com/en/articles/10291617
- 앱 계정 연결 및 관리: https://help.openai.com/en/articles/20001494-connecting-and-managing-app-accounts-in-chatgpt
- Google Drive 앱 설정: https://help.openai.com/en/articles/10929079
- Google 앱 데이터 제어: https://help.openai.com/en/articles/10408842-google-app-data-controls-faq

## 라이선스

별도 표기가 없는 한 이 저장소의 원본 프롬프트, 문서, 예시, 템플릿은 **Creative Commons Attribution 4.0 International (CC BY 4.0)** 라이선스를 따릅니다.

적절한 출처를 표시하고, 라이선스 링크를 제공하며, 변경 사항을 명시하는 조건으로 상업적 이용을 포함한 공유와 수정이 가능합니다.

권장 출처 표기:

> Job Mail Collector by Aiden, licensed under CC BY 4.0.

자세한 내용은 `LICENSE` 및 https://creativecommons.org/licenses/by/4.0/ 를 참고하세요.
