---
title: "Selenium HttpWebRequests Session sharing"
categories:
  - csharp
  
tags:
  - selenium
  - snippet

---


```csharp
CookieCollection cc = new CookieCollection();
foreach (OpenQA.Selenium.Cookie cook in _driver.Manage().Cookies.AllCookies)
{
    System.Net.Cookie cookie = new System.Net.Cookie();
    cookie.Name = cook.Name;
    cookie.Value = cook.Value;
    cookie.Domain = cook.Domain;
    cc.Add(cookie);
}
cookieContainer.Add(cc);
CookieContainer cookieContainer = new CookieContainer();
HttpWebRequest httpWebRequest;
httpWebRequest.CookieContainer = cookieContainer;
```