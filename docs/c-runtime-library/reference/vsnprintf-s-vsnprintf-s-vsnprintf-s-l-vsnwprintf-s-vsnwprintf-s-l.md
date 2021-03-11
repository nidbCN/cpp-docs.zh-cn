---
description: 了解详细信息： vsnprintf_s、_vsnprintf_s、_vsnprintf_s_l、_vsnwprintf_s、_vsnwprintf_s_l
title: vsnprintf_s、_vsnprintf_s、_vsnprintf_s_l、_vsnwprintf_s、_vsnwprintf_s_l
ms.date: 3/9/2021
api_name:
- _vsnwprintf_s
- _vsnwprintf_s_l
- _vsnprintf_s
- vsnprintf_s
- _vsnprintf_s_l
api_location:
- msvcrt.dll
- msvcr80.dll
- msvcr90.dll
- msvcr100.dll
- msvcr100_clr0400.dll
- msvcr110.dll
- msvcr110_clr0400.dll
- msvcr120.dll
- msvcr120_clr0400.dll
- ntdll.dll
- ucrtbase.dll
- ntoskrnl.exe
api_type:
- DLLExport
topic_type:
- apiref
f1_keywords:
- _vsnprintf_s
- _vsntprintf_s
- _vsnwprintf_s
helpviewer_keywords:
- vsnwprintf_s function
- _vsntprintf_s function
- _vsntprintf_s_l function
- vsntprintf_s function
- vsnwprintf_s_l function
- vsnprintf_s_l function
- vsntprintf_s_l function
- _vsnwprintf_s_l function
- _vsnprintf_s function
- vsnprintf_s function
- _vsnprintf_s_l function
- _vsnwprintf_s function
- formatted text [C++]
ms.openlocfilehash: c5f472c1ff481d4d940ac081bf04986cb18e5a78
ms.sourcegitcommit: b04b39940b0c1e265f80fc1951278fdb05a1b30a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622005"
---
# <a name="vsnprintf_s-_vsnprintf_s-_vsnprintf_s_l-_vsnwprintf_s-_vsnwprintf_s_l"></a>vsnprintf_s、_vsnprintf_s、_vsnprintf_s_l、_vsnwprintf_s、_vsnwprintf_s_l

使用指向参数列表的指针写入格式化的输出。 这些版本的 [vsnprintf、_vsnprintf、_vsnprintf_l、_vsnwprintf、_vsnwprintf_l](vsnprintf-vsnprintf-vsnprintf-l-vsnwprintf-vsnwprintf-l.md) 具有安全增强功能，如 [CRT 中的安全功能](../../c-runtime-library/security-features-in-the-crt.md)所述。

## <a name="syntax"></a>语法

```C
int vsnprintf_s(
   char *buffer,
   size_t sizeOfBuffer,
   size_t count,
   const char *format,
   va_list argptr
);
int _vsnprintf_s(
   char *buffer,
   size_t sizeOfBuffer,
   size_t count,
   const char *format,
   va_list argptr
);
int _vsnprintf_s_l(
   char *buffer,
   size_t sizeOfBuffer,
   size_t count,
   const char *format,
   locale_t locale,
   va_list argptr
);
int _vsnwprintf_s(
   wchar_t *buffer,
   size_t sizeOfBuffer,
   size_t count,
   const wchar_t *format,
   va_list argptr
);
int _vsnwprintf_s_l(
   wchar_t *buffer,
   size_t sizeOfBuffer,
   size_t count,
   const wchar_t *format,
   locale_t locale,
   va_list argptr
);
template <size_t size>
int _vsnprintf_s(
   char (&buffer)[size],
   size_t count,
   const char *format,
   va_list argptr
); // C++ only
template <size_t size>
int _vsnwprintf_s(
   wchar_t (&buffer)[size],
   size_t count,
   const wchar_t *format,
   va_list argptr
); // C++ only
```

### <a name="parameters"></a>参数

*宽限*<br/>
输出的存储位置

*sizeOfBuffer*<br/>
用于输出的 *缓冲区* 大小，作为字符计数。

*计数*<br/>
要写入的字符最大数量（不包括终止 null 或 [_TRUNCATE](../../c-runtime-library/truncate.md)）。

*format*<br/>
格式规范。

*argptr*<br/>
指向参数列表的指针。

*locale*<br/>
要使用的区域设置。

有关更多信息，请参见 [格式规范](../../c-runtime-library/format-specification-syntax-printf-and-wprintf-functions.md)。

## <a name="return-value"></a>返回值

**vsnprintf_s**， **_vsnprintf_s** 和 **_vsnwprintf_s** 返回写入的字符数，不包括终止 null，或者在发生数据截断或输出错误时返回一个负值。

* 如果 *count* 小于 *sizeOfBuffer* ，且数据的字符数小于或等于 *count*，或者 *count* 为 [_TRUNCATE](../../c-runtime-library/truncate.md) ，且数据的字符数小于 *sizeOfBuffer*，则写入所有数据并返回的字符数。

* 如果 *count* 小于 *sizeOfBuffer* 但数据超出了 *count* 个字符，则写入第一个 *计数* 字符。 出现剩余数据的截断，返回-1，而不调用无效的参数处理程序。

* 如果 *count* 是 [_TRUNCATE](../../c-runtime-library/truncate.md) 的，并且数据的字符数等于或超过 *sizeOfBuffer*，则在写入 *缓冲区* (时，将写入包含终止 null) 的最大字符串。 出现剩余数据的截断，返回-1，而不调用无效的参数处理程序。

* 如果 *count* 等于或超过 *sizeOfBuffer* ，但数据的字符数小于 *sizeOfBuffer*，则会将所有数据写入 (终止 null) ，并返回字符数。

* 如果数据的 *计数* 和数据的数量等于或超过 *sizeOfBuffer*，则调用无效参数处理程序，如 [参数验证](../../c-runtime-library/parameter-validation.md)中所述。 如果执行在无效的参数处理程序之后继续，则这些函数将 *缓冲区* 设置为空字符串，将 **Errno** 设置为 **ERANGE**，并返回-1。

* 如果 *buffer* 或 *format* 为 **空** 指针，或者 *count* 小于或等于零，则调用无效的参数处理程序。 如果允许执行继续，则这些函数会将 **errno** 设置为 **EINVAL** ，并返回-1。

### <a name="error-conditions"></a>错误条件

|**条件**|返回|**errno**|
|-----------------|------------|-------------|
|*缓冲区* 为 **NULL**|-1|**EINVAL**|
|*格式* 为 **NULL**|-1|**EINVAL**|
|*计数* <= 0|-1|**EINVAL**|
|*sizeOfBuffer* 太小 (和 *count* ！ = **_TRUNCATE**) |-1 (，并将 *缓冲区* 设置为空字符串) |**ERANGE**|

## <a name="remarks"></a>注解

**vsnprintf_s** 与 **_vsnprintf_s** 相同。 包含 **vsnprintf_s** 是为了符合 ANSI 标准。 保留 **_vnsprintf** 以便向后兼容。

其中每个函数都采用一个指向参数列表的指针，然后将给定数据的最多个字符的 *计数* 字符写入 *缓冲区* ，并追加一个终止 null。

