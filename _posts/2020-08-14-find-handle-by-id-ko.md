---
title: 아이디 값으로 핸들찾기
categories:
  - cpp
  
tags:
  - snippet
---

**FindWindowEx** 를 써서 핸들값을 구할경우  
프로그램이 약간 느리게 로딩이 되어버리면  
(CreateWindowEx) 완료가 덜 되었으면   
다른핸들값을 찾아버릴수도 있습니다.   
그래서 아이디로 구분하면 편하게 찾을수 있습니다.


**EnumChildWindows** 는 모든 자식의 핸들값을 열거해줍니다.  
**GetDlgCtrlID**  는 핸들값을 받으면 컨트롤아이디를 반환해줍니다.  

이것을 사용해주면 **FindWindowEx** 를 사용안하고 쉽게 정확한 핸들값을 찾을수 있습니다.  
이는 프로그램이 늦게켜질경우를 생각해서 핸들값을 못찾았으면 무한루프를 합니다.  

​

```cpp
BOOL CALLBACK EnumChildProc(HWND hwnd, LPARAM lParam)
{
    if (GetDlgCtrlID(hwnd) == 1) {
        HWND* hDlg = reinterpret_cast<HWND*>(lParam);
        *hDlg = hwnd;
        return false;
    }
    return true;
}

int main()
{
    HWND hDlg = NULL;
    HWND hWnd = FindWindow(NULL, _T("title"));
    while (EnumChildWindows(hWnd, EnumChildProc, (LPARAM)&hDlg))
    {
        Sleep(10);
    }
    printf("hDlg : %X\n", hDlg);

    return 0;
}
```