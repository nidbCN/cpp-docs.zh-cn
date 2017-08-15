---
title: "deque::begin (STL/CLR) | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "reference"
f1_keywords: 
  - "cliext::deque::begin"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "begin 成员 [STL/CLR]"
ms.assetid: e99d20d2-bb33-415f-9bd6-fe331d8c2ba2
caps.latest.revision: 15
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
caps.handback.revision: 13
---
# deque::begin (STL/CLR)
[!INCLUDE[vs2017banner](../assembler/inline/includes/vs2017banner.md)]

指定受控序列的开头。  
  
## 语法  
  
```  
iterator begin();  
```  
  
## 备注  
 指定控件成员函数返回序列中的第一个元素的随机访问迭代器，或者除一个空序列的结尾。  用于获取一个迭代器，该迭代器指定受控序列的 `current` 开头，但如果受控序列的长度发生更改，则该迭代器的状态也会发生更改。  
  
## 示例  
  
```  
// cliext_deque_begin.cpp   
// compile with: /clr   
#include <cliext/deque>   
  
int main()   
    {   
    cliext::deque<wchar_t> c1;   
    c1.push_back(L'a');   
    c1.push_back(L'b');   
    c1.push_back(L'c');   
  
// display initial contents " a b c"   
    for each (wchar_t elem in c1)   
        System::Console::Write(" {0}", elem);   
    System::Console::WriteLine();   
  
// inspect first two items   
    cliext::deque<wchar_t>::iterator it = c1.begin();   
    System::Console::WriteLine("*begin() = {0}", *it);   
    System::Console::WriteLine("*++begin() = {0}", *++it);   
  
// alter first two items and reinspect   
    *--it = L'x';   
    *++it = L'y';   
    for each (wchar_t elem in c1)   
        System::Console::Write(" {0}", elem);   
    System::Console::WriteLine();   
    return (0);   
    }  
  
```  
  
  **a b c**  
**\*begin\(\) \=。**  
**\*\+\+begin\(\) \= b**  
 **x \- Y 的 c**   
## 要求  
 **页眉：** \<cliext\/deque\>  
  
 **命名空间：** cliext  
  
## 请参阅  
 [deque](../dotnet/deque-stl-clr.md)   
 [deque::end](../dotnet/deque-end-stl-clr.md)   
 [deque::front](../dotnet/deque-front-stl-clr.md)   
 [deque::front\_item](../dotnet/deque-front-item-stl-clr.md)