如果 *count* 是 [_TRUNCATE](../../c-runtime-library/truncate.md)的，则这些函数会将尽可能多的字符串写入 *缓冲区* 中，同时为终止 null 留出空间。 如果包含终止 null) 的整个字符串 (符合 *缓冲区*，则这些函数将返回写入的字符数， (不包括终止 null) ;否则，这些函数将返回-1 以指示发生了截断。

这些具有 **_l** 后缀的函数的版本相同，只不过它们使用传入的区域设置参数而不是当前线程区域设置。

> [!IMPORTANT]
> 确保 format 不是用户定义的字符串。 有关详细信息，请参阅 [避免缓冲区溢出](/windows/win32/SecBP/avoiding-buffer-overruns)。
> 从 Windows 10 版本2004开始， (生成 19041) ， `printf` 函数系列按用于舍入的 IEEE 754 规则打印完全可表示的浮点数。 在以前版本的 Windows 中，准确地表示以 "5" 结尾的浮点数始终向上舍入。 IEEE 754 指出它们必须舍入到最接近的偶数 (也称为 "银行家舍入" ) 。 例如，和都 `printf("%1.0f", 1.5)` `printf("%1.0f", 2.5)` 应该舍入为2。 以前，1.5 将舍入为2，2.5 将舍入为3。 此更改只影响精确的可表示数字。 例如，2.35 (当在内存中表示时，) 将继续向上舍入到2.4。 这些函数所做的舍入现在还遵循由设置的浮点舍入模式 [`fesetround`](fegetround-fesetround2.md) 。 以前，舍入始终选择 `FE_TONEAREST` 行为。 此更改仅影响使用 Visual Studio 2019 版本16.2 和更高版本生成的程序。 若要使用旧的浮点舍入行为，请使用 ["legacy_stdio_float_rounding .obj"](../link-options.md)链接。

> [!NOTE]
> 若要确保终止 null 的空间，请确保 *计数* 严格小于缓冲区长度，或使用 **_TRUNCATE**。

在 C++ 中，使用这些函数由模板重载简化；重载可以自动推导出缓冲区长度 (不再需要指定大小自变量)，并且它们可以自动用以更新、更安全的对应物替换旧的、不安全的函数。 有关详细信息，请参阅[安全模板重载](../../c-runtime-library/secure-template-overloads.md)。

### <a name="generic-text-routine-mappings"></a>一般文本例程映射

|TCHAR.H 例程|未定义 _UNICODE 和 _MBCS|已定义 _MBCS|已定义 _UNICODE|
|---------------------|------------------------------------|--------------------|-----------------------|
|**_vsntprintf_s**|**_vsnprintf_s**|**_vsnprintf_s**|**_vsnwprintf_s**|
|**_vsntprintf_s_l**|**_vsnprintf_s_l**|**_vsnprintf_s_l**|**_vsnwprintf_s_l**|

## <a name="requirements"></a>要求

|例程所返回的值|必需的标头|可选标头|
|-------------|---------------------|----------------------|
|**vsnprintf_s**|\<stdio.h> 和 \<stdarg.h>|\<varargs.h>*|
|**_vsnprintf_s**， **_vsnprintf_s_l**|\<stdio.h> 和 \<stdarg.h>|\<varargs.h>*|
|**_vsnwprintf_s**， **_vsnwprintf_s_l**|\<stdio.h> 或 \<wchar.h> 、和 \<stdarg.h>|\<varargs.h>*|

\* 仅对 UNIX V 兼容性是必需的。

有关其他兼容性信息，请参阅[兼容性](../../c-runtime-library/compatibility.md)。

## <a name="example"></a>示例

```cpp
// crt_vsnprintf_s.cpp
#include <stdio.h>
#include <wtypes.h>

void FormatOutput(LPCSTR formatstring, ...)
{
   int nSize = 0;
   char buff[10];
   memset(buff, 0, sizeof(buff));
   va_list args;
   va_start(args, formatstring);
   nSize = vsnprintf_s( buff, _countof(buff), _TRUNCATE, formatstring, args);
   printf("nSize: %d, buff: %s\n", nSize, buff);
   va_end(args);
}

int main() {
   FormatOutput("%s %s", "Hi", "there");
   FormatOutput("%s %s", "Hi", "there!");
   FormatOutput("%s %s", "Hi", "there!!");
}
```

```Output
nSize: 8, buff: Hi there
nSize: 9, buff: Hi there!
nSize: -1, buff: Hi there!
```

## <a name="see-also"></a>另请参阅

[流 I/O](../../c-runtime-library/stream-i-o.md)<br/>
[vprintf 函数](../../c-runtime-library/vprintf-functions.md)<br/>
[fprintf、_fprintf_l、fwprintf、_fwprintf_l](fprintf-fprintf-l-fwprintf-fwprintf-l.md)<br/>
[printf、_printf_l、wprintf、_wprintf_l](printf-printf-l-wprintf-wprintf-l.md)<br/>
[sprintf、_sprintf_l、swprintf、_swprintf_l、 \_ _swprintf_l](sprintf-sprintf-l-swprintf-swprintf-l-swprintf-l.md)<br/>
[va_arg、va_copy、va_end、va_start](va-arg-va-copy-va-end-va-start.md)<br/>
