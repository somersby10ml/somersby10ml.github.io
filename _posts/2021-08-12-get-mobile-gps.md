---
title: "모바일 사용자의 위도 경도 알아오기"
categories:
  - javascript
  
tags:
  - gps

---

## 요약
JavaScript 에서 모바일 사용자의 GPS 정보를 얻어오는 방법은 `navigator.geolocation` 를 사용합니다.  
이를 사용할시 사용자에게 동의를 구하게 됩니다.  
![이미지1](/assets/img/getmobilegps/image1.png)


## 함수
`navigator.geolocation` 에 있는 함수는 아래와 같습니다.  
- **getCurrentPosition**  장치의 현재 위치를 검색합니다.
- **watchPosition** 장치의 현재 위치를 주기적으로 업데이트 합니다.
- **clearWatch** 진행중인 watchPosition 호출을 취소합니다.


얻어오는 값은 아래와 같습니다.
- **accuracy** 위도/경도 의 오차(단위 m)
- **altitude** 고도
- **altitudeAccuracy** 고도의 오차(단위 m)
- **heading** 방위
- **latitude** 위도
- **longitude** 경도
- **speed** 속도


옵션은 아래와 같습니다.
- **enableHighAccuracy** 위젯이 가능한 가장 정확한 위치 추정치를 수신할 것인지 여부를 지정합니다. 기본적으로 이것은 거짓입니다.
- **timeout** timeout 속성은 웹 응용 프로그램이 위치를 기다리는 시간(밀리초)입니다.
- **maximumAge** 캐시된 위치 정보의 만료 시간을 밀리초 단위로 지정합니다.

## 샘플소스
```html
<!DOCTYPE html>
<html>

<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>위치API</title>
  <style>
    .text {
      width: 500px;
      height: 100px;
    }
  </style>
</head>

<body>
  accuracy(정확도, 위도/경도의 오차m) : <label id="accuracy"></label><br>
  altitude(고도) : <label id="altitude"></label><br>
  altitudeAccuracy(고도의 오차) : <label id="altitudeAccuracy"></label><br>
  heading(방위) : <label id="heading"></label><br>
  latitude(위도) : <label id="latitude"></label><br>
  longitude(경도) : <label id="longitude"></label><br>
  speed(속도) : <label id="speed"></label><br>

  <script>
    const $ = e => document.querySelector(e);

    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => { // allowe
          console.log(position.coords);
          $("#accuracy").innerText = position.coords.accuracy;
          $("#altitude").innerText = position.coords.altitude;
          $("#altitudeAccuracy").innerText = position.coords.altitudeAccuracy;
          $("#heading").innerText = position.coords.heading;
          $("#latitude").innerText = position.coords.latitude;
          $("#longitude").innerText = position.coords.longitude;
          $("#speed").innerText = position.coords.speed;
        },
        (error) => { // deny
          let x = "";
          switch (error.code) {
            case error.PERMISSION_DENIED:
              x = "User denied the request for Geolocation."
              break;
            case error.POSITION_UNAVAILABLE:
              x = "Location information is unavailable."
              break;
            case error.TIMEOUT:
              x = "The request to get user location timed out."
              break;
            case error.UNKNOWN_ERROR:
              x = "An unknown error occurred."
              break;
          }
          alert(x);
        },
        { // options
          enableHighAccuracy: true
        }
      );
    }
    else {
      alert("Geolocation is not supported by this browser.");
    }
  </script>
</body>
</html>
```

## 참조
[https://www.w3schools.com/html/html5_geolocation.asp](https://www.w3schools.com/html/html5_geolocation.asp){:target="_blank"}  
[https://www.tutorialspoint.com/html5/html5_geolocation.htm](https://www.tutorialspoint.com/html5/html5_geolocation.htm){:target="_blank"}  

