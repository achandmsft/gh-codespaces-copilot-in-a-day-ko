# Session01 Code Sheet

첫 번째 세션에서 copilot으로 생성해야 할 코드 가이드라인/예시를 소개합니다. 라이브로 따라하지 못해도 해당 sheet 페이지를 통해 본인의 copilot 결과와 비교하며 앱을 테스트하고 배포할 수 있습니다.

## 인프라 프로비저닝 - `infra` 폴더

### 1. `appservicePlan.bicep`

* `hostingPlan` 정의

![hostingPlan 정의](../images/appserviceplan01.png)

> hostingPlan의 이름은 수동으로 `'asplan-${name}'` 으로 변경해줍니다.

* `asplan` 리소스 정의

![asplan 리소스 정의](../images/appserviceplan02.png)

> `serverfarms` 버전을 확인합니다.
> 
> sku의 `name`, `tier`를 확인합니다.

* `output` 정의
  
![output 정의](../images/appserviceplan03.png)


### 2. `provision-appService.bicep`

* `appServicePlan` 모듈 정의

![appServicePlan 모듈 정의](../images/provision-asp01.png)

### 3. `openAI.bicep`

* `openai` 변수 정의

![openai 변수 정의](../images/openai01.png)

* `aoai` 리소스 정의

![aoai 리소스 정의](../images/openai02.png)

> `accounts` 버전을 확인합니다.

* `openaiDeployment` 리소스 정의

![openaiDeployment 리소스 정의](../images/openai03.png)

> `properties` 사이에 주석을 추가해서 `model` 과 `scaleSettings` 파라미터를 추가합니다.

### 4. `provision-cognitiveServices.bicep`

* `aoai` 모듈 정의

![aoai 모듈 정의](../images/provision-cog01.png)

* `output` 정의

![output 정의](../images/provision-cog02.png)

## 프론트 엔드 - `web` 폴더

### 1. 헤더 추가

return 상단에 원하는 헤더 내용을 담아 `msger head title` 을 추가합니다.

![헤더 추가](../images/web01.png)

### 2. `const[messages, setMessages] = useState([])` 추가

`messages` 를 정의하고, appendMessage 함수를 통해 azure bot의 인사말을 추가합니다.

![useState 추가](../images/web02.png)

> 인사말 결과

![인사말 결과](../images/greetings.png)

### 3. `form` 데이터에서 message 가져오기

![form 데이터에서 message 가져오기](../images/web03.png)

### 4. `appendMessage`로 질문과 로딩 답변 추가하기

* Enter와 함께 input을 비워줍니다.
* `setMessage` 함수를 통해 입력 받은 질문과 로딩 답변을 추가합니다.

![appendMessage로 질문과 로딩 답변 추가하기](../images/web04.png)

### 5. `map`으로 `messages` 리턴하기

return 상단에 `messages.map` 함수의 형태/결과 등을 정의합니다.

![map으로 messages 리턴하기](../images/web05.png)

![map으로 messages 리턴하기](../images/web06.png)

## 백엔드 - `api` 폴더

### 1. `OpenAPI` 구성

* `OpenAPI` object 생성
* `Contact` object 생성
* `License` object 생성
* `Info` object 생성

![OpenAPI 구성](../images/api-openapi.png)

### 2. POST `/api/messages` 구성

* `request` json에서 text 가져오기
* `String preMsg` 주석 해제
* `HTTPHeaders` 정의
* `api-key` 정의
  
![POST /api/messages 구성](../images/api01.png)

* `body` `headers` 로 `HTTPEntity` 정의
* `RestTemplate` 정의

![POST /api/messages 구성](../images/api02.png)

* `try` `catch` 문으로 Azure OpenAI API 호출
* `response` json에서 `content` 가져오기
* Error message 정의

![POST /api/messages 구성](../images/api03.png)

* `return` 문으로 `response` 리턴

![POST /api/messages 구성](../images/api04.png)

## 배포 시 주의 사항
1. `application.properties` 12번째 줄 주석 처리
```
    #CORS_ORIGIN=https://${CODESPACE_NAME}-3000.${GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN}
```
> CODESPACE_NAME과 같은 환경 변수가 GH Action 인스턴스에는 없기 때문에 빌드 시 에러가 발생하므로 반드시 주석처리

2. `application.properties` 13번째 줄 주석 해제
```
    CORS_ORIGIN=http://localhost:3000
```