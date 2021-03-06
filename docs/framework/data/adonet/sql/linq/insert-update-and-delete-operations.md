---
title: Insert-, Update- und Delete-Operationen
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 26a43a4f-83c9-4732-806d-bb23aad0ff6b
ms.openlocfilehash: 6a25ea5fe80da1fed16f44fd3243ebea4d64069f
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59230673"
---
# <a name="insert-update-and-delete-operations"></a>Insert-, Update- und Delete-Operationen
Sie führen die Operationen `Insert`, `Update` und `Delete` in [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] aus, indem Sie Objekte dem Objektmodell hinzufügen, diese ändern oder entfernen. Standardmäßig übersetzt [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] Ihre Aktionen in SQL und übergibt die Änderungen an die Datenbank.  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] bietet maximale Flexibilität bei der Bearbeitung und Beibehalten von Änderungen, die Sie sich an Ihren Objekten vorgenommen. Sobald Entitätsobjekte zur Verfügung stehen (entweder durch Abrufen in einer Abfrage oder durch Neuzusammenstellung), können Sie diese wie typische Objekte in Ihrer Anwendung ändern. D. h. Sie deren Werte ändern, können Sie diese Auflistungen hinzufügen und können sie aus Ihren Sammlungen entfernt werden. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] verfolgt die Änderungen und kann diese zurück in die Datenbank übertragen werden, beim Aufrufen <xref:System.Data.Linq.DataContext.SubmitChanges%2A>.  
  
> [!NOTE]
>  [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] nicht unterstützt oder erkennt kaskadierte Löschvorgänge. Wenn Sie möchten eine Zeile in einer Tabelle zu löschen, die Einschränkungen gelten, müssen Sie entweder die `ON DELETE CASCADE` -Regel in der foreign Key-Einschränkung in der Datenbank, oder verwenden Sie Ihren eigenen Code, um zunächst die untergeordneten Objekte löschen, die verhindern, dass das übergeordnete Objekt gelöscht. Andernfalls wird eine Ausnahme ausgelöst. Weitere Informationen finden Sie unter [Vorgehensweise: Löschen von Zeilen aus der Datenbank](../../../../../../docs/framework/data/adonet/sql/linq/how-to-delete-rows-from-the-database.md).  
  
 Die folgenden Auszüge verwenden die `Customer`-Klasse und die `Order`-Klasse aus der Beispieldatenbank Northwind. Klassendefinitionen werden zur besseren Übersicht nicht angezeigt.  
  
 [!code-csharp[DLinqCRUDOps#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCRUDOps/cs/Program.cs#1)]
 [!code-vb[DLinqCRUDOps#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCRUDOps/vb/Module1.vb#1)]  
  
 Wenn Sie <xref:System.Data.Linq.DataContext.SubmitChanges%2A> aufrufen, übernimmt [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] automatisch die Erzeugung und Ausführung der SQL-Befehle, die zur Übertragung Ihrer Änderungen in die Datenbank erforderlich sind.  
  
> [!NOTE]
>  Sie können dieses Verhalten überschreiben, indem Sie Ihre eigene Logik verwenden (typischerweise in Form einer gespeicherten Prozedur). Weitere Informationen finden Sie unter [Verantwortlichkeiten der Entwickler In Überschreiben von Standardverhalten](../../../../../../docs/framework/data/adonet/sql/linq/responsibilities-of-the-developer-in-overriding-default-behavior.md).  
>   
>  Können Entwickler mithilfe von Visual Studio die [!INCLUDE[vs_ordesigner_long](../../../../../../includes/vs-ordesigner-long-md.md)] gespeicherte Prozeduren zu diesem Zweck entwickeln.  
  
## <a name="see-also"></a>Siehe auch

- [Herunterladen von Beispieldatenbanken](../../../../../../docs/framework/data/adonet/sql/linq/downloading-sample-databases.md)
- [Anpassen von Insert-, Update- und Delete-Operationen](../../../../../../docs/framework/data/adonet/sql/linq/customizing-insert-update-and-delete-operations.md)
