---
title: "파이썬 셀레니움 자동설치"
categories:
  - selenium
  
tags:
  - chromedriver
  - selenium
  - python

---

현재 크롬버전의 드라이버를 자동으로 다운로드해줍니다.

## 설치
```shell
pip3 install chromedriver-autoinstaller
```

## 사용
```python
from selenium import webdriver
import chromedriver_autoinstaller


chromedriver_autoinstaller.install()  # Check if the current version of chromedriver exists
                                      # and if it doesn't exist, download it automatically,
                                      # then add chromedriver to path

driver = webdriver.Chrome()
driver.get("http://www.python.org")
assert "Python" in driver.title
```

reference : [https://pypi.org/project/chromedriver-autoinstaller/](https://pypi.org/project/chromedriver-autoinstaller/){:target="_blank"}
