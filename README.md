# FleaFully, 중고마켓 서비스 개선으로 보다 풍요로운 삶을 제공하다

![img](https://user-images.githubusercontent.com/72847093/101735679-91af6b80-3b05-11eb-972b-97d421deff0e.PNG)

## 1. 소개 
### 1-1. 기획 의도, 그리고 우리의 목표 
다양한 중고마켓 사이트의 데이터를 유저들이 하나의 플랫폼에서 좀 더 편하게 사용하는  목표로 함

### 1-2. 이름은 왜 FleaFully 인가요?
중고 마켓이라는 Fleamarket과 온전함, 풍부함의 뜻을 가지고 있는 Full을 합쳐서 만든 이름입니다. 

### 1-3. Built with 
- 김성준 : Selenium을 통한 중고나라 크롤링, MongoDB로 데이터베이스 관리, 슬랙 챗봇 구현, 카카오 api와 슬랙 webhook을 통한 알림톡 발송, 리드미 작성
- 전예나 : Flask를 통한 AWS 웹 서버 배포, css-js 템플릿을 통한 프론트엔드 구현, 번개장터 크롤링, DB - 웹 연동, 메일 발송 기능 구현, 리드미 작성 
- 정하윤 : Scrapy Framework를 통해 당근마켓 크롤링, Kakao local api 통해 거래주소에서 lat, lon값 추가, Rscript의 jitter함수를 통해 중복되는 좌표 변경, Node.js를 통해 mongodb에서 collection 데이터 import후 kakao map api통해 지도 구현

### 1-4. FleaFully를 통해 무엇을 할 수 있나요?

#### 1. 인기 카테고리 품목들의 가격 비교

<img src="https://user-images.githubusercontent.com/71831714/104890732-51b88f80-59b3-11eb-99dc-b06c39dcf4fa.png" height="300"></img>
<img src="https://user-images.githubusercontent.com/71831714/104890525-0aca9a00-59b3-11eb-8a4e-8e6c0214a972.png" height="300"></img>
<img src="https://user-images.githubusercontent.com/71831714/105712395-11ca4d00-5f5d-11eb-942d-7b3c4586f117.png"></img>
------  
#### 2. 구매자들은 현재 구매하려는 물품의 시세와 근처 동네의 매물 개수 파악
  
<img src="https://user-images.githubusercontent.com/71831714/104889993-4ca71080-59b2-11eb-99d1-5672c1132a53.png" width="300"></img>
<img src="https://user-images.githubusercontent.com/71831714/104890215-998ae700-59b2-11eb-9d56-f50372b9d161.png" width="300"></img>
------  
#### 3. 원하는 품목의 특정 가격 매물 정보를 메일로 발송

<img src="https://user-images.githubusercontent.com/71831714/104890339-c9d28580-59b2-11eb-95bb-22548afb4dbc.png"></img>
------  
#### 4. 챗봇을 통한 매물 추천과 필터링 기능 (슬랙 & 카카오톡)
  
<img src="https://user-images.githubusercontent.com/71831714/104883632-67748780-59a8-11eb-95b3-7504f07c4eb5.png" width="500"></img>
<img src="https://user-images.githubusercontent.com/71831714/104881288-62153e00-59a4-11eb-9d20-99d131959fea.png" width="400"></img>
------

## 2. 시스템 구조
![fleafully-draw_yena](https://user-images.githubusercontent.com/71831714/101885533-97788000-3bdd-11eb-94b8-5db03d840607.png)

## 3. 구성 요소 
### Details 
#### Getting Started
#### Prerequisites
- python3, R
- Mongodb, Mysql
- kakao & slack api

#### 코드 설명 - 성준
- run.py
  - jungonara.py를 12개 카테고리에 대해 실행  후 mongo.py 실행
- jungonara.py
  - 키워드를 입력하면 중고나라에서 크롤링 및 좌표 추출 및 mongodb에 저장
- dump.sh
  - 외부 서버로 db 백업
- mongo.py 
  - 데이터 전처리 후 챗봇용 collection 생성, slack_msg.py & kakao_msg.py 실행, 2시간 단위 db 삭제
- kakao_msg.py
  - 카카오톡으로 알림톡을 발송
- kakao_token.py
  - 카카오 api token을 발급 & 갱신
- slack_msg.py
  - 슬랙으로 알림톡 발송
- chatbot.py
  - 슬랙 챗봇 실행
- fleafully.py
  - 챗봇 기능 구현
  
#### 코드 설명 - 예나 
- fleafully.py
  - flask 프레임워크를 활용한 웹 서버 실행 모듈 
  - AWS 서버를 통해 WAS 구현 
  - mongoDB 연결을 통해 특정 조건의 데이터를 불러옴 
  - mysql 연결을 통해 프론트에서 user 입력값을 DB에 저장 
- bunjang_crawl_all.py 
  - json을 활용한 번개장터 크롤링 모듈
  - 크롤링 후 mongo DB 데이터 입력 
- template > mongo.html category.html kakaomap.html mail.html main.html
  - 프론트엔드 구현을 위한 html 
- mongo.html 
  - flask 모듈에서 연결된 mongo DB의 데이터를 jinja2를 활용한 프론트 표현 
  
#### 코드 설명 - 하윤
- scrapy.py
  - 당근마켓에서 원하는 키워드 명으로 데이터를 crawling
- pipeline.py
  - 당근마켓에 있는 ~구~동 주소으로 부터 kakao local api를 통해 lat,lon 추출
- jitter.R
  - 정확한 주소가 아니라 ~구~동이라던가 지하철명으로 되있어서 중복 되는 좌표값들이 맵에서 겹쳐보이는 걸 방지하기 위해 jitter함수로 중복값을 변경
- mongodb.py
  - 모은 자료들을 mongoDB에 저장
- get_data.js
  - mongoDB에서 node.js를 통해 json파일 형식의 데이터로 가져옴
- map.html
  - get_data.js를 통해 가져온 데이터를 kakao map api를 통해 맵으로 구현

## 4. 그리고 
#### 참고사이트 
- 중고나라 : https://www.joongna.com/
- 당근마켓 : https://www.daangn.com/
- 번개장터 : https://m.bunjang.co.kr/
- 헬로마켓 : https://www.hellomarket.com/
#### Q&A
- Contact us :  cuhz108@gmail.com
###### 본 프로젝트는 패스트캠퍼스 데이터사이언스 취업스쿨 15th 크롤링 프로젝트로 진행되었습니다.
