---
title: ICorDebugThread::EnumerateChains-Methode
ms.date: 03/30/2017
api_name:
- ICorDebugThread.EnumerateChains
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread::EnumerateChains
helpviewer_keywords:
- EnumerateChains method [.NET Framework debugging]
- ICorDebugThread::EnumerateChains method [.NET Framework debugging]
ms.assetid: ec00bc21-117e-4acd-9301-2cfafd5be8d3
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: e01f94e9574ebc032bc45490fd88ff92e9104aa3
ms.sourcegitcommit: 5137208fa414d9ca3c58cdfd2155ac81bc89e917
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57482859"
---
# <a name="icordebugthreadenumeratechains-method"></a>ICorDebugThread::EnumerateChains-Methode
Ruft einen Schnittstellenzeiger auf einem ICorDebugChainEnum-Enumerator, der alle Stapelketten in diesem ICorDebugThread-Objekt enthält.  
  
## <a name="syntax"></a>Syntax  
  
```  
HRESULT EnumerateChains (  
    [out] ICorDebugChainEnum **ppChains  
);  
```  
  
## <a name="parameters"></a>Parameter  
 `ppChains`  
 [out] Ein Zeiger auf die Adresse einer `ICorDebugChainEnum` -Objekt, das ermöglicht die Enumeration des alle-Stapels in diesem Thread, beginnend ab der Kette aktiv (d. h. die aktuellste) verkettet.  
  
## <a name="remarks"></a>Hinweise  
 Die Stapelkette stellt die physische Aufrufliste für den Thread dar. Die folgenden Situationen erstellen Sie eine Kette-Grenze von Stack:  
  
-   Ein Übergang verwalteten zum nicht verwalteten oder nicht verwaltet zu verwaltet.  
  
-   Ein Wechsel des Ausführungskontexts.  
  
-   Ein debugger-Hijacking eines Benutzerthreads.  
  
 Im einfachen Fall für einen Thread, der ausschließlich verwalteten Code in einem einzelnen Kontext ausgeführt wird, wird eine 1: 1-Entsprechung zwischen Threads und Stapelketten vorhanden.  
  
 Ein Debugger sollten die physischen Aufruflisten aller Threads in logischen Aufruflisten neu anzuordnen. Dies würde betreffen, sortieren die Threads Ketten nach deren Aufrufer-/Aufgerufener-Beziehungen und liefern sie.  
  
## <a name="requirements"></a>Anforderungen  
 **Plattformen:** Weitere Informationen finden Sie unter [Systemanforderungen](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Header:** CorDebug.idl, CorDebug.h  
  
 **Bibliothek:** CorGuids.lib  
  
 **.NET Framework-Versionen:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
