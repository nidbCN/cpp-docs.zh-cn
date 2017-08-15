---
title: "CDaoIndexInfo 结构 |Microsoft 文档"
ms.custom: 
ms.date: 11/04/2016
ms.reviewer: 
ms.suite: 
ms.technology:
- cpp-windows
ms.tgt_pltfrm: 
ms.topic: article
f1_keywords:
- CDaoIndexInfo
dev_langs:
- C++
helpviewer_keywords:
- DAO (Data Access Objects), Indexes collection
- CDaoIndexInfo structure
ms.assetid: 251d8285-78ce-4716-a0b3-ccc3395fc437
caps.latest.revision: 13
author: mikeblome
ms.author: mblome
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
ms.translationtype: Machine Translation
ms.sourcegitcommit: 040985df34f2613b4e4fae29498721aef15d50cb
ms.openlocfilehash: 92206d8d8f9b2315fb859e2712a83d32a4c293ad
ms.contentlocale: zh-cn
ms.lasthandoff: 02/24/2017

---
# <a name="cdaoindexinfo-structure"></a>CDaoIndexInfo 结构
`CDaoIndexInfo`结构包含有关为数据访问对象 (DAO) 定义的索引对象的信息。  
  
## <a name="syntax"></a>语法  
  
```  
struct CDaoIndexInfo {  
    CDaoIndexInfo();
*// Constructor  
    CString m_strName;  // Primary  
    CDaoIndexFieldInfo* m_pFieldInfos;  // Primary  
    short m_nFields;    // Primary  
    BOOL m_bPrimary;    // Secondary  
    BOOL m_bUnique;     // Secondary  
    BOOL m_bClustered;  // Secondary  
    BOOL m_bIgnoreNulls;                // Secondary  
    BOOL m_bRequired;   // Secondary  
    BOOL m_bForeign;    // Secondary  
    long m_lDistinctCount;              // All  
 *// Below the // Implementation comment: *// Destructor, not otherwise documented  
};   
```  
  
#### <a name="parameters"></a>参数  
 `m_strName`  
 唯一地命名字段对象。 有关详细信息，请参阅主题 DAO 帮助中的"名称属性"。  
  
 `m_pFieldInfos`  
 指向数组的指针[CDaoIndexFieldInfo](../../mfc/reference/cdaoindexfieldinfo-structure.md)对象指示哪些 tabledef 或记录集字段在索引中的键字段。 每个对象标识在索引中的一个字段。 默认索引排序为升序。 Index 对象可以表示为每个记录的索引键的一个或多个字段。 可以对这些进行升序、 降序或组合。  
  
 `m_nFields`  
 存储在中的字段数`m_pFieldInfos`。  
  
 *m_bPrimary*  
 如果主属性为**TRUE**，索引对象都表示主索引。 主索引包含唯一标识表中预定义顺序中的所有记录的一个或多个字段。 索引字段必须是唯一的因为索引对象的唯一属性也设置为**TRUE**在 DAO 中。 如果主索引由多个字段组成，每个字段可以包含重复值，但所有索引的字段值的每个组合必须是唯一。 主索引包含表的键，并且通常包含的主键相同的字段。  
  
 当设置表的主键时，主键被自动定义为表的主索引。 有关详细信息，请参阅"主属性"和"唯一 Property"DAO 帮助中的主题。  
  
> [!NOTE]
>  可以有，最多一个主索引的表。  
  
 *m_bUnique*  
 指示索引对象是否表示一个表的唯一索引。 如果此属性为**TRUE**，index 对象代表中是唯一的索引。 唯一索引包含在逻辑上排列中唯一的、 预定义顺序的表中的所有记录的一个或多个字段。 如果该索引包含的一个字段，该字段中的值必须是唯一的整个表。 如果索引包含多个字段，每个字段可以包含重复值，但所有索引的字段值的每个组合必须唯一。  
  
 如果 Unique 和主索引对象的属性设置为**TRUE**，索引是唯一和主要︰ 它唯一地标识表中预定义的逻辑顺序中的所有记录。 如果主属性设置为**FALSE**，该索引是辅助索引。 辅助索引 （密钥和非键） 在逻辑上排列预定义顺序中的记录，但不作为表中的记录的标识符。  
  
 有关详细信息，请参阅"主属性"和"唯一 Property"DAO 帮助中的主题。  
  
 *m_bClustered*  
 指示索引对象是否表示一个表的聚集的索引。 如果此属性为**TRUE**，索引对象都表示一个聚集的索引; 否则，不允许。 聚集的索引包含一个或多个非键字段、 合起来看，排列中预定义顺序的表中的所有记录。 对于非聚集索引，表中的数据按原义存储在聚集索引按指定的顺序。 聚集的索引提供了高效访问表中的记录。 有关详细信息，请参阅主题 DAO 帮助中的"群集属性"。  
  
