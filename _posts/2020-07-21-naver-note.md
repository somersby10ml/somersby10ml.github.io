---
title: "[tampermonkey] 네이버 쪽지 알림"
categories:
  - javascript
  
tags:
  - tampermonkey
  - edge case
---

텔레그램 으로 알림이 옵니다.   
[네이버 쪽지](https://note.naver.com/){:target="_blank"} 를 계속 켜고있어야합니다.  


```javascript
// ==UserScript==
// @name         네이버 쪽지 확인
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       You
// @match        https://note.naver.com/
// @grant        GM_xmlhttpRequest
// ==/UserScript==
(function() {
  'use strict';
  let a = document.querySelectorAll(".notelist_item > li").length;
  let b = document.querySelectorAll(".read").length;
  if (a != b) {
    GM_xmlhttpRequest({
      method: "GET",
      url: "https://api.telegram.org/bot{봇토큰}/sendMessage?chat_id={채팅방고유번호}&text=메세지읽으세요",
      headers:  {
        "Content-Type": "application/x-www-form-urlencoded"
      },
      onload: function(response) {
        console.log(response)
      }
    });
  }
  setTimeout(function(){
    location.reload();
  }, 5000);
})();
```