# 쿡슝(Cook-Shoong)
![](https://velog.velcdn.com/images/eora21/post/9b2cc6a2-84df-4ff6-9c1c-338c53c7f22e/image.png)

## 개요 
MSA 구조를 학습하고 배운 기술을 적용해보기 위한 온라인 음식 배달 플랫폼 제작 프로젝트 

## 팀원
백엔드 5명으로 구성된 프로젝트

## ERD
[https://www.erdcloud.com/d/5D89pNAP23LAuexGz](https://www.erdcloud.com/d/xBD2t2L46DdMeBfeg)

### DB 관리

ERD 작성은 ERDCloud를 사용하였으며, 생성 및 수정은 My Workbench를 이용하였습니다.   
해당 DB의 DDL파일은 이곳을 참조해 주세요. -> https://github.com/nhnacademy-be3-CookShoong/ddl_manage  
(Cook-Shoong ERD 1.0.2 version까지 업데이트되었습니다)


![스크린샷 2023-08-19 오후 8 36 20](https://github.com/nhnacademy-be3-CookShoong/.github/assets/66362713/1d51e50a-18aa-4920-b179-75a030591ff6)


## 아키텍처
1. 사용자 요청 처리 (Front-End)
- 모든 사용자 요청은 NGINX를 통해 안전하게 들어옵니다
- 트래픽 분산을 위해 Round Robin 방식으로 여러 서버에 균등하게 부하를 분배합니다

2. 서비스 연동 (Back-End)
- API Gateway가 모든 서비스 요청을 중앙에서 관리합니다
- Netflix Eureka를 통해 필요한 서비스의 위치를 자동으로 찾아줍니다
- 주요 서비스:
  * 인증(Auth) 서버: 사용자 인증/인가 담당
  * 상점(Shop) 서버: 상품 정보 및 주문 관리
  * 검색 서버: Elasticsearch로 빠른 검색 기능 제공
  * 배치 서버: 대량 데이터 처리

![CookShoong_아키텍처](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/e644a030-23cf-4e91-9319-7e45d905893a)

## CI/CD
1. `Github`을 통해 코드베이스를 관리하며 기본적으로 `Github Action`을 통한 CI를 진행하여 빌드하고 테스트가 진행되어 성공시 통합과정이 이뤄집니다.
   - 원격 레포지토리를 사용함에 따라 DB 정보 등 암호화가 필요한 정보들은 NHN Cloud의 Secure Key Manager를 사용하여 관리되고 환경변수를 통해 정보에 접근합니다.
   - `NHN Dooray` 서비스의 WebHook 설정을 통해 CI 관련 알림을 받고 확인할 수 있습니다.
   - Google Checks를 변형한 Checkstyle을 사용하여 팀원들끼리의 코드 일관성을 유지하며 재사용성을 높였습니다.
2. CI 과정이 일어나고 검증을 마친 코드들은 `Docker` 이미지로 생성되며 `Github Action` 또는 Jenkins를 통해 자동으로 `NHN Cloud Instance`에 `Docker Container`가 생성되어 배포가 이뤄집니다.
   - CD 과정에서 정적 분석 툴인 `SonarQube`을 사용하여 코드 품질을 측정하며 이상 있는 코드들을 감지합니다.

![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/0cf8b365-230e-4040-98d9-04b0323c8a50)

## Black Box - Client Test Flow
- 사이트에 처음 방문하는 회원이 사용할 수 있는 기능들에 대해 실험해 보는 블랙박스테스트 플로우입니다.

![Client_Test_Flow](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/3caa5da3-914e-47fe-86eb-05e4133acb71)

## 프로젝트 관리
체계적이고 효율적인 관리를 위해, [Github Project](https://github.com/orgs/nhnacademy-be3-CookShoong/projects/1)을 활용하여 프로젝트를 진행하였습니다.

### Kanban
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/d4023219-5576-4f5a-a13e-7f5a84284ce3)

Kan ban 보드를 활용하여 현재 프로젝트의 진행도를 수시로 확인할 수 있었습니다.

### Scrum
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/26c89680-9a95-42ce-b413-95af2dfbcd60)

스크럼은 평일, 하루에 한 번씩 진행하였습니다. 아침에 서로의 진행 상황을 이야기하고, 필요하다면 오후에도 스스럼없이 의견을 나누는 시간을 가졌습니다.

### Share
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/11ff915c-5988-4013-ab7b-d9fde6dfb515)

서로 공부한 상황에 대해 수시로 공유하였습니다. 만약 모여서 이야기를 할 수 없는 상황일 경우, STUDY 또는 REFERENCE 항목에 해당하는 글을 남겨 확인하였습니다.

### Collaboration
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/8c2e34d5-6d25-4199-9bf6-d2a6584bc35e)

