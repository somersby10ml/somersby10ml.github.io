---
title: "MessageBox 가변인수"
categories:
  - cpp
  
tags:
  - snippet

---
Variable Arguments C++

```cpp
#include <tchar.h>
void MyMessageBox(const TCHAR* pFormat, ...)
{
    TCHAR szData[500];
    va_list argptr;
    va_start(argptr, pFormat);
    _vstprintf(szData, _countof(szData), pFormat, argptr);
    va_end(argptr);
    MessageBox(NULL, szData, _T("MyMessageBox"), MB_OK | MB_ICONINFORMATION);
}
void MyOutputDebugString(const TCHAR* pFormat, ...)
{
    TCHAR szData[500];
    va_list argptr;
    va_start(argptr, pFormat);
    _vstprintf(szData, _countof(szData), pFormat, argptr);
    va_end(argptr);
    OutputDebugString(szData);
}
```

## AllocConsole
```cpp
FILE *fp_in, *fp_out, *fp_err;
AllocConsole();
freopen_s(&fp_in, "CONIN$", "r", stdin);
freopen_s(&fp_out, "CONOUT$", "w", stdout);
freopen_s(&fp_err, "CONOUT$", "w", stdout);

// printf("%08X\n", 0x11223344);

if (fp_err) fclose(fp_err);
if (fp_out) fclose(fp_out);
if (fp_in) fclose(fp_in);
_fcloseall();
FreeConsole();
```

## self unload DLL
Free all resources used by this dll.  
Create a thread and run from it  

```cpp
FreeLibraryAndExitThread(g_hModule, 0);
FreeLibrary(g_hModule);
```