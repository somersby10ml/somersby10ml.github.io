---
title: "윈도우 레지스트리 트릭"
categories:
  - windows
  
tags:
  - trick

---

## 윈도우 레지스트리 트릭 (Windows Registry Trick)
해당 트릭은 **Registry Win32API** 가 **NULL** 문자열로 발생합니다.  

레지스트리 경로가 `SOFTWARE\a_MyProgram` 일때 여기서  
`SOFTWARE\a_\x00\x00_MyProgram` 이렇게 추가하게 되면  
**Win32API** 를 사용한 프로그램은 중간에 문자열이 짤려서 해당 레지스트리 경로가 없는 오류가 나오게 되어 정상적으로 열리지 않습니다.  

`Regedit` `Regworkshop` `PC Hunter` `RegEditX` 로는 위와같은 방법을 사용한 레지스트리는 정상적으로 편집할수 없었습니다.  

## Win32API 동작흐름
아래의 동작 환경은 Windows10 64bit UserMode 에서 작성한 소스코드입니다.  

보통 아래와 비슷한 형태로 레지스트리를 읽습니다.  
```cpp
  const LPCWSTR regPath = LR"(SOFTWARE\a_MyProgram)";
  HKEY hKey;
  if (RegOpenKeyExW(HKEY_LOCAL_MACHINE, regPath, 0, KEY_ALL_ACCESS, &hKey) == ERROR_SUCCESS) {
      _tprintf(TEXT("Success\n"));
      RegCloseKey(hKey);
  }
  else {
      _tprintf(TEXT("Error\n"));
  }
```

하지만 레지스트리의 경로 `a_MyProgram` 의 문자열에 **NULL** 이 들어가면 정상적으로 열리지 않습니다.  
그 이유는 **Win32API** 에서는 **LPCWSTR** 를 사용하기 때문입니다.  
즉, 문자열의 포인터만 넘겨주기 때문에 **NULL** 문자열이 있으면 딱 거기까지만 문자열로 받아드리며 뒤에는 잘리게 됩니다.  
그래서 이는 **Win32API RegCreateKeyExW RegOpenKeyExW** 로 레지스트리를 생성하거나 읽을수 없습니다.  


하지만 **NTAPI** 를 사용하면 **NULL** 문자열이 들어가 있어도 레지스트리에 추가가 가능합니다.  
**NTAPI** 는 **UNICODE_STRING** 을 사용합니다.  
```cpp
typedef struct _UNICODE_STRING {
    USHORT Length;
    USHORT MaximumLength;
    PWSTR  Buffer;
} UNICODE_STRING, * PUNICODE_STRING;
```
문자열 중간에 **NULL** 이 들어가도 Length 만큼 읽고 레지스트리에 쓰기 때문입니다.  
**RegEnumValue** 또는 **ZwEnumerateKey** 로 **NULL** 문자열을 포함한 전체적인 경로를 받아올 수 있습니다.  

```cpp
HANDLE hKey;
WCHAR regPath[] = L"\\Registry\\Machine\\SOFTWARE\\a_\x00\x00MyProgram";
DWORD dwBufferSize = sizeof(regPath) - 2;

UNICODE_STRING uni = { dwBufferSize, dwBufferSize, (PWSTR)regPath };
OBJECT_ATTRIBUTES ObjectAttr = { 0, };
ObjectAttr.Length = sizeof(OBJECT_ATTRIBUTES);
ObjectAttr.ObjectName = &uni;
ObjectAttr.Attributes = OBJ_KERNEL_HANDLE | OBJ_CASE_INSENSITIVE;

// Create
if (ZwCreateKey(&hKey, KEY_ALL_ACCESS, &ObjectAttr, NULL, NULL, NULL, NULL) == STATUS_SUCCESS) {
    _tprintf(TEXT("ZwCreateKey Success\n"));
    ZwClose(hKey);
}
else {
    _tprintf(TEXT("Failure\n"));
}

// Open
DWORD dwStatus = ZwOpenKey(&hKey, KEY_ALL_ACCESS, &ObjectAttr);
if (dwStatus == STATUS_SUCCESS) {
    _tprintf(TEXT("ZwOpenKey Success\n"));

    _getch();

    // Delete
    DWORD a = ZwDeleteKey(hKey);
    _tprintf(TEXT("ZwDeleteKey: %08X\n"), a);

    ZwClose(hKey);
}
else {
    _tprintf(TEXT("Failure\n"));
    _tprintf(TEXT("%X\n"), dwStatus);
}
```
이런 형태로 생성 열고 삭제가 가능합니다.  
`_getch();` 부분에서 레지스트리 편집기로 보면 문자열이 짤려있음을 확인할수 있습니다.  
하위키 까지 있으면 더이상 값이 안보이는 상태가 되어버립니다.  

[https://github.com/somersby10ml/registrytrick](https://github.com/somersby10ml/registrytrick){:target="_blank"}

![Image1](/assets/img/registrytrick/img1.png)
![Image2](/assets/img/registrytrick/img2.png)
![Image3](/assets/img/registrytrick/img3.png)
![Image4](/assets/img/registrytrick/img4.png)
