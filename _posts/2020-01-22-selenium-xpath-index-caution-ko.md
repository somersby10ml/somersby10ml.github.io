---
title: "셀레니움 xpath index 주의사항"
categories:
  - selenium
  
tags:
  - chromedriver
  - selenium

---

xpath 를 사용하면서 index 부분을 다뤄야하는데 last() 를 사용하다가 잘못된 element 를 잡아서 검색해보니...  
`//a//b[last()]`  
이러한 소스가 있으면  [] 가 우선순위가 더 높기때문에  

b[last()] 작업을 먼저하고   
//a 를 진행한다는것.  
그래서 b의 마지막 요소를 찾으려면  
(//a//b)[last()] 이라고 해주어야 합니다.  

​

reference : [https://stackoverflow.com/questions/36754697/why-is-xpath-last-function-not-working-as-i-expect](https://stackoverflow.com/questions/36754697/why-is-xpath-last-function-not-working-as-i-expect){:target="_blank"}
 