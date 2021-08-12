---
title: "Modify the navigator"
categories:
  - javascript
  
tags:
  - selenium

---

## 브라우저 navigator 값 수정

셀레니움이라면 `driver.ExecuteChromeCommandWithResult(Page.addScriptToEvaluateOnNewDocument)` 을 통해서 실행시키면 됩니다.

```javascript
function __my__define__(d, p, v) {
    Object.defineProperty(d, p, 
        {
            get: function () { return v; },
            set: function (newValue) {}
        }
    );
}

__my__define__(navigator, 'platform', 'TEST');
__my__define__(navigator, 'userLanguage', 'TEST');

__my__define__(HTMLElement.prototype, 'clientWidth', 111);
__my__define__(HTMLElement.prototype, 'clientHeight', 111);

__my__define__(screen, 'width', 800);
__my__define__(screen, 'height', 600);


__my__define__(window, 'devicePixelRatio', 1);

__my__define__(screen, 'colorDepth', 999);

navigator.javaEnabled = function() { return true; };

__my__define__(navigator, 'cookieEnabled', false);
__my__define__(navigator, 'userAgent', 'Mozilla/5.0 (iPhone; CPU iPhone OS 8_4 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) GSA/7.0.55539 Mobile/12H143 Safari/600.1.4');

// Object.defineProperty(document.documentElement.prototype, 'clientWidth', {value: 123});
// document.documentElement.clientWidth
```