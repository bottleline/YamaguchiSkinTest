# Yamaguchi Skintest

## 주요기능

- 피부미용 기기와 블루투스 연결을 지원 
- 블루투스 제어 기능
- 스마트폰 카메라로 얼굴을 촬영하여 피부의 상태를 측정

## 수행역할
- 단독 프로젝트
- 앱 제작을 요청한 기업의 가이드라인에 따라 앱 설계 및 제작 배포
- 하드웨어 설계

## 사용한 기술
- `Swift 4`, `Xcode 11`
- `CoreBluetooth`
- `Alamofire`

## 적용한 주요 Layout
- `TableView`
- `TabbarController`
- `ScrollView`

## 주요 구현 장면

### 1. 블루투스 연결 (CoreBluetooth)
![bt_rs](https://user-images.githubusercontent.com/42457589/132491009-ab3fbeb9-16f6-42fe-97bd-aa3f0f201e50.gif)  
하드웨어 기기와 블루투스 연결을 한다.

### 2. 이미지 정사각형 크롭 및 서버전송 (Alamofire)
![crop](https://user-images.githubusercontent.com/42457589/132491006-89d419a6-0604-41d8-beb2-e88d7cc8fe6c.gif)  
이미지를 정사각형으로 크롭하여 서버로 전송한다.

### 3. 분석 결과 DB 저장
![graph](https://user-images.githubusercontent.com/42457589/132491010-6ce3fa94-e88a-481f-99c9-8a591b5d6528.gif)  
DB에 저장된 결과를 불러올수 있다.


