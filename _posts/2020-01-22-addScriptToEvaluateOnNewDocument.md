---
title: "addScriptToEvaluateOnNewDocument"
categories:
  - csharp
  
tags:
  - chromedriver
  - selenium

---

## addScriptToEvaluateOnNewDocument
[https://chromedevtools.github.io/devtools-protocol/tot/Page/#method-addScriptToEvaluateOnNewDocument](https://chromedevtools.github.io/devtools-protocol/tot/Page/#method-addScriptToEvaluateOnNewDocument){:target="_blank"}

생성시 모든 프레임에서 주어진 스크립트를 평가합니다 (프레임의 스크립트를로드하기 전에).  
즉, 셀레니움으로 페이지 접속시 모든프레임에서 나만의 자바스크립트를 먼저 실행시킬수 있습니다.  
모든 아이프레임(iframe) 에 대해서 최초 1회만 먼저  실행됩니다.  

String a = 

JavaScript
```javascript
Object.defineProperty(navigator,'userAgent', {
	    get: function () {
	        return 'MyAgent'; 
	    },
	    set: function (a) { }
	}
);
```


C#  
```csharp
var parameters = new Dictionary<string, object>
{
    ["source"] = a
};

Dictionary<string, object> q = driver.ExecuteChromeCommandWithResult("Page.addScriptToEvaluateOnNewDocument", parameters) as Dictionary<string, object>;
// 중지시킬려면 driver.ExecuteChromeCommand("Page.removeScriptToEvaluateOnNewDocument", q);
```