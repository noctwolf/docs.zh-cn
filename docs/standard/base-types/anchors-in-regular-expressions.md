---
title: .NET 正则表达式中的定位点
description: 了解如何在正则表达式模式中使用定位点。
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- atomic zero-width assertions
- regular expressions, anchors
- regular expressions, atomic zero-width assertions
- anchors, in regular expressions
- metacharacters, atomic zero-width assertions
- metacharacters, anchors
- .NET Framework regular expressions, anchors
- .NET Framework regular expressions, atomic zero-width assertions
ms.assetid: 336391f6-2614-499b-8b1b-07a6837108a7
author: rpetrusha
ms.author: ronpet
ms.custom: seodec18
ms.openlocfilehash: 85e971d3464b6f2a073288e7f8b5406563598f79
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64634759"
---
# <a name="anchors-in-regular-expressions"></a>正则表达式中的定位点
<a name="top"></a> 定位点（原子零宽度断言）指定字符串中必须出现匹配的位置。 在搜索表达式中使用定位点时，正则表达式引擎不在字符串中前进或使用字符，它仅在指定位置查找匹配。 例如， `^` 指定必须从行或字符串的开头开始匹配。 因此，正则表达式 `^http:` 仅当 "http:" 出现在行开头时才与之匹配。 下表列出了 .NET 中正则表达式支持的定位点。  
  
