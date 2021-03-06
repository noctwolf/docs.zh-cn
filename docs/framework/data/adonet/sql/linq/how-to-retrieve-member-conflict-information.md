---
title: 如何：检索成员冲突信息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7dd6829e-79a5-4480-9023-9e588cb0bf2e
ms.openlocfilehash: fae1513e7a7ead98318d907b220b7510758c9ffe
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "62037597"
---
# <a name="how-to-retrieve-member-conflict-information"></a>如何：检索成员冲突信息
您可以使用 <xref:System.Data.Linq.MemberChangeConflict> 类检索有关发生冲突的各成员的信息。 在此上下文中，您可以提供任何成员的冲突的自定义处理方法。 有关详细信息，请参阅[开放式并发：概述](../../../../../../docs/framework/data/adonet/sql/linq/optimistic-concurrency-overview.md)。  
  
## <a name="example"></a>示例  
 下面的代码循环访问 <xref:System.Data.Linq.ObjectChangeConflict> 对象。 对于每个对象，它会接着循环访问 <xref:System.Data.Linq.MemberChangeConflict> 对象。  
  
> [!NOTE]
>  请将 <xref:System.Reflection> 包含在内以提供 <xref:System.Data.Linq.MemberChangeConflict.Member%2A> 信息。  
  
 [!code-csharp[System.Data.Linq.MemberChangeConflict#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.memberchangeconflict/cs/program.cs#1)]
 [!code-vb[System.Data.Linq.MemberChangeConflict#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.memberchangeconflict/vb/module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：管理更改冲突](../../../../../../docs/framework/data/adonet/sql/linq/how-to-manage-change-conflicts.md)