결정하거나 진행해야 할 일이 있다면 만료일자를 설정하여 해당 기간 안에 마치도록 하였습니다. 설령 마치지 못했다면, delay로 넘겨 꾸준히 관리할 수 있도록 하였습니다.

### Manage
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/6750030e-a8b1-453e-9823-1f23c09e585b)

![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/26e59416-9a87-489d-a549-3532ce2a1d32)

항목마다 프로젝트를 생성하여 관리했으며, git-flow 전략을 활용해 진행중인 작업별로 브랜치를 나누었습니다.

각각 개발중인 내용은 feature에 작성하였으며, 해당 작업이 끝났을 경우 develop으로 병합하였습니다. 병합된 내용들은 main에 올려 실제 배포서버로 배포하였습니다.

### PR
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/8c1f5600-5cbe-4369-90d9-a3324e00bbb2)

PR 제목은 해당 작업을 잘 나타낼 수 있도록 하였으며, 2명 이상의 approve를 받았을 시 merge할 수 있도록 하였습니다.

### CodeReview
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/61442066/2db4b538-f6bf-452b-8a8f-c03c8fe16ac6)

PR에 대해 코드리뷰를 진행하였으며, 오프라인으로 진행했더라도 온라인에 기록을 남겨 서로가 확인할 수 있도록 하였습니다.

## 일정관리 - [WBS](https://docs.google.com/spreadsheets/d/1OSbKIHQUBWEuf-fbNGP96Cmv5vJj0ipUISgu_yhQV1Y/edit#gid=893001414)
회원과 배송, 매장과 리뷰, 메뉴, 쿠폰과 포인트, 주문과 결제로 크게 나누어 업무를 분담하였습니다. 각 파트에서 주요 기능별로 업무 분리 후 세부 업무 목록을 작성하였고, 주요 업무를 수행하면서 사용하게 되는 예상 사용 기술들을 같이 기재하였습니다. 이후 배달 어플리케이션에서 반드시 필요한 (회원, 매장, 메뉴, 쿠폰)을 우선적으로 작업하였고, (주문, 결제, 포인트, 리뷰, 배송) 순으로 업무 진행 순서를 정했습니다.

![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/76582376/55227907-d603-45f4-a488-40e01ea19fb2)
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/76582376/05f4a57b-fdc6-4814-ab09-175927cb65ad)

## [Github Projects](https://github.com/orgs/nhnacademy-be3-CookShoong/projects/1) 링크, Github Roadmap 관리
- WBS 일정을 바탕으로 각자 일정에 맞추어 세분화 작업을 진행하였습니다. 추가로 각자 진행하는 부분에 있어서 사전 지식 및 진행 사항 등을 일정 안에 작성함으로써 프로젝트 효율을 높였습니다.
- 추만석
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/0d99356e-f8bb-4a7f-9523-e2b68b835188)

## 칸반보드
- Git에서 제공하는 칸반보드를 활용해서 해야 할 일, 진행 중, 지연, 완료 단계로 프로젝트 효율을 높였습니다.
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/5907ea4e-721a-4db0-9a8d-9188a297c35b)

## REST API Specification
- SwaggerUI + Spring REST Docs 함께 사용하였습니다.
- [REST API](https://www.cookshoong.store/swagger-ui) (현재 서버 종료됨)
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/d2659fac-288c-45f6-ba75-018ca5cfe9b6)

## [QA](https://github.com/orgs/nhnacademy-be3-CookShoong/projects/1/views/18?pane=issue&itemId=35665501)
- CookShoong에 'Quality Assurance'을 진행하여 문제가 될 수 있는 부분을 종합하고 해결하였습니다.
![image](https://github.com/nhnacademy-be3-CookShoong/.github/assets/85005950/bdfcf19f-a7fa-4986-8787-d35ce4ae5e35)


# 주요기능

## 인증/인가
> Cookshoong 서비스 이용자임을 증명을 확인하고 서비스를 이용할 수 있게 합니다.

### 담당자
- 추만석

### 기능
- 인증서버에서 자격증명을 확인하고 JWT을 발급합니다.
- 액세스토큰과 리프레쉬 토큰은 JWT로 발급되며 사용자는 리프레쉬 토큰을 통해 토큰들을 자동으로 재발급합니다.
- 게이트웨이에서 토큰을 검증하여 인증된 사용자만이 백엔드 API 호출을 가능하게 합니다.
- 프론트서버에서는 발급받은 토큰을 이용해 백엔드 API 호출을 합니다.
- 블랙리스트를 두어 만료된 토큰 또는 탈취된 토큰의 접근을 막고 관리합니다.
- 리프레쉬 토큰은 암호화되어 관리됩니다.
- 게이트웨이에서 권한별로 적절한 API를 호출하는 지 확인합니다.
  
### 사용된 기술
- Spring Security, Spring Cloud Gateway, JWT


---


## 회원
> 사용자가 Cookshoong 서비스에 간편하게 가입할 수 있도록 돕습니다.

### 담당자
- 추만석

### 기능
- Cookshoong 서비스를 이용하기 위한 회원가입이 가능합니다.
- OAuth2에서 받은 권한을 통해 얻어낸 정보로 간편 회원가입이 가능합니다.
- 등록된 자신의 정보를 수정할 수 있습니다.
- 일반 회원, 사업자 회원, 관리자로 나눠서 관리하며 계층구조를 이룹니다.
- 일반 회원은 매장 또는 메뉴 조회, 주문 등의 권한을 가집니다.
- 사업자 회원은 일반 회원의 상위 권한이며 매장을 등록 및 관리 등을 할 수 있습니다.
- 관리자는 일반, 사업자 회원보다 높은 권한을 가지며 서비스 이용에 모든 권한을 가집니다.

### 사용된 기술
- Spring Security, Spring Cloud Gateway, JWT

---

## 인프라
### 사용된 기술
- Github Action, Jenkins, Nginx, Road Balancer, Docker, NHN Cloud Instance

### 설명
- CI/CD
   - Github Action : front, back, batch, gateway
   - Jenkins : auth, delivery
   - Github Action, Jenkins를 통해 CI를 진행하고 검증을 마친 코드들은 Docker 이미지로 생성되며 자동으로 NHN Cloud Instance에 Docker Container 가 생성되어 배포가 이뤄집니다.
- 무중단 배포
   - Front Server : 클라이언트의 요청을 Nginx 통해서 들어오고, 로드밸런서에서 Round Robin 방식으로 순서대로 Front Application에 보내지게 됩니다.
   - Back, Batch, Gateway, Delivery : Service Discovery 인 Eureka 에서 필요한 서비스가 어느 곳에 있는지에 대한 정보를 API Gateway로 반환하고 API Gateway는 이에 따라 해당 API 서비스를 호출하고 결과를 받게 됩니다.
---


## 기술
- Spring <br>
![Spring](https://img.shields.io/badge/Spring-6DB33F?style=flat&logo=spring&logoColor=white)
![Spring Boot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/SpringSecurity-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/SpringCloud-6DB33F?style=flat&logo=spring&logoColor=white)

- DB <br>
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![Redis](https://img.shields.io/badge/MySql-4479A1?style=flat&logo=mysql&logoColor=white)

- CI/CD <br>
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=Jenkins&logoColor=white)
![Github Actions](https://img.shields.io/badge/GithubActions-2088FF?style=flat&logo=githubactions&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=Docker&logoColor=white)
![iCloud](https://img.shields.io/badge/NHNCloud-3693F3?style=flat&logo=icloud&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat&logo=nginx&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat&logo=Ubuntu&logoColor=white)

- 그 외 기술 <br>
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=HTML5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=CSS3&logoColor=white)
![JAVA](https://img.shields.io/badge/JAVA-007396?style=flat&logo=OpenJDK&logoColor=white)
![JPA](https://img.shields.io/badge/JPA-59666C?style=flat&logo=Hibernate&logoColor=white)
![SonarLint](https://img.shields.io/badge/SonarLint-CB2029?style=flat&logo=SonarLint&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-4E9BCD?style=flat&logo=SonarQube&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white)
![ApacheMaven](https://img.shields.io/badge/ApacheMaven-C71A36?style=flat&logo=ApacheMaven&logoColor=white)
