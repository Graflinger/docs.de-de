---
title: 'Vorgehensweise: Aufrufen von Objekten, die an DataGridView-Zeilen in Windows Forms gebunden sind'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- object binding [Windows Forms], accessing bound objects
- data grids [Windows Forms], accessing bound objects
- DataGridView control [Windows Forms], accessing objects bound to rows
ms.assetid: 0e05748f-4403-4eb8-8b2f-b098108181b5
ms.openlocfilehash: 50882ab9a1a498bf8f76381e3f4aac53876abbb8
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59217405"
---
# <a name="how-to-access-objects-bound-to-windows-forms-datagridview-rows"></a>Vorgehensweise: Aufrufen von Objekten, die an DataGridView-Zeilen in Windows Forms gebunden sind
Manchmal ist es hilfreich, eine Tabelle mit Informationen anzuzeigen, die in einer Auflistung von Geschäftsobjekten gespeichert sind. Beim Binden eines <xref:System.Windows.Forms.DataGridView>-Steuerelements an eine solche Auflistung wird jede öffentliche Eigenschaft in einer eigenen Spalte angezeigt, es sei denn, die Eigenschaft wurde mit <xref:System.ComponentModel.BrowsableAttribute> als nicht durchsuchbar markiert. Angenommen, eine Auflistung von `Customer`-Objekten hätte Spalten wie **Name** und **Adresse**.  
  
 Wenn diese Objekte zusätzliche Informationen und Code enthalten, auf die Sie zugreifen möchten, können Sie dies über Zeilenobjekte erreichen. Im folgenden Codebeispiel können Benutzer mehrere Zeilen auswählen und auf eine Schaltfläche klicken, um eine Rechnung an jeden der entsprechenden Kunden zu senden.  
  
### <a name="to-access-row-bound-objects"></a>So greifen Sie auf zeilengebundene Objekte zu  
  
-   Verwenden Sie die <xref:System.Windows.Forms.DataGridViewRow.DataBoundItem%2A?displayProperty=nameWithType>-Eigenschaft.  
  
     [!code-csharp[System.Windows.Forms.DataGridViewObjectBinding#10](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/CS/datagridviewobjectbinding.cs#10)]
     [!code-vb[System.Windows.Forms.DataGridViewObjectBinding#10](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/VB/datagridviewobjectbinding.vb#10)]  
  
## <a name="example"></a>Beispiel  
 Das vollständige Codebeispiel beinhaltet eine einfache `Customer`-Implementierung und bindet die <xref:System.Windows.Forms.DataGridView> an eine <xref:System.Collections.ArrayList>, die ein paar `Customer`-Objekte enthält. Der <xref:System.Windows.Forms.Control.Click>-Ereignishandler der <xref:System.Windows.Forms.Button?displayProperty=nameWithType> muss über die Zeilen auf die `Customer`-Objekte zugreifen, weil außerhalb des <xref:System.Windows.Forms.Form.Load?displayProperty=nameWithType>-Ereignishandlers nicht auf die Kundenauflistung zugegriffen werden kann.  
  
 [!code-csharp[System.Windows.Forms.DataGridViewObjectBinding#00](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/CS/datagridviewobjectbinding.cs#00)]
 [!code-vb[System.Windows.Forms.DataGridViewObjectBinding#00](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewObjectBinding/VB/datagridviewobjectbinding.vb#00)]  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Für dieses Beispiel benötigen Sie Folgendes:  
  
-   Verweise auf die Assemblys "System" und "System.Windows.Forms".  
  
 Informationen zum Erstellen dieses Beispiels über die Befehlszeile für Visual Basic oder Visual c# finden Sie unter [erstellen über die Befehlszeile](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md) oder [Befehlszeile mit csc.exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md). Sie können auch in diesem Beispiel in Visual Studio erstellen, indem Sie den Code in ein neues Projekt einfügen.  
  
## <a name="see-also"></a>Siehe auch

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewRow>
- <xref:System.Windows.Forms.DataGridViewRow.DataBoundItem%2A?displayProperty=nameWithType>
- [Anzeigen von Daten im DataGridView-Steuerelement in Windows Forms](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Vorgehensweise: Binden von Objekten an das DataGridView-Steuerelement in Windows Forms](how-to-bind-objects-to-windows-forms-datagridview-controls.md)
