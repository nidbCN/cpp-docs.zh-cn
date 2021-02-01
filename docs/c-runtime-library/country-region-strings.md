---
title: 国家-地区字符串
description: 了解详细信息：国家/地区字符串
ms.date: 1/29/2020
helpviewer_keywords:
- country/region strings
ms.openlocfilehash: 34494eb2bcace615e72127ab1735101809e8ade5
ms.sourcegitcommit: beac3ddf1a20de5e836569ae07407d5f3703f536
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99224432"
---
# <a name="countryregion-strings"></a>Country/Region Strings

可以将国家和地区字符串与语言字符串结合使用，以便创建 `setlocale`、 `_wsetlocale`、 `_create_locale`和 `_wcreate_locale` 函数的区域设置规范。 

有关各种 Windows 操作系统版本支持的国家/地区名称的列表，请参阅 [附录 A：产品行为](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c)（MS-LCID）中表的 **语言**、**位置** 和 **语言标记** 列 \[ ： Windows 语言代码标识符 (LCID) 引用。 有关枚举可用区域设置名称和相关值的代码示例，请参阅 [NLS：基于名称的 API 示例](/windows/win32/intl/nls--name-based-apis-sample)。

## <a name="additional-supported-country-and-region-strings"></a>其他受支持的国家和地区字符串

Microsoft C 运行时库实现还支持以下其他国家/地区字符串和缩写：

|国家/地区字符串|缩写|等效区域设置名称|
|----------------------------|------------------|----------------------------|
|`america`|`USA`|`en-US`|
|`britain`|`GBR`|`en-GB`|
|`china`|`CHN`|`zh-CN`|
|`czech`|`CZE`|`cs-CZ`|
|`england`|`GBR`|`en-GB`|
|`great britain`|`GBR`|`en-GB`|
|`holland`|`NLD`|`nl-NL`|
|`hong-kong`|`HKG`|`zh-HK`|
|`new-zealand`|`NZL`|`en-NZ`|
|`nz`|`NZL`|`en-NZ`|
|`pr china`|`CHN`|`zh-CN`|
|`pr-china`|`CHN`|`zh-CN`|
|`puerto-rico`|`PRI`|`es-PR`|
|`slovak`|`SVK`|`sk-SK`|
|`south africa`|`ZAF`|`af-ZA`|
|`south korea`|`KOR`|`ko-KR`|
|`south-africa`|`ZAF`|`af-ZA`|
|`south-korea`|`KOR`|`ko-KR`|
|`trinidad & tobago`|`TTO`|`en-TT`|
|`uk`|`GBR`|`en-GB`|
|`united-kingdom`|`GBR`|`en-GB`|
|`united-states`|`USA`|`en-US`|
|`us`|`USA`|`en-US`|

## <a name="see-also"></a>另请参阅

[区域设置名称、语言和国家/地区字符串](../c-runtime-library/locale-names-languages-and-country-region-strings.md)<br/>
[语言字符串](../c-runtime-library/language-strings.md)<br/>
[setlocale、_wsetlocale](../c-runtime-library/reference/setlocale-wsetlocale.md)<br/>
[_create_locale、_wcreate_locale](../c-runtime-library/reference/create-locale-wcreate-locale.md)