> [!NOTE]
>  对于使用 Microsoft Jet 数据库引擎，因为 Jet 数据库引擎不支持聚集的索引的数据库时将忽略群集属性。  
  
 *m_bIgnoreNulls*  
 指示是否在其索引字段中具有 Null 值的记录的索引条目。 如果此属性为**TRUE**，值为 Null 的字段没有任何索引条目。 若要使搜索更快地使用的字段的记录，您可以定义字段的索引。 如果不允许在索引的字段中的 Null 项和期待的许多条目为 Null，则可以设置到的索引对象的 IgnoreNulls 属性**TRUE**以减少这些索引使用的存储空间量。 IgnoreNulls 属性设置以及所需的属性的设置一起确定具有 Null 索引值的记录是否索引条目，如下表所示。  
  
|IgnoreNulls|必需|在索引字段中，则为 null|  
|-----------------|--------------|-------------------------|  
|True|False|允许为 null 值未添加的索引条目。|  
|False|False|允许为 null 值已添加的索引条目。|  
|True 或 False|True|不允许; 的 null 值未添加的索引条目。|  
  
 有关详细信息，请参阅主题 DAO 帮助中的"IgnoreNulls 属性"。  
  
 `m_bRequired`  
 指示是否 DAO index 对象需要非 Null 值。 如果此属性为**TRUE**，index 对象不允许 Null 值。 有关详细信息，请参阅主题 DAO 帮助中的"所需属性"。  
  
> [!TIP]
>  在可以设置此属性对于 DAO index 对象或 field 对象 （包含按 tabledef、 记录集或 querydef 对象中），则将其设置为字段对象。 在此索引对象之前检查字段对象的属性设置的有效性。  
  
 *m_bForeign*  
 指示索引对象是否表示一个表中的外键。 如果此属性为**TRUE**，索引表示一个表中的外键。 外键包括一个或多个表中的字段外，用于唯一标识主表中的行。 Microsoft Jet 数据库引擎创建外部表的索引对象和设置外属性后创建的关系，将强制引用完整性。 有关详细信息，请参阅主题 DAO 帮助中的"外属性"。  
  
 *m_lDistinctCount*  
 指示索引对象的关联的表中包含的唯一值的个数。 检查非重复计数属性以确定的唯一值或在索引中的键数。 任意键是只计算一次，即使在索引允许重复的值可能是该值的多个匹配项。 此信息很有用的应用程序尝试通过评估索引信息优化数据访问。 唯一值的数目也称为是索引对象的基数。 DistinctCount 属性不会始终反映实际的密钥数在特定时间。 例如，由事务回滚引起的更改将不会立即反映在非重复计数属性。 有关详细信息，请参阅主题 DAO 帮助中的"非重复计数属性"。  
  
## <a name="remarks"></a>备注  
 对主、 辅助数据库，并且所有上面引用指示如何通过返回的信息`GetIndexInfo`类中的成员函数[CDaoTableDef](../../mfc/reference/cdaotabledef-class.md#getindexinfo)和[CDaoRecordset](../../mfc/reference/cdaorecordset-class.md#getindexinfo)。  
  
 索引对象不由 MFC 类表示。 相反，DAO 对象类的基础 MFC 对象[CDaoTableDef](../../mfc/reference/cdaotabledef-class.md)或[CDaoRecordset](../../mfc/reference/cdaorecordset-class.md)包含称为索引集合的索引对象的集合。 这些类提供成员函数来访问各个项的索引信息，或可以访问它们同时与`CDaoIndexInfo`对象通过调用`GetIndexInfo`包含对象的成员函数。  
  
 `CDaoIndexInfo`具有一个构造函数和析构函数以便正确地分配和解除分配中的索引字段信息`m_pFieldInfos`。  
  
 检索的信息`GetIndexInfo`tabledef 对象成员函数将存储在`CDaoIndexInfo`结构。 调用`GetIndexInfo`包含 tabledef 对象的索引集合中存储的索引对象的成员函数。 `CDaoIndexInfo`此外定义了`Dump`成员函数在调试生成。 您可以使用`Dump`转储的内容`CDaoIndexInfo`对象。  
  
## <a name="requirements"></a>要求  
 **标头︰** afxdao.h  
  
## <a name="see-also"></a>另请参阅  
 [结构、 样式、 回调和消息映射](../../mfc/reference/structures-styles-callbacks-and-message-maps.md)   
 [CDaoTableDef::GetIndexInfo](../../mfc/reference/cdaotabledef-class.md#getindexinfo)

