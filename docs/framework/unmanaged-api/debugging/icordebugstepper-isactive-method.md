---
title: ICorDebugStepper::IsActive 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.IsActive
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::IsActive
helpviewer_keywords:
- IsActive method, ICorDebugStepper interface [.NET Framework debugging]
- ICorDebugStepper::IsActive method [.NET Framework debugging]
ms.assetid: 8b35e7a9-b40e-40a9-8d8e-b82e823fc575
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9dde27f74ac59d033b6e25fba1dbb8e52c4b91af
ms.sourcegitcommit: 7f616512044ab7795e32806578e8dc0c6a0e038f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67760674"
---
# <a name="icordebugstepperisactive-method"></a>ICorDebugStepper::IsActive 方法
获取一个值，该值指示是否此 ICorDebugStepper 当前正在执行一个步骤。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsActive (  
    [out] BOOL   *pbActive  
);  
```  
  
## <a name="parameters"></a>参数  
 `pbActive`  
 [out]返回`true`分档器当前正在执行一个步骤; 否则，返回`false`。  
  
## <a name="remarks"></a>备注  
 任何步骤操作保持活动状态，直到收到调试器[icordebugmanagedcallback:: Stepcomplete](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-stepcomplete-method.md)调用，这将自动停用分档器。 分档器可能还会停用过早地通过调用[icordebugstepper:: Deactivate](../../../../docs/framework/unmanaged-api/debugging/icordebugstepper-deactivate-method.md)在回调之前条件为止。  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统需求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** CorDebug.idl、 CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
