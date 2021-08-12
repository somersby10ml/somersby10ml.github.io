---
title: "셀레니움 클릭하는 여러가지방법"
categories:
  - selenium
  
tags:
  - chromedriver
  - selenium

---
셀레니움에서 태그를 클릭하는 여러가지 방법   

공통 : IWebElement a = ~~~  

## Method 1
```csharp
a.Click();
```

# Method 2
```csharp
_driver.ExecuteScript("arguments[0].click();", a);
```

# Method 3
```csharp
Actions actionProvider = new Actions(_driver);
actionProvider.MoveToElement(a).Click().Perform();
```

## 설명
1번은 오류가 좀 많이납니다.  
추가로 막 div 같은거 클릭 잘 안되는거같고  
추가로 체크박스는 클릭도 안됩니다.  

그래서 저는 2번을 주로 사용합니다.  
아 그리고 키입력할때  IWebElement 의 SendKeys 는 
input type text 가 아니면 못씀  

span 이나 div 에 키보드입력을 하고싶을때..  
그럴땐 Actions 에 있는 SendKeys 를 사용하면 됩니다.  
