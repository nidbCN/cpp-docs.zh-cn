---
title: "_acmdln、_tcmdln、_wcmdln | Microsoft Docs"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- cpp-standard-libraries
ms.tgt_pltfrm: 
ms.topic: article
apiname:
- _wcmdln
- _acmdln
apilocation:
- msvcrt.dll
apitype: DLLExport
f1_keywords:
- _acmdln
- acmdln
- _wcmdln
- wcmdln
- _tcmdln
- tcmdln
dev_langs:
- C++
helpviewer_keywords:
- _wcmdln global variable
- wcmdln global variable
- _acmdln global variable
- _tcmdln global variable
- tcmdln global variable
- acmdln global variable
ms.assetid: 4fc0a6a0-3f93-420a-a19f-5276061ba539
caps.latest.revision: 7
author: corob-msft
ms.author: corob
manager: ghogen
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Human Translation
ms.sourcegitcommit: d6eb43b2e77b11f4c85f6cf7e563fe743d2a7093
ms.openlocfilehash: f2a5c42313699980828e663727dbc95039c23431
ms.contentlocale: zh-cn
ms.lasthandoff: 05/18/2017

---
# <a name="acmdln-tcmdln-wcmdln"></a>_acmdln、_tcmdln、_wcmdln
内部 CRT 全局变量。 命令行。  
  
## <a name="syntax"></a>语法  
  
```  
char * _acmdln;  
wchar_t * _wcmdln;  
  
#ifdef WPRFLAG  
   #define _tcmdln _wcmdln  
#else  
   #define _tcmdln _acmdln  
```  
  
## <a name="remarks"></a>备注  
 这些 CRT 内部变量将存储完整的命令行。 将在 CRT 的导出符号中公开它们，但不会在代码中使用它们。 `_acmdln` 将数据存储为字符字符串。 `_wcmdln` 将数据存储为宽字符字符串。 可将 `_tcmdln` 定义为 `_acmdln` 或 `_wcmdln`，具体取决于哪一个是合适的。  
  
## <a name="see-also"></a>另请参阅  
 [全局变量](../c-runtime-library/global-variables.md)