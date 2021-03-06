---
title: 创建 DataView 对象 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 76057508-e12d-4779-a707-06a4c2568acf
ms.openlocfilehash: bd39b40864703b6bb24c2cc6590787562f3f4f98
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2019
ms.locfileid: "67504147"
---
# <a name="creating-a-dataview-object-linq-to-dataset"></a>创建 DataView 对象 (LINQ to DataSet)
有两种方法来创建<xref:System.Data.DataView>LINQ 到数据集上下文中。 您可以创建<xref:System.Data.DataView>LINQ to DataSet 查询通过从<xref:System.Data.DataTable>，也可以从类型化或非类型化创建<xref:System.Data.DataTable>。 在这两种情况下，您将创建<xref:System.Data.DataView>通过使用其中一个<xref:System.Data.DataTableExtensions.AsDataView%2A>; 的扩展方法<xref:System.Data.DataView>不是 LINQ 到数据集上下文中可直接构造。  
  
 创建 <xref:System.Data.DataView> 之后，您可以将其绑定到 Windows 窗体应用程序或 ASP.NET 应用程序中的 UI 控件上，或者更改筛选和排序设置。  
  
 <xref:System.Data.DataView> 构造一个索引，该索引可显著提高可使用该索引的操作（如筛选和排序）的性能。 创建 <xref:System.Data.DataView> 及修改任何排序或筛选信息时，均会生成 <xref:System.Data.DataView> 的索引。 创建 <xref:System.Data.DataView> 然后设置排序或筛选信息会使索引生成至少两次：一次是在创建 <xref:System.Data.DataView> 时，另一次是在修改任何排序或筛选属性时。  
  
 有关筛选和排序的详细信息<xref:System.Data.DataView>，请参阅[使用 DataView 进行筛选](../../../../docs/framework/data/adonet/filtering-with-dataview-linq-to-dataset.md)并[使用 DataView 进行排序](../../../../docs/framework/data/adonet/sorting-with-dataview-linq-to-dataset.md)。  
  
## <a name="creating-dataview-from-a-linq-to-dataset-query"></a>通过 LINQ to DataSet 查询创建 DataView  
 一个<xref:System.Data.DataView>可以为数据集查询，结果所在的投影的结果中的 LINQ 创建对象<xref:System.Data.DataRow>对象。 新创建的 <xref:System.Data.DataView> 会从创建它的查询继承筛选和排序信息。  
  
> [!NOTE]
>  在大多数情况下，用于筛选和排序的表达式不应有副作用且必须是确定的。 另外，表达式不应包含依赖于固定执行次数的任何逻辑，因为排序和筛选操作可能会执行任意次。  
  
 不支持通过返回匿名类型的查询或执行联接操作的查询创建 <xref:System.Data.DataView>。  
  
 在用于创建 <xref:System.Data.DataView> 的查询中仅支持以下查询运算符：  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.Cast%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.OrderBy%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.OrderByDescending%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.Select%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.ThenBy%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.ThenByDescending%2A>  
  
- <xref:System.Data.EnumerableRowCollectionExtensions.Where%2A>  
  
 请注意，当<xref:System.Data.DataView>从 LINQ to DataSet 查询创建<xref:System.Data.EnumerableRowCollectionExtensions.Select%2A>方法必须是在查询中调用的最后一个方法。 这一点在以下示例中，这会创建<xref:System.Data.DataView>按照欠款总额排序的联机订单的：  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQuery1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromquery1)]
 [!code-vb[DP DataView Samples#CreateLDVFromQuery1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromquery1)]  
  
 此外可以使用基于字符串的<xref:System.Data.DataView.RowFilter%2A>并<xref:System.Data.DataView.Sort%2A>来筛选和排序属性<xref:System.Data.DataView>创建从查询后。 请注意，这将清除继承自查询的排序和筛选信息。 下面的示例创建<xref:System.Data.DataView>从 LINQ to 通过开头的最后一个名称来筛选的数据集查询的。 将基于字符串的 <xref:System.Data.DataView.Sort%2A> 属性设置为先按姓氏升序排序，然后按名字降序排序：  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQueryStringSort](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromquerystringsort)]
 [!code-vb[DP DataView Samples#CreateLDVFromQueryStringSort](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromquerystringsort)]  
  
## <a name="creating-a-dataview-from-a-datatable"></a>从数据表创建 DataView  
 除了从 LINQ to DataSet 查询创建<xref:System.Data.DataView>可以从创建对象<xref:System.Data.DataTable>通过使用<xref:System.Data.DataTableExtensions.AsDataView%2A>方法。  
  
 下面的示例从 SalesOrderDetail 表创建 <xref:System.Data.DataView> 并将其设置为 <xref:System.Windows.Forms.BindingSource> 对象的数据源。 此对象充当 <xref:System.Windows.Forms.DataGridView> 控件的代理。  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromTable](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromtable)]
 [!code-vb[DP DataView Samples#CreateLDVFromTable](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromtable)]  
  
 在从 <xref:System.Data.DataView> 创建 <xref:System.Data.DataTable> 后，可以在其上设置筛选和排序。 下面的示例从 Contact 表创建 <xref:System.Data.DataView> 并将 <xref:System.Data.DataView.Sort%2A> 属性设置为先按姓氏升序排序，然后按名字降序排序：  
  
 [!code-csharp[DP DataView Samples#LDVStringSort](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvstringsort)]
 [!code-vb[DP DataView Samples#LDVStringSort](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvstringsort)]  
  
 不过，在通过查询创建 <xref:System.Data.DataView.RowFilter%2A> 之后，设置 <xref:System.Data.DataView.Sort%2A> 或 <xref:System.Data.DataView> 属性会带来性能降低，因为 <xref:System.Data.DataView> 将会构造一个索引来支持筛选和排序操作。 设置 <xref:System.Data.DataView.RowFilter%2A> 或 <xref:System.Data.DataView.Sort%2A> 属性会重新生成数据的索引，从而增加应用程序的系统开销并降低性能。 在可能的情况下，最好在第一次创建 <xref:System.Data.DataView> 时指定筛选和排序信息并避免之后对其进行修改。  
  
## <a name="see-also"></a>请参阅

- [数据绑定和 LINQ to DataSet](../../../../docs/framework/data/adonet/data-binding-and-linq-to-dataset.md)
- [使用 DataView 进行筛选](../../../../docs/framework/data/adonet/filtering-with-dataview-linq-to-dataset.md)
- [使用 DataView 进行排序](../../../../docs/framework/data/adonet/sorting-with-dataview-linq-to-dataset.md)
