---
title: "LdrRegisterDllNotification"
categories:
  - cpp
  
tags:
  - winapi

---
LdrRegisterDllNotification DLL이 로드될때 알려줍니다.  
[https://docs.microsoft.com/ko-kr/windows/win32/devnotes/ldrregisterdllnotification](https://docs.microsoft.com/ko-kr/windows/win32/devnotes/ldrregisterdllnotification)  


```cpp
#include <Windows.h>
#include <stdio.h>
#include <CONIO.H>
 
//#include <Ntsecapi.h> // DDK 
 
#ifndef _NTDEF_
    typedef LONG NTSTATUS, *PNTSTATUS;
#endif
 
#ifndef _NTSECAPI_
    typedef struct _LSA_UNICODE_STRING {
        USHORT Length;
        USHORT MaximumLength;
        PWSTR  Buffer;
    } LSA_UNICODE_STRING, *PLSA_UNICODE_STRING;
#endif
 
typedef const _LSA_UNICODE_STRING* PCUNICODE_STRING;  
  
typedef struct _LDR_DLL_LOADED_NOTIFICATION_DATA {  
    ULONG Flags;                    //Reserved.  
    PCUNICODE_STRING FullDllName;   //The full path name of the DLL module.  
    PCUNICODE_STRING BaseDllName;   //The base file name of the DLL module.  
    PVOID DllBase;                  //A pointer to the base address for the DLL in memory.  
    ULONG SizeOfImage;              //The size of the DLL image, in bytes.  
} LDR_DLL_LOADED_NOTIFICATION_DATA, *PLDR_DLL_LOADED_NOTIFICATION_DATA;  
  
typedef struct _LDR_DLL_UNLOADED_NOTIFICATION_DATA {  
    ULONG Flags;                    //Reserved.  
    PCUNICODE_STRING FullDllName;   //The full path name of the DLL module.  
    PCUNICODE_STRING BaseDllName;   //The base file name of the DLL module.  
    PVOID DllBase;                  //A pointer to the base address for the DLL in memory.  
    ULONG SizeOfImage;              //The size of the DLL image, in bytes.  
} LDR_DLL_UNLOADED_NOTIFICATION_DATA, *PLDR_DLL_UNLOADED_NOTIFICATION_DATA;  
  
typedef union _LDR_DLL_NOTIFICATION_DATA {  
    LDR_DLL_LOADED_NOTIFICATION_DATA Loaded;  
    LDR_DLL_UNLOADED_NOTIFICATION_DATA Unloaded;  
} LDR_DLL_NOTIFICATION_DATA, *PLDR_DLL_NOTIFICATION_DATA;  
 
typedef const PLDR_DLL_NOTIFICATION_DATA PCLDR_DLL_NOTIFICATION_DATA;  
 
typedef VOID (NTAPI *PLDR_DLL_NOTIFICATION_FUNCTION)(ULONG NotificationReason, PCLDR_DLL_NOTIFICATION_DATA NotificationData, PVOID Context);  
typedef NTSTATUS (NTAPI *pfnLdrRegisterDllNotification)(ULONG Flags, PLDR_DLL_NOTIFICATION_FUNCTION NotificationFunction, void* Context, void **Cookie);  
typedef NTSTATUS (NTAPI *pfnLdrUnregisterDllNotification)(void *Cookie);  
 
#define LDR_DLL_NOTIFICATION_REASON_LOADED 1   
#define LDR_DLL_NOTIFICATION_REASON_UNLOADED 2  
 
VOID NTAPI MyLdrDllNotification(  
                                ULONG NotificationReason,  
                                PCLDR_DLL_NOTIFICATION_DATA NotificationData,  
                                PVOID Context  
                                )  
{  
    switch (NotificationReason)  
    {  
    case LDR_DLL_NOTIFICATION_REASON_LOADED:  
        printf("Dll Loaded: %18S\n", NotificationData->Loaded.BaseDllName->Buffer);
        //printf("DLL Base Address : %8x\r\n\n",NotificationData->Loaded.DllBase);
        break;  
    case LDR_DLL_NOTIFICATION_REASON_UNLOADED:  
        printf("Dll Unloaded: %15S\n\n", NotificationData->Unloaded.BaseDllName->Buffer);  
        //printf("DLL Base Address : %8x\r\n\n",NotificationData->Unloaded.DllBase);
        break;  
    }  
}  
 
int main(int argc, char* argv[])  
{  
    
    HMODULE hModule = GetModuleHandleW(L"NTDLL.DLL");  
    if (!hModule)
    {
        puts("GetModuleHandle NTDLL.DLL : Failed!\r\n"
                "Press any key to continue");
        getch();
        return EXIT_FAILURE;
    }
    
    pfnLdrRegisterDllNotification pLdrRegisterDllNotification = (pfnLdrRegisterDllNotification)GetProcAddress(hModule, "LdrRegisterDllNotification");  
    pfnLdrUnregisterDllNotification pLdrUnregisterDllNotification = (pfnLdrUnregisterDllNotification)GetProcAddress(hModule, "LdrUnregisterDllNotification");  
   
    void *pvCookie = NULL;  
    pLdrRegisterDllNotification(0, MyLdrDllNotification, NULL, &pvCookie);  
    
    puts("[ Press any key to continue ]");
    fflush(stdin);
    getch();
    
    if (pvCookie)  
    {  
        pLdrUnregisterDllNotification(pvCookie);  
        pvCookie = NULL;  
    } 
    return 0;
}
```