|定位点|说明|  
|------------|-----------------|  
|`^`|默认情况下，匹配必须出现在字符串的开头；在多行模式中，必须出现在该行的开头。 有关详细信息，请参阅 [字符串或行的开头](#Start)。|  
|`$`|默认情况下，匹配必须出现在字符串的末尾，或在字符串末尾的 `\n` 之前；在多行模式中，必须出现在该行的末尾，或在该行末尾的 `\n` 之前。 有关详细信息，请参阅 [字符串或行的末尾](#End)。|  
|`\A`|匹配必须仅出现在字符串的开头位置（无多行支持）。 有关详细信息，请参阅 [仅字符串的开头](#StartOnly)。|  
|`\Z`|匹配必须出现在字符串的末尾，或出现在字符串末尾的 `\n` 之前。 有关详细信息，请参阅 [字符串的末尾或结束换行之前](#EndOrNOnly)。|  
|`\z`|匹配必须仅出现在字符串的末尾。 有关详细信息，请参阅 [仅字符串的末尾](#EndOnly)。|  
|`\G`|匹配必须从上一个匹配结束的位置开始。 有关详细信息，请参阅 [连续匹配](#Contiguous)。|  
|`\b`|匹配必须出现在字边界。 有关详细信息，请参阅 [字边界](#WordBoundary)。|  
|`\B`|匹配不得出现在字边界上。 有关详细信息，请参阅 [非字边界](#NonwordBoundary)。|  
  
<a name="Start"></a>   
## <a name="start-of-string-or-line-"></a>字符串或行的开头：^  
 默认情况下，`^` 定位点指定以下模式必须从字符串的第一个字符位置开始。 如果结合使用 `^` 与 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项（请参阅[正则表达式选项](../../../docs/standard/base-types/regular-expression-options.md)），匹配必须出现在每行的开头。  
  
 以下示例在正则表达式中使用 `^` 定位点，可提取有关某些职业棒球队存在年限的信息。 该示例调用 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 方法的两个重载：  
  
- 调用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29> 重载仅找到输入字符串中与正则表达式模式匹配的第一个子字符串。  
  
- 调用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29> 重载，并将 `options` 参数设置为 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 可找到所有五个子字符串。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring1.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring1.vb#1)]  
  
 正则表达式模式 `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+` 的定义如下表所示。  
  
|模式|说明|  
|-------------|-----------------|  
|`^`|从输入字符串的开头开始匹配（如果在调用该方法时选择了 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项，则从行的开头开始匹配）。|  
|`((\w+(\s?)){2,}`|匹配至少两次后跟零个或一个空格的一个或多个单词字符。 这是第一个捕获组。 此表达式还定义第二个和第三个捕获组：第二个捕获组由捕获的单词组成，第三个捕获组由捕获的空白组成。|  
|`,\s`|匹配后跟一个空白字符的逗号。|  
|`(\w+\s\w+)`|匹配后跟一个空格再后跟一个或多个单词字符的一个或多个单词字符。 这是第四个捕获组。|  
|`,`|匹配逗号。|  
|`\s\d{4}`|匹配后跟四个十进制数字的空格。|  
|<code>(-(\d{4}&#124;present))?</code>|匹配连字符后跟四个十进制数字或字符串“present”的零或一个匹配项。 这是第六个捕获组。 还包括第七个捕获组。|  
|`,?`|匹配逗号的零个或一个匹配项。|  
|<code>(\s\d{4}(-(\d{4}&#124;present))?,?)+</code>|匹配以下内容的一个或多个匹配项：空格、四个十进制数字、连字符后跟四个十进制数字或字符串“present”的零个或一个匹配项以及零个或一个逗号。 这是第五个捕获组。|  
  
 [返回页首](#top)  
  
<a name="End"></a>   
## <a name="end-of-string-or-line-"></a>字符串或行的末尾：$  
 `$` 定位点指定前面的模式必须出现在输入字符串的末尾，或出现在输入字符串末尾的 `\n` 之前。  
  
 如果结合使用 `$` 与 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项，则匹配也可能出现在行的末尾。 请注意， `$` 与 `\n` 匹配但与 `\r\n` （回车符和换行符的组合，或称 CR/LF）不匹配。 若要匹配 CR/LF 字符组合，请将 `\r?$` 包括到正则表达式模式中。  
  
 以下示例将 `$` 定位点添加到 [字符串或行的开头](#Start) 部分的示例中所使用的正则表达式模式中。 配合包括五行文本的原始输入字符串使用时， <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> 方法找不到匹配项，因为第一行的末尾与 `$` 模式不匹配。 当原始输入字符串被拆分为字符串数组时， <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%29?displayProperty=nameWithType> 方法会成功匹配五行中的每一行。 如果调用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法时将 `options` 参数设置为 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>，则找不到匹配项，因为正则表达式模式不考虑回车符元素 (\u+000D)。 但是，如果通过用将 `$` 替换成 `\r?$`修改了正则表达式模式，则调用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法并将 `options` 参数设置为 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 将再次找到五个匹配项。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring1.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring1.vb#2)]  
  
 [返回页首](#top)  
  
<a name="StartOnly"></a>   
## <a name="start-of-string-only-a"></a>仅字符串的开头：\A  
 `\A` 定位点指定匹配必须出现在输入字符串的开头。 它等同于 `^` 定位点，只不过 `\A` 忽略了 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项。 因此，在多行的输入字符串中，它只能匹配第一行的开头。  
  
 以下示例与 `^` 和 `$` 定位点的示例类似。 它在正则表达式中使用 `\A` 定位点，可提取有关某些职业棒球队存在年限的信息。 输入字符串包括五行。 调用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法仅找到输入字符串中与正则表达式模式匹配的第一个子字符串。 如示例所示， <xref:System.Text.RegularExpressions.RegexOptions.Multiline> 选项不起作用。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/startofstring2.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/startofstring2.vb#3)]  
  
 [返回页首](#top)  
  
<a name="EndOrNOnly"></a>   
## <a name="end-of-string-or-before-ending-newline-z"></a>字符串末尾或结束换行之前：\Z  
 `\Z` 定位点指定匹配项必须出现在输入字符串的末尾，或出现在输入字符串末尾的 `\n` 之前。 它等同于 `$` 定位点，只不过 `\Z` 忽略了 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项。 因此，在多行字符串中，它只能匹配最后一行的末尾，或 `\n`前的最后一行。  
  
 请注意， `\Z` 与 `\n` 匹配但与 `\r\n` （CR/LF 字符组合）不匹配。 若要匹配 CR/LF，请将 `\r?\Z` 包括到正则表达式模式中。  
  
 以下示例在正则表达式中使用 `\Z` 定位点，与 [字符串或行的开头](#Start) 部分的示例类似，可提取有关某些职业棒球队存在年限的信息。 正则表达式 `\r?\Z` 中的子表达式 `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\Z` 匹配字符串的末尾，也匹配以 `\n` 或 `\r\n`结尾的字符串。 因此，数组中的每个元素都与正则表达式模式匹配。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring2.cs#4)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring2.vb#4)]  
  
 [返回页首](#top)  
  
<a name="EndOnly"></a>   
## <a name="end-of-string-only-z"></a>仅字符串末尾：\z  
 `\z` 定位点指定匹配必须出现在输入字符串末尾。 与 `$` 语言元素类似， `\z` 忽略了 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项。 与 `\Z` 语言元素不同， `\z` 不匹配字符串末尾的 `\n` 字符。 因此，它只能匹配输入字符串的最后一行。  
  
 以下示例在正则表达式中使用 `\z` 定位点，与上一部分的示例中使用的定位点在其他方面相同，用于提取有关某些职业棒球队存在年限的信息。 此示例尝试使用正则表达式模式 `^((\w+(\s?)){2,}),\s(\w+\s\w+),(\s\d{4}(-(\d{4}|present))?,?)+\r?\z`匹配字符串数组中五个元素的每一个。 两个字符串以回车符和换行符结尾，一个字符串以换行符结尾，另外两个既不以回车符也不以换行符结尾。 如输出所示，只有不包含回车符或换行符的字符串与模式匹配。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/endofstring3.cs#5)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/endofstring3.vb#5)]  
  
 [返回页首](#top)  
  
<a name="Contiguous"></a>   
## <a name="contiguous-matches-g"></a>连续匹配：\G  
 `\G` 定位符指定匹配必须出现在上一个匹配结束的点。 将此定位点与 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 或 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法配合使用时，它可确保所有匹配项是连续的。  
  
 以下示例使用正则表达式从一个以逗号分隔的字符串中提取啮齿类动物的名称。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/contiguous1.cs#6)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/contiguous1.vb#6)]  
  
 正则表达式 `\G(\w+\s?\w*),?` 可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\G`|从上次匹配结束的位置开始。|  
|`\w+`|匹配一个或多个单词字符。|  
|`\s?`|匹配零个或一个空格。|  
|`\w*`|匹配零个或多个单词字符。|  
|`(\w+\s?\w*)`|匹配后跟零个或一个空格再后跟零个或多个单词字符的一个或多个单词字符。 这是第一个捕获组。|  
|`,?`|匹配文本逗号字符的零个或一个匹配项。|  
  
 [返回页首](#top)  
  
<a name="WordBoundary"></a>   
## <a name="word-boundary-b"></a>字边界：\b  
 `\b` 定位符指定匹配必须出现单词字符（ `\w` 语言元素）和非单词字符（ `\W` 语言元素）之间的边界上。 单词字符包括字母数字字符和下划线；非单词字符包括不为字母数字字符或下划线的任何字符。 （有关详细信息，请参阅[字符类](../../../docs/standard/base-types/character-classes-in-regular-expressions.md)。）匹配也可以出现在字符串开头或结尾处的单词边界上。  
  
 `\b` 定位点经常用于确保子表达式与整个单词而不仅与单词的开头或结尾匹配。 以下示例中的正则表达式 `\bare\w*\b` 阐释了这种用法。 它与任何以子字符串“are”开头的单词匹配。 该示例的输出也阐释了 `\b` 与输入字符串的开头和结尾均匹配。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/word1.cs#7)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/word1.vb#7)]  
  
 正则表达式模式可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`are`|匹配子字符串“are”。|  
|`\w*`|匹配零个或多个单词字符。|  
|`\b`|在单词边界处结束匹配。|  
  
 [返回页首](#top)  
  
<a name="NonwordBoundary"></a>   
## <a name="non-word-boundary-b"></a>非字边界：\B  
 `\B` 定位符指定匹配不得出现在单词边界上。 它与 `\b` 定位点截然相反。  
  
 以下示例使用 `\B` 定位点定位单词中的子字符串“qu”匹配项。 正则表达式模式 `\Bqu\w+` 与以“qu”开头（但“qu”并不位于单词之首）且延续到单词末尾的子字符串匹配。  
  
 [!code-csharp[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.assertions/cs/nonword1.cs#8)]
 [!code-vb[Conceptual.RegEx.Language.Assertions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.assertions/vb/nonword1.vb#8)]  
  
 正则表达式模式可以解释为下表中所示内容。  
  
|模式|说明|  
|-------------|-----------------|  
|`\B`|不在单词边界处开始匹配。|  
|`qu`|匹配子字符串“qu”。|  
|`\w+`|匹配一个或多个单词字符。|  
  
## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)
- [正则表达式选项](../../../docs/standard/base-types/regular-expression-options.md)
