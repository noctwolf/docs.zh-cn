---
title: CodeChunkInfo 结构
ms.date: 03/30/2017
api_name:
- CodeChunkInfo
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CodeChunkInfo
helpviewer_keywords:
- CodeChunkInfo structure [.NET Framework debugging]
ms.assetid: 0f482454-8517-48de-ba7a-d7aedab13bb5
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 2baefa45deb8c13e8c1e627724fbe271b210a9ce
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67740882"
---
# <a name="codechunkinfo-structure"></a>CodeChunkInfo 结构

表示内存中的单一代码块。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _CodeChunkInfo {  
    CORDB_ADDRESS startAddr;  
    ULONG32       length;  
} CodeChunkInfo;  
```  
  
## <a name="members"></a>成员  
  
|成员|描述|  
|------------|-----------------|  
|`startAddr`|一个`CORDB_ADDRESS`值，该值指定块的起始地址。|  
|`length`|以字节为单位的块区的大小。|  
  
## <a name="remarks"></a>备注  
 单一代码块是本机代码的代码对象，例如函数的一部分的区域。  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** CorDebug.idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [GetCodeChunks 方法](../../../../docs/framework/unmanaged-api/debugging/icordebugcode2-getcodechunks-method.md)
- [调试结构](../../../../docs/framework/unmanaged-api/debugging/debugging-structures.md)
- [调试](../../../../docs/framework/unmanaged-api/debugging/index.md)
