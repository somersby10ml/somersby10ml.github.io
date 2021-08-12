---
title: (PE32) Add Section C++
categories:
  - cpp
  
tags:
  - pe32+
  - Section
---

add Section from pe memory.  
This will reallocate the space, so if you want to use the PE headers it will be parsed again.

```c++
DWORD Align(DWORD size, DWORD align) {
    if (!(size % align))
        return size;
    return (size / align + 1) * align;
}

DWORD pe::addSection(const char* sectionName, DWORD rawSize) {
    
    PIMAGE_SECTION_HEADER LastSection = &pSectionHeader[pNtHeader->FileHeader.NumberOfSections - 1];
    PIMAGE_SECTION_HEADER NewSection = &pSectionHeader[pNtHeader->FileHeader.NumberOfSections];

    // 섹션구조체 작성
    memcpy(NewSection->Name, sectionName, 8);
    NewSection->Characteristics = IMAGE_SCN_CNT_CODE | IMAGE_SCN_MEM_READ | IMAGE_SCN_MEM_WRITE | IMAGE_SCN_MEM_EXECUTE;
    NewSection->PointerToRawData = Align(LastSection->PointerToRawData + LastSection->SizeOfRawData, pNtHeader->OptionalHeader.FileAlignment);
    NewSection->SizeOfRawData = Align(rawSize, pNtHeader->OptionalHeader.FileAlignment);
    NewSection->VirtualAddress = Align(LastSection->VirtualAddress + LastSection->Misc.VirtualSize, pNtHeader->OptionalHeader.SectionAlignment);
    NewSection->Misc.VirtualSize = Align(rawSize, pNtHeader->OptionalHeader.SectionAlignment);

    pNtHeader->FileHeader.NumberOfSections++;
    pNtHeader->OptionalHeader.SizeOfImage = pNtHeader->OptionalHeader.SizeOfImage + NewSection->Misc.VirtualSize;

    dwFileSize += rawSize;
    lpBinary = (unsigned char*)realloc(lpBinary, dwFileSize);

    ParserPE();
    return pNtHeader->FileHeader.NumberOfSections - 1;
}
```

<style>
  pre code {
    font-family: Consolas;
    font-size: 12px;
  }
</style>
