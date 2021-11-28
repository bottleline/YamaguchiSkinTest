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
### 1
``` swift
 // 1. 디바이스 스캔시작 
 centralManager.scanForPeripherals(withServices: nil) 

``` 
### 2
``` swift
 // 1. 디바이스 발견시 연결 
func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : Any], rssi RSSI: NSNumber) {
        if peripheral.name == "myDevice"{
            central.connect(peripheral,options:nil) // 일련번호를 확인후 연결한다
        }
    }

``` 
``` swift
 // 2. notifying 서비스 연결
 func peripheral(_ peripheral: CBPeripheral, didDiscoverCharacteristicsFor service: CBService, error: Error?) {
     guard let charactistics = service.characteristics else{return}
     if charactistic.uuid == notiUUID{
                peripheral.setNotifyValue(true, for: charactistic) // notifiying 서비스를 구독하여 기기에서 변경된 값을 감시
            }
      }
 }
 
 //3. 원하는 패킷 전송
 peripheral.writeValue(value) // 패킷 전송

``` 
### 3
``` swift
// 3. 응답패킷 수신 
 func peripheral(_ peripheral: CBPeripheral, didUpdateValueFor characteristic: CBCharacteristic, error: Error?) {
        switch characteristic.uuid{
        
        case notiUUID:
            guard let s = characteristic.value else {return}
            guard let sData = s.data(using: .utf8) else {return}
            if let string = String(data: sData,encoding: String.Encoding.utf8){
                print("receive : \(string)")
            }
        default:
            print("Unhandled Charactistic UUID: \(characteristic.uuid)")
        }
    }
```
### 2. 이미지 정사각형 크롭 및 서버전송 (Alamofire)
![crop](https://user-images.githubusercontent.com/42457589/132491006-89d419a6-0604-41d8-beb2-e88d7cc8fe6c.gif)  
이미지를 정사각형으로 크롭하여 서버로 전송한다.
``` swift
func cropImage(_ inputImage : UIImage, toRect cropRect: CGRect) -> UIImage{
        let frame = CGRect(x: cropRect.minX * scale,
                           y: cropRect.minY * scale,
                           width: cropRect.width * scale,
                           height: cropRect.height * scale)
        guard let cutImageRef: CGImage = inputImage.cgImage?.cropping(to: frame)
            else{
                return inputImage
        }
        let croppedImage: UIImage = UIImage(cgImage: cutImageRef) 
        return croppedImage
    }
    
    // Alamofire를 통한 multipart 이미지전송 
AF.upload(multipartFormData: { multipartFormData in
            if let imageData = UIImageJPEGRepresentation(image, 1) {
                multipartFormData.append(imageData, withName: "image", fileName: "image.jpg", mimeType: "image/png")
            }}, to: url, method: .post, headers: ["Authorization": AUTO],
                encodingCompletion: { encodingResult in
                    switch encodingResult {
                    case .success(let upload, _, _):
                        
                        upload.uploadProgress { progress in
                            print(progress.fractionCompleted)
                            self.progressView.setProgress(Float(progress, animated: true)
                        }

                        upload.response { [weak self] response in
                            guard self != nil else {return}
                            do{
                                guard let json = try JSONSerialization.jsonObject(with: response.data!, options: .mutableContainers) as? [String:Any] else { return }
                            }catch let jsonErr{
                                print("Error Jason Serializing", jsonErr)
                            }
                        }
                    case .failure(let encodingError):
                        
                    }
        })
```
### 3. 분석 결과 DB 저장
![graph](https://user-images.githubusercontent.com/42457589/132491010-6ce3fa94-e88a-481f-99c9-8a591b5d6528.gif)  
DB에 저장된 결과를 불러올수 있다.


