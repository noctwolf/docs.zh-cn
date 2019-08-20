---
title: 正则表达式中的分组构造
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lookbehinds
- regular expressions, grouping constructs
- lookaheads
- .NET Framework regular expressions, grouping constructs
- constructs, grouping
- grouping constructs
ms.assetid: 0fc18634-f590-4062-8d5c-f0b71abe405b
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 63a3ee099d4256a4bc800f74615fca8eaec2a77f
ms.sourcegitcommit: ced0cccf15adfd492f8196cb739f01dde52c9252
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "67135683"
---
# <a name="grouping-constructs-in-regular-expressions"></a>正则表达式中的分组构造
分组构造描述了正则表达式的子表达式，用于捕获输入字符串的子字符串。 你可以使用分组构造来完成下列任务：  
  
- 匹配输入字符串中重复的子表达式。  
  
- 将限定符应用于拥有多个正则表达式语言元素的子表达式。 有关限定符的更多信息，请参见 [Quantifiers](../../../docs/standard/base-types/quantifiers-in-regular-expressions.md)。  
  
- 包括由 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法返回的字符串的子表达式。  
  
- 从 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性中检索各个子表达式，并分别从匹配的文本作为一个整体处理它们。  
  
 下表列出了 .NET 正则表达式引擎支持的分组构造，并指明它们是捕获构造，还是非捕获构造。  
  
|分组构造|捕获或非捕获|  
|------------------------|-------------------------------|  
|[匹配的子表达式](#matched_subexpression)|捕获|  
|[命名匹配的子表达式](#named_matched_subexpression)|捕获|  
|[平衡组定义](#balancing_group_definition)|捕获|  
|[非捕获组](#noncapturing_group)|非捕获|  
|[组选项](#group_options)|非捕获|  
|[零宽度正预测先行断言](#zerowidth_positive_lookahead_assertion)|非捕获|  
|[零宽度负预测先行断言](#zerowidth_negative_lookahead_assertion)|非捕获|  
|[零宽度正回顾后发断言](#zerowidth_positive_lookbehind_assertion)|非捕获|  
|[零宽度负回顾后发断言](#zerowidth_negative_lookbehind_assertion)|非捕获|  
|[非回溯子表达式](#nonbacktracking_subexpression)|非捕获|  
  
 有关组和正则表达式对象模型的信息，请参见 [分组构造和正则表达式对象](#Objects)。  
  
<a name="matched_subexpression"></a>   
## <a name="matched-subexpressions"></a>匹配的子表达式  
 以下分组构造捕获匹配的子表达式：  
  
 `(` *子表达式* `)`  
  
 其中 *子表达式* 为任何有效正则表达式模式。 使用括号的捕获按正则表达式中左括号的顺序从`1`开始从左到右自动编号。 捕获元素编号为`0`的捕获是由整个正则表达式模式匹配的文本。  
  
> [!NOTE]
>  默认情况下， `(`*子表达式*`)` 语言元素捕获匹配的子表达式。 但是，如果正则表达式模式匹配方法的 <xref:System.Text.RegularExpressions.RegexOptions> 参数包含 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 标志，或者如果 `n` 选项应用于此子表达式（参见本主题后面的 [组选项](#group_options) ），则不会捕获匹配的子表达式。  
  
 可以四种方法访问捕获的组：  
  
- 通过使用正则表达式中的反向引用构造。 使用语法 `\`*数字*在同一正则表达式中引用匹配的子表达式，其中 *数字* 是捕获的表达式的初始数字。  
  
- 通过使用正则表达式中的命名的反向引用构造。 使用语法 `\k<`*name*`>`在同一正则表达式中引用匹配的子表达式，其中 *name* 是捕获组的名称，或使用 `\k<`*数字*`>`在同一正则表达式中引用匹配的子表达式，其中 *数字* 是捕获组的初始数字。 捕获组具有与其原始编号相同的默认名称。 有关更多信息，请参见本主题后面的 [命名匹配的子表达式](#named_matched_subexpression) 。  
  
- 通过使用 `$`*数字* $ <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> number <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 替换序列，其中 *数字* 是捕获的表达式的初始数字。  
  
- 以编程的方式，通过使用 <xref:System.Text.RegularExpressions.GroupCollection> 对象的方式，该对象由 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回。 集合中位置零上的成员表示正则表达式匹配。 每个后续成员表示匹配的子表达式。 有关更多信息，请参见 [Grouping Constructs and Regular Expression Objects](#Objects) 一节。  
  
 以下示例阐释表示文本中重复单词的正则表达式。 正则表达式模式的两个捕获组表示重复的单词的两个实例。 捕获第二个实例，以报告它在输入字符串的起始位置。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping1.cs#1)]
 [!code-vb[RegularExpressions.Language.Grouping#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping1.vb#1)]  
  
 正则表达式模式为：  
  
```  
(\w+)\s(\1)\W  
```  
  
 下表演示了如何解释正则表达式模式。  
  
|模式|说明|  
|-------------|-----------------|  
|`(\w+)`|匹配一个或多个单词字符。 这是第一个捕获组。|  
|`\s`|与空白字符匹配。|  
|`(\1)`|与第一个捕获组捕获中的字符串匹配。 这是第二个捕获组。 该示例将其指定到捕获组上，以便可从 `Match.Index` 属性返回。|  
|`\W`|匹配包括空格和标点符号的一个非单词字符。 这样可以防止正则表达式模式匹配从第一个捕获组的单词开头的单词。|  
  
<a name="named_matched_subexpression"></a>   
## <a name="named-matched-subexpressions"></a>命名匹配的子表达式  
 以下分组构造捕获匹配的子表达式，并允许你按名称或编号访问它：  
  
```  
(?<name>subexpression)  
```  
  
 或：  
  
```  
(?'name'subexpression)  
```  
  
 其中 *名称* 是有效的组名称，而 *子表达式* 是任何有效的正则表达式模式。 *名称* 不得包含任何标点符号字符，并且不能以数字开头。  
  
> [!NOTE]
>  如果正则表达式模式匹配方法的 <xref:System.Text.RegularExpressions.RegexOptions> 参数包含 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 标志，或者如果 `n` 选项应用于此子表达式（参见本主题后面的 [组选项](#group_options) ），则捕获子表达式的唯一方法就是显式命名捕获组。  
  
 可用以下方式访问已命名的捕获组：  
  
- 通过使用正则表达式中的命名的反向引用构造。 使用语法 `\k<`*name*`>`在同一正则表达式中引用匹配的子表达式，其中 *name* 是捕获子表达式的名称。  
  
- 通过使用正则表达式中的反向引用构造。 使用语法 `\`*数字*在同一正则表达式中引用匹配的子表达式，其中 *数字* 是捕获的表达式的初始数字。 已命名的匹配子表达式在匹配子表达式后从左到右连续编号。  
  
- 通过使用 `${`*name*`}` $ <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> number <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 替换序列，其中 *name* 是捕获子表达式的名称。  
  
- 通过在 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> 或 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法调用中使用 `$`数字  替换序列，其中“数字”  为捕获的子表达式的序号。  
  
- 以编程的方式，通过使用 <xref:System.Text.RegularExpressions.GroupCollection> 对象的方式，该对象由 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回。 集合中位置零上的成员表示正则表达式匹配。 每个后续成员表示匹配的子表达式。 已命名的捕获组在集合中存储在已编号的捕获组后面。  
  
- 以编程方式，通过将子表达式名称提供至 <xref:System.Text.RegularExpressions.GroupCollection> 对象的索引器（在 C# 中），或者提供至其 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> 属性（在 Visual Basic 中）。  
  
 简单的正则表达式模式会阐释如何编号（未命名），并且可以以编程方式或通过正则表达式语言语法引用已命名的组。 正则表达式 `((?<One>abc)\d+)?(?<Two>xyz)(.*)` 按编号和名称产生下列捕获组。 编号为 0 的第一个捕获组总是指整个模式。  
  
|数字|name|模式|  
|------------|----------|-------------|  
|0|0（默认名称）|`((?<One>abc)\d+)?(?<Two>xyz)(.*)`|  
|1|1（默认名称）|`((?<One>abc)\d+)`|  
|2|2（默认名称）|`(.*)`|  
|3|一|`(?<One>abc)`|  
|4|二|`(?<Two>xyz)`|  
  
 下面的示例阐释了一个正则表达式，标识出重复的单词和紧随每个重复的单词的单词。 正则表达式模式定义了两个命名的子表达式： `duplicateWord`，它表示重复的单词；和 `nextWord`，它表示后面跟随重复单词的单词。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping2.cs#2)]
 [!code-vb[RegularExpressions.Language.Grouping#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping2.vb#2)]  
  
 正则表达式模式按如下方式定义：  
  
```  
(?<duplicateWord>\w+)\s\k<duplicateWord>\W(?<nextWord>\w+)  
```  
  
 下表演示了正则表达式的含义。  
  
|模式|说明|  
|-------------|-----------------|  
|`(?<duplicateWord>\w+)`|匹配一个或多个单词字符。 命名此捕获组 `duplicateWord`。|  
|`\s`|与空白字符匹配。|  
|`\k<duplicateWord>`|匹配名为 `duplicateWord`的捕获的组。|  
|`\W`|匹配包括空格和标点符号的一个非单词字符。 这样可以防止正则表达式模式匹配从第一个捕获组的单词开头的单词。|  
|`(?<nextWord>\w+)`|匹配一个或多个单词字符。 命名此捕获组 `nextWord`。|  
  
 请注意可在正则表达式中重复组名。 例如，可将多个组命名为 `digit`，如下面的示例所示。 在名称重复的情况下， <xref:System.Text.RegularExpressions.Group> 对象的值由输入字符串中最后一个成功的捕获确定。 此外，如果组名不重复，则使用有关每个捕获的信息填充 <xref:System.Text.RegularExpressions.CaptureCollection> 。  
  
 在下面的示例中，正则表达式 `\D+(?<digit>\d+)\D+(?<digit>\d+)?` 中两次出现了名为 `digit`的组。 第一个名为 `digit` 的组捕获一个或多个数字字符。 第二个名为 `digit` 的组捕获一个或多个数字字符的零个或一个匹配项。 如示例的输出所示，如果第二个捕获组成功匹配文本，则文本的值定义 <xref:System.Text.RegularExpressions.Group> 对象的值。 如果第二个捕获组无法不匹配输入字符串，则最后一个成功匹配的值定义 <xref:System.Text.RegularExpressions.Group> 对象的值。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#12](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/duplicate1.cs#12)]
 [!code-vb[RegularExpressions.Language.Grouping#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/duplicate1.vb#12)]  
  
 下表演示了正则表达式的含义。  
  
|模式|说明|  
|-------------|-----------------|  
|`\D+`|匹配一个或多个非十进制数字字符。|  
|`(?<digit>\d+)`|匹配一个或多个十进制数字字符。 将匹配分配到 `digit` 命名组。|  
|`\D+`|匹配一个或多个非十进制数字字符。|  
|`(?<digit>\d+)?`|匹配一个或多个十进制数字字符的零个或一个匹配项。 将匹配分配到 `digit` 命名组。|  
  
<a name="balancing_group_definition"></a>   
## <a name="balancing-group-definitions"></a>平衡组定义  
 平衡组定义将删除以前定义的组和存储的定义，并在当前组中存储以前定义的组和当前组之间的间隔。 此分组构造具有以下格式：  
  
```  
(?<name1-name2>subexpression)  
```  
  
 或：  
  
```  
(?'name1-name2' subexpression)  
```  
  
 *name1* 位置是当前的组（可选）， *name2* 是一个以前定义的组，而 *子表达式* 是任何有效的正则表达式模式。 平衡组定义删除 *name2* 的定义并在 *name1* 中保存 *name2* 和 *name1*之间的间隔。 如果未定义 *name2* 组，则匹配将回溯。 由于删除 *name2* 的最后一个定义会显示 *name2*以前的定义，因此该构造允许将 *name2* 组的捕获堆栈用作计数器，用于跟踪嵌套构造（如括号或者左括号和右括号）。  
  
 平衡组定义将 *name2* 作为堆栈使用。 将每个嵌套构造的开头字符放在组中，并放在其 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 集合中。 当匹配结束字符时，从组中删除其相应的开始字符，并且 <xref:System.Text.RegularExpressions.Group.Captures%2A> 集合减少 1。 所有嵌套构造的开始和结束字符匹配完后，name2  为空。  
  
> [!NOTE]
>  通过修改下面示例中的正则表达式来使用合适的嵌套构造的开始和结束字符后，你可以用它来处理多数嵌套构造，如数学表达式或包括多个嵌套方法调用的程序代码行。  
  
 下面的示例使用平衡组定义匹配输入字符串中的左右尖括号 (<>)。 该示例定义两个已命名的组， `Open` 和 `Close`，用作堆栈来跟踪配对的尖括号。 将每个已捕获的左尖括号推入到 `Open` 组的捕获集合，而将每个已捕获的右尖括号推入到 `Close` 组的捕获集合。 平衡组定义确保每个左尖括号都有一个匹配的右尖角括号。 如果没有，则仅会在 `(?(Open)(?!))`组不为空的情况下计算最终子模式 `Open` 的值（因此，如果所有嵌套构造尚未关闭）。 如果计算了最终子模式的值，则匹配将失败，因为 `(?!)` 子模式是始终失败的零宽度负预测先行断言。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping3.cs#3)]
 [!code-vb[RegularExpressions.Language.Grouping#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping3.vb#3)]  
  
 正则表达式模式为：  
  
```  
^[^<>]*(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*(?(Open)(?!))$  
```  
  
 正则表达式按如下方式解释：  
  
|模式|说明|  
|-------------|-----------------|  
|`^`|从字符串的开头部分开始。|  
|`[^<>]*`|匹配零个或多个不是左侧或右侧角度方括号的字符。|  
|`(?'Open'<)`|匹配左尖括号并分配给名为 `Open`的组。|  
|`[^<>]*`|匹配零个或多个不是左侧或右侧角度方括号的字符。|  
|`((?'Open'<)[^<>]*) +`|匹配跟在非左尖括号或非右尖括号的零个或多个字符后面的一个或多个左尖括号匹配项。 这是第二个捕获组。|  
|`(?'Close-Open'>)`|匹配右尖括号，将 `Open` 组和当前组分配给 `Close` 组并删除 `Open` 组的定义。|  
|`[^<>]*`|匹配非左尖括号或非右尖括号的任何字符的零个或多个匹配项。|  
|`((?'Close-Open'>)[^<>]*)+`|匹配跟在零后面或跟在非左尖括号或非右尖括号的多个字符后面的一个或多个右尖括号匹配项。 在匹配右尖括号时，将 `Open` 组和当前组分配给 `Close` 组并删除 `Open` 组的定义。 这是第三个捕获组。|  
|`(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*`|匹配零个或多个下列模式的匹配项：一个或多个左尖括号匹配项，后跟零个或多个非尖括号字符，后跟一个或多个右尖括号的匹配项，后跟零个或多个非尖括号的匹配项。 在匹配右尖括号时，删除 `Open` 组的定义，并将 `Open` 组和当前组之间的子字符串分配给 `Close` 组。 这是第一个捕获组。|  
|`(?(Open)(?!))`|如果 `Open` 组存在，并可以匹配空字符串，则放弃匹配，但不前移字符串中的正则表达式引擎的位置。 这是零宽度负预测先行断言。 因为空字符串总是隐式地存在于输入字符串中，所以此匹配始终失败。 此匹配的失败表示尖括号不平衡。|  
|`$`|匹配输入字符串的末尾部分。|  
  
 最终子表达式 `(?(Open)(?!))`，指示是否正确平衡输入字符串中的嵌套构造（例如，是否每个左尖括号由右键括号匹配）。 它使用基于有效的捕获组的条件匹配，有关详细信息请参阅 [Alternation Constructs](../../../docs/standard/base-types/alternation-constructs-in-regular-expressions.md)。 如果定义了 `Open` 组，则正则表达式引擎会尝试匹配输入字符串中的子表达式 `(?!)` 。 仅当嵌套构造不均衡时，才应该定义 `Open` 组。 因此，要在输入字符串中匹配的模式应该是一个始终导致匹配失败的模式。 在此情况下， `(?!)` 是始终失败的零宽度负预测先行断言，因为空字符串总是隐式地存在于输入字符串中的下一个位置。  
  
 在此示例中，正则表达式引擎计算输入字符串“\<abc><mno\<xyz>>”，如下表所示。  
  
|步骤|模式|结果|  
|----------|-------------|------------|  
|1|`^`|从输入字符串的开头部分开始匹配。|  
|2|`[^<>]*`|查找左尖括号之前的非尖括号字符；未找到匹配项。|  
|3|`(((?'Open'<)`|匹配“\<abc>”中的左尖括号，并将它分配给 `Open` 组。|  
|4|`[^<>]*`|与“abc”匹配。|  
|5|`)+`|“<abc”是第二个捕获组的值。<br /><br /> 输入字符串中的下一个字符不是左尖括号，因此正则表达式引擎不会循环回到 `(?'Open'<)[^<>]*)` 子模式。|  
|6|`((?'Close-Open'>)`|匹配“\<abc>”中的右尖括号，将“abc”（`Open` 组和右尖括号之间的子字符串）分配给 `Close` 组，并删除 `Open` 组的当前值（“<”），同时保持它为空。|  
|7|`[^<>]*`|查找右尖括号之后的非尖括号字符；未找到匹配项。|  
|8|`)+`|第三个捕获组的值是“>”。<br /><br /> 输入字符串中的下一个字符不是右尖括号，因此正则表达式引擎不会循环回到 `((?'Close-Open'>)[^<>]*)` 子模式。|  
|9|`)*`|第一个捕获组的值是“\<abc>”。<br /><br /> 输入字符串中的下一个字符是左尖括号，因此正则表达式引擎会循环回到 `(((?'Open'<)` 子模式。|  
|10|`(((?'Open'<)`|匹配“\<mno>”中的左尖括号，并将它分配给 `Open` 组。 其 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 集合现在具有单个值“<”。|  
|11|`[^<>]*`|与“mno”匹配。|  
|12|`)+`|“<mno”是第二个捕获组的值。<br /><br /> 输入字符串中的下一个字符是左尖括号，因此正则表达式引擎会循环回到 `(?'Open'<)[^<>]*)` 子模式。|  
|13|`(((?'Open'<)`|匹配“\<xyz>”中的左尖括号，并将它分配给 `Open` 组。 `Open` 组的 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 集合现在包括两个捕获：“\<mno>”中的左尖括号和“\<xyz>”中的左尖括号。|  
|14|`[^<>]*`|与“xyz”匹配。|  
|15|`)+`|“<xyz”是第二个捕获组的值。<br /><br /> 输入字符串中的下一个字符不是左尖括号，因此正则表达式引擎不会循环回到 `(?'Open'<)[^<>]*)` 子模式。|  
|16|`((?'Close-Open'>)`|匹配“\<xyz>”中的右尖括号。 “xyz”将 `Open` 组合右尖括号之间的子字符串分配给 `Close` 组，并删除 `Open` 组的当前值。 前一个捕获的值（“\<mno>”中的左尖括号）成为 `Open` 组的当前值。 `Open` 组的 <xref:System.Text.RegularExpressions.Group.Captures%2A> 集合现在包括一个捕获，即“\<xyz>”中的左尖括号。|  
|17|`[^<>]*`|查找非尖括号字符；未找到匹配项。|  
|18|`)+`|第三个捕获组的值是“>”。<br /><br /> 输入字符串中的下一个字符是右尖括号，因此正则表达式引擎会循环回到 `((?'Close-Open'>)[^<>]*)` 子模式。|  
|19|`((?'Close-Open'>)`|匹配“xyz>>”中的最后一个右尖括号，将“mno\<xyz>”（`Open` 组和右尖括号之间的子字符串）分配给 `Close` 组，并删除 `Open` 组的当前值。 `Open` 组现在为空。|  
|20|`[^<>]*`|查找非尖括号字符；未找到匹配项。|  
|21|`)+`|第三个捕获组的值是“>”。<br /><br /> 输入字符串中的下一个字符不是右尖括号，因此正则表达式引擎不会循环回到 `((?'Close-Open'>)[^<>]*)` 子模式。|  
|22|`)*`|第一个捕获组的值是“<mno\<xyz>>”。<br /><br /> 输入字符串中的下一个字符不是左尖括号，因此正则表达式引擎不会循环回到 `(((?'Open'<)` 子模式。|  
|23|`(?(Open)(?!))`|`Open` 组是未定义的，因此没有尝试匹配。|  
|24|`$`|匹配输入字符串的末尾部分。|  
  
<a name="noncapturing_group"></a>   
## <a name="noncapturing-groups"></a>非捕获组  
 以下分组构造不会捕获由子表达式匹配的子字符串：  
  
```  
(?:subexpression)  
```  
  
 其中 *子表达式* 为任何有效正则表达式模式。 当一个限定符应用到一个组，但组捕获的子字符串并非所需时，通常会使用非捕获组构造。  
  
> [!NOTE]
>  如果正则表达式包含嵌套的分组构造，则外部非捕获组构造不适用于内部嵌套组构造。  
  
 下面的示例阐释包括非捕获组的正则表达式。 请注意输出不包含任何已捕获的组。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/noncapture1.cs#5)]
 [!code-vb[RegularExpressions.Language.Grouping#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/noncapture1.vb#5)]  
  
 正则表达式 `(?:\b(?:\w+)\W*)+\.` 匹配由句号终止的语句。 因为正则表达式重点介绍句子，而不是个别单词，所以分组构造以独占方式用作限定符。 正则表达式模式可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(?:\w+)`|匹配一个或多个单词字符。 不将匹配的文本分配给捕获的组。|  
|`\W*`|匹配零个或多个非单词字符。|  
|`(?:\b(?:\w+)\W*)+`|一次或多次匹配跟在零个或多个非单词字符后面以单词边界开头的一个或多个单词字符的模式。 不将匹配的文本分配给捕获的组。|  
|`\.`|匹配句点。|  
  
<a name="group_options"></a>   
## <a name="group-options"></a>组选项  
 以下分组构造应用或禁用子表达式中指定的选项：  
  
 `(?imnsx-imnsx:` *子表达式* `)`  
  
 其中 *子表达式* 为任何有效正则表达式模式。 例如， `(?i-s:)` 将打开不区分大小写并禁用单行模式。 有关可以指定的内联选项的更多信息，请参见 [正则表达式选项](../../../docs/standard/base-types/regular-expression-options.md)。  
  
> [!NOTE]
>  可以指定将应用于整个正则表达式，而不是子表达式的选项，方法是使用 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> 类构造函数或静态方法。 也可指定在正则表达式特定点后使用的内联选项，方法是使用 `(?imnsx-imnsx)` 语言构造。  
  
 组的选项构造并非捕获组。 即尽管 *子表达式* 捕获的字符串的任意部分包含在匹配中，但不会包含在捕获的组中也不会用于填充 <xref:System.Text.RegularExpressions.GroupCollection> 对象。  
  
 例如，以下示例中的正则表达式 `\b(?ix: d \w+)\s` 使用分组构造中的内联选项，以启用不区分大小写的匹配和在识别所有以字母“d”开头的单词时忽略模式空白。 该正则表达式的定义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(?ix: d \w+)`|使用不区分大小写的匹配并忽略此模式中的白色空间，匹配后跟一个或多个单词字符的“d”。|  
|`\s`|与空白字符匹配。|  
  
 [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
 [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]  
  
<a name="zerowidth_positive_lookahead_assertion"></a>   
## <a name="zero-width-positive-lookahead-assertions"></a>零宽度正预测先行断言  
 以下分组构造定义零宽度正预测先行断言：  
  
 `(?=` *子表达式* `)`  
  
 其中 *子表达式* 为任何正则表达式模式。 若要成功匹配，则输入字符串必须匹配 *子表达式*中的正则表达式模式，尽管匹配的子字符串未包含在匹配结果中。 零宽度正预测先行断言不会回溯。  
  
 通常，零宽度正预测先行断言是在正则表达式模式的末尾找到的。 它定义了一个子字符串，该子字符串必须出现在匹配字符串的末尾但又不能包含在匹配结果中。 还有助于防止过度回溯。 可使用零宽度正预测先行断言来确保特定捕获组以与专为该捕获组定义的模式的子集相匹配的文本开始。 例如，如果捕获组与连续单词字符相匹配，可以使用零宽度正预测先行断言要求第一个字符是按字母顺序排列的大写字符。  
  
 下面的示例使用零宽度正预测先行断言，以匹配输入字符串中谓词“is”前的单词。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#6](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookahead1.cs#6)]
 [!code-vb[RegularExpressions.Language.Grouping#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookahead1.vb#6)]  
  
 正则表达式 `\b\w+(?=\sis\b)` 可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\w+`|匹配一个或多个单词字符。|  
|`(?=\sis\b)`|确定单词字符是否后接空白字符和字符串“is”，其在单词边界处结束。 如果如此，则匹配成功。|  
  
<a name="zerowidth_negative_lookahead_assertion"></a>   
## <a name="zero-width-negative-lookahead-assertions"></a>零宽度负预测先行断言  
 以下分组构造定义零宽度负预测先行断言：  
  
 `(?!` *子表达式* `)`  
  
 其中 *子表达式* 为任何正则表达式模式。 若要成功匹配，则输入字符串不得匹配 *子表达式*中的正则表达式模式，尽管匹配的子字符串未包含在匹配结果中。  
  
 零宽度负预测先行断言通常用在正则表达式的开头或结尾。 正则表达式的开头可以定义当其定义了要被匹配的相似但更常规的模式时，不应被匹配的特定模式。 在这种情况下，它通常用于限制回溯。 正则表达式的末尾可以定义不能出现在匹配项末尾处的子表达式。  
  
 下面的示例定义了正则表达式匹配，其在正则表达式的开头使用零宽度预测先行断言，以匹配未以“un”开头的单词。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#7](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead1.cs#7)]
 [!code-vb[RegularExpressions.Language.Grouping#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead1.vb#7)]  
  
 正则表达式 `\b(?!un)\w+\b` 可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(?!un)`|确定接下来的两个的字符是否为“un”。 如果没有，则可能匹配。|  
|`\w+`|匹配一个或多个单词字符。|  
|`\b`|在单词边界处结束匹配。|  
  
 下面的示例定义了正则表达式匹配，其在正则表达式的末尾使用零宽度预测先行断言，以匹配未以标点字符结束的单词。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#8](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead2.cs#8)]
 [!code-vb[RegularExpressions.Language.Grouping#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead2.vb#8)]  
  
 正则表达式 `\b\w+\b(?!\p{P})` 可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\w+`|匹配一个或多个单词字符。|  
|`\b`|在单词边界处结束匹配。|  
|`\p{P})`|如果下个字符不是一个标点符号（如句点或逗号），则匹配成功。|  
  
<a name="zerowidth_positive_lookbehind_assertion"></a>   
## <a name="zero-width-positive-lookbehind-assertions"></a>零宽度正回顾后发断言  
 以下分组构造定义零宽度正回顾后发断言：  
  
 `(?<=` *子表达式* `)`  
  
 其中 *子表达式* 为任何正则表达式模式。 若要成功匹配，则 *子表达式* 必须在输入字符串当前位置左侧出现，尽管 `subexpression` 未包含在匹配结果中。 零宽度正回顾后发断言不会回溯。  
  
 零宽度正预测后发断言通常在正则表达式的开头使用。 它们定义的模式是一个匹配的前提条件，但它不是匹配结果的一部分。  
  
 例如，下面的示例匹配二十一世纪年份的最后两个数字（也就是说，数字“20”要在匹配的字符串之前）。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#9](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookbehind1.cs#9)]
 [!code-vb[RegularExpressions.Language.Grouping#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookbehind1.vb#9)]  
  
 正则表达式模式 `(?<=\b20)\d{2}\b` 的含义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`\d{2}`|匹配两个十进制数字。|  
|`(?<=\b20)`|如果两个十进制数字的字边界以小数位数“20”开头，则继续匹配。|  
|`\b`|在单词边界处结束匹配。|  
  
 零宽度正回顾后发断言还用于在捕获组中的最后一个或多个字符不得为与该捕获组的正则表达式模式相匹配的字符的子集时限制回溯。 例如，如果组捕获所有的连续单词字符，可以使用零宽度正回顾后发断言要求最后一个字符时按字母顺序的。  
  
<a name="zerowidth_negative_lookbehind_assertion"></a>   
## <a name="zero-width-negative-lookbehind-assertions"></a>零宽度负回顾后发断言  
 以下组构造定义零宽度负回顾后发断言：  
  
 `(?<!` *子表达式* `)`  
  
 其中 *子表达式* 为任何正则表达式模式。 若要成功匹配，则 *子表达式* 不得在输入字符串当前位置的左侧出现。 但是，任何不匹配 `subexpression` 的子字符串不包含在匹配结果中。  
  
 零宽度负回顾后发断言通常在正则表达式的开头使用。 它们定义的模式预先排除在后面的字符串中的匹配项。 它们还用于在捕获组中的最后一个或多个字符不得为与该捕获组的正则表达式模式相匹配的其中一个或多个字符时限制回溯。 例如，如果如果组捕获了所有的连续单词字符，可以使用零宽度正回顾后发断言要求最后一个字符不是下划线 (_)。  
  
 下面的示例匹配除周末之外的一周的任何一天（也就是星期六和星期日都没有）。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#10](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookbehind1.cs#10)]
 [!code-vb[RegularExpressions.Language.Grouping#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookbehind1.vb#10)]  
  
 正则表达式模式 `(?<!(Saturday|Sunday) )\b\w+ \d{1,2}, \d{4}\b` 的含义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\w+`|匹配一个或多个后跟空白字符的单词字符。|  
|`\d{1,2},`|匹配空白字符和逗号后面的一个或两个十进制数字。|  
|`\d{4}\b`|匹配四个十进制数字并在单词边界处结束匹配。|  
|<code>(?<!(Saturday&#124;Sunday) )</code>|如果匹配以字符串“星期六”或者“星期日”开头，后跟一个空格，则匹配成功。|  
  
<a name="nonbacktracking_subexpression"></a>   
## <a name="nonbacktracking-subexpressions"></a>非回溯子表达式  
 以下分组构造表示非回溯子表达式（也称为一个“贪婪”子表达式）：  
  
 `(?>` *子表达式* `)`  
  
 其中 *子表达式* 为任何正则表达式模式。  
  
 通常，如果正则表达式包含一个可选或可替代匹配模式并且备选不成功的话，正则表达式引擎可以在多个方向上分支以将输入的字符串与某种模式进行匹配。 如果未找到使用第一个分支的匹配项，则正则表达式引擎可以备份或回溯到使用第一个匹配项的点并尝试使用第二个分支的匹配项。 此过程可继续进行，直到尝试所有分支。  
  
 仅当嵌套构造不均衡时，才应该定义 `(?>`*子表达式*`)` 语言构造禁用回溯。 正则表达式引擎将在输入字符串中匹配尽可能多的字符。 在没有任何进一步匹配可用时，它将不回溯以尝试备用模式匹配。 （也就是说，该子表达式仅与可由该子表达式单独匹配的字符串匹配；子表达式不会尝试与基于该子表达式的字符串和任何该子表达式之后的子表达式匹配。）  
  
 如果你知道回溯不会成功，则建议使用此选项。 防止正则表达式引擎执行不需要的搜索可以提高性能。  
  
 下面的示例阐释非回溯子表达式如何修改模式匹配的结果。 回溯正则表达式成功匹配一系列重复字符，在字边界上其后为相同字符，但非回溯正则表达式不会匹配。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#11](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/nonbacktracking1.cs#11)]
 [!code-vb[RegularExpressions.Language.Grouping#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/nonbacktracking1.vb#11)]  
  
 非回溯正则表达式 `(?>(\w)\1+).\b` 的定义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`(\w)`|匹配单个单词字符，并将其分配给第一捕获组。|  
|`\1+`|一次或多次匹配的第一个捕获子字符串的值。|  
|`.`|匹配任意字符。|  
|`\b`|在单词边界处结束匹配。|  
|`(?>(\w)\1+)`|匹配一个重复的单词字符的一个或多个匹配项，但不执行回溯以匹配在单词边界上的最后一个字符。|  
  
<a name="Objects"></a>   
## <a name="grouping-constructs-and-regular-expression-objects"></a>分组构造和正则表达式对象  
 由正则表达式捕获组匹配的子字符串由 <xref:System.Text.RegularExpressions.Group?displayProperty=nameWithType> 对象表示，其从 <xref:System.Text.RegularExpressions.GroupCollection?displayProperty=nameWithType> 对象检索，其由 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回。 填充 <xref:System.Text.RegularExpressions.GroupCollection> 对象，如下所示：  
  
- 集合中的第一个 <xref:System.Text.RegularExpressions.Group> 对象（位于索引零的对象）表示整个匹配。  
  
- 下一组 <xref:System.Text.RegularExpressions.Group> 对象表示未命名（编号）的捕获组。 它们以在正则表达式中定义的顺序出现，从左至右。 这些组的索引值范围从 1 到集合中未命名捕获组的数目。 （特定组索引等效于其带编号的反向引用。 有关向后引用的更多信息，请参见 [Backreference Constructs](../../../docs/standard/base-types/backreference-constructs-in-regular-expressions.md)。）  
  
- 最后的 <xref:System.Text.RegularExpressions.Group> 对象组表示命名的捕获组。 它们以在正则表达式中定义的顺序出现，从左至右。 第一个名为捕获组的索引值是一个大于最后一个未命名的捕获组的索引。 如果正则表达式中没有未命名捕获组，则第一个命名的捕获组的索引值为 1。  
  
 如果将限定符应用于捕获组，则对应的 <xref:System.Text.RegularExpressions.Group> 对象的 <xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType>、 <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType>和 <xref:System.Text.RegularExpressions.Capture.Length%2A?displayProperty=nameWithType> 属性反映捕获组捕获的最后一个子字符串。 可以检索一整组子字符串，其是按组捕获的并具有来自 <xref:System.Text.RegularExpressions.CaptureCollection> 对象的限定符，其由 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 属性返回。  
  
 下面的示例阐释 <xref:System.Text.RegularExpressions.Group> 和 <xref:System.Text.RegularExpressions.Capture> 对象之间的关系。  
  
 [!code-csharp[RegularExpressions.Language.Grouping#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/objectmodel1.cs#4)]
 [!code-vb[RegularExpressions.Language.Grouping#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/objectmodel1.vb#4)]  
  
 正则表达式模式 `\b(\w+)\W+)+` 从字符串提取各个单词。 其定义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(\w+)`|匹配一个或多个单词字符。 这些字符一起构成一个单词。 这是第二个捕获组。|  
|`\W+`|匹配一个或多个非单词字符。|  
|`(\w+)\W+)+`|一次或多次匹配跟在一个或多个非单词字符后面的一个或多个单词字符的模式。 这是第一个捕获组。|  
  
 第一个捕获组匹配句子的每个单词。 第二个捕获组匹配每个单词，连同标点符号和该单词后的空白区域。 <xref:System.Text.RegularExpressions.Group> 对象的索引是 2，提供了有关由第二个捕获组匹配的文本的信息。 可从 <xref:System.Text.RegularExpressions.CaptureCollection> 对象获取捕获组捕获的整组单词，该对象由 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 属性返回。  
  
## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [回溯](../../../docs/standard/base-types/backtracking-in-regular-expressions.md)
