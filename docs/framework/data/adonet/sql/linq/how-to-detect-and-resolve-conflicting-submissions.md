---
title: 'Vorgehensweise: Erkennen und Auflösen von Übergabekonflikten'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 91e27206-01fb-4c7a-8afc-1383a6ac5067
ms.openlocfilehash: 606231449263f1c26596ca8606a88053c6aded8e
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59138124"
---
# <a name="how-to-detect-and-resolve-conflicting-submissions"></a>Vorgehensweise: Erkennen und Auflösen von Übergabekonflikten
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] bietet viele Ressourcen zum Erkennen und Lösen von Konflikten, die in der Datenbank von mehreren Benutzern Änderungen ergeben. Weitere Informationen finden Sie unter [Vorgehensweise: Verwalten von Änderungskonflikten](../../../../../../docs/framework/data/adonet/sql/linq/how-to-manage-change-conflicts.md).  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel zeigt eine `try` / `catch` Block, der fängt eine <xref:System.Data.Linq.ChangeConflictException> Ausnahme. Entitäts- und Memberinformationen für jeden Konflikt werden im Konsolenfenster angezeigt.  
  
> [!NOTE]
>  Sie müssen die `using System.Reflection`-Direktive (`Imports System.Reflection` in Visual Basic) einschließen, um das Abrufen von Informationen zu unterstützen. Weitere Informationen finden Sie unter <xref:System.Reflection>.  
  
 [!code-csharp[DLinqSubmittingChanges#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#2)]
 [!code-vb[DLinqSubmittingChanges#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#2)]  
  
## <a name="see-also"></a>Siehe auch

- [Vornehmen und Übergeben von Datenänderungen](../../../../../../docs/framework/data/adonet/sql/linq/making-and-submitting-data-changes.md)
- [Vorgehensweise: Verwalten von Änderungskonflikten](../../../../../../docs/framework/data/adonet/sql/linq/how-to-manage-change-conflicts.md)
