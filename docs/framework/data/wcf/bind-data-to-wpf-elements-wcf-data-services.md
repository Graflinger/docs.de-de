---
title: 'Vorgehensweise: Binden von Daten an Windows Presentation Foundation-Elemente (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding, WCF Data Services
- WCF Data Services, data binding
ms.assetid: d6538ab0-0abe-426a-b9d9-e6f3a5ca2016
ms.openlocfilehash: 559857ab647d6a1e6d2ae7ffcc344e7583b48553
ms.sourcegitcommit: d9a0071d0fd490ae006c816f78a563b9946e269a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "55066171"
---
# <a name="how-to-bind-data-to-windows-presentation-foundation-elements-wcf-data-services"></a>Vorgehensweise: Binden von Daten an Windows Presentation Foundation-Elemente (WCF Data Services)
Mit [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)], Sie können z. B. Windows Presentation Foundation (WPF)-Elemente Binden einer <xref:System.Windows.Controls.ListBox> oder <xref:System.Windows.Controls.ComboBox> mit einer Instanz von <xref:System.Data.Services.Client.DataServiceCollection%601>, die die von den Steuerelementen zu ausgelösten Ereignisse behandelt die <xref:System.Data.Services.Client.DataServiceContext> mit Änderungen synchronisiert versucht, Daten in den Steuerelementen. Weitere Informationen finden Sie unter [Binden von Daten an Steuerelemente](../../../../docs/framework/data/wcf/binding-data-to-controls-wcf-data-services.md).  
  
 Im Beispiel in diesem Thema werden der Northwind-Beispieldatendienst und automatisch generierte Client-Datendienstklassen verwendet. Dieser Dienst und die clientdatenklassen werden erstellt, wenn Sie die [WCF Data Services-Schnellstart](../../../../docs/framework/data/wcf/quickstart-wcf-data-services.md).  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel ist aus der CodeBehind-Seite für eine Extensible Application Markup Language (XAML)-Seite, die das Fenster `SalesOrders` in WPF definiert. Wenn das Fenster geladen wird, wird eine <xref:System.Data.Services.Client.DataServiceCollection%601> basierend auf dem Ergebnis einer Abfrage erstellt, die Kunden nach Land gefiltert zurückgibt. Alle Seiten dieses ausgelagerten Ergebnisses werden zusammen mit den verknüpften Bestellungen geladen und an die <xref:System.Windows.FrameworkElement.DataContext%2A>-Eigenschaft des <xref:System.Windows.Controls.StackPanel> gebunden, der das Stammlayoutsteuerelement für das WPF-Fenster ist. Weitere Informationen finden Sie unter [verzögerte Inhalte laden](../../../../docs/framework/data/wcf/loading-deferred-content-wcf-data-services.md).  
  
 [!code-csharp[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderswpf3.xaml.cs#bindpageddata)]
 [!code-vb[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf3.xaml.vb#bindpageddata)]  
  
## <a name="example"></a>Beispiel  
 Der folgende XAML-Code definiert das Fenster `SalesOrders` in WPF für das vorherige Beispiel.  
  
 [!code-xaml[Astoria Northwind Client#BindPagedDataXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf3.xaml#bindpageddataxaml)]  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel ist aus der CodeBehind-Seite für eine Extensible Application Markup Language (XAML)-Seite, die das Fenster `SalesOrders` in WPF definiert. Wenn das Fenster geladen wird, wird eine <xref:System.Data.Services.Client.DataServiceCollection%601> basierend auf dem Ergebnis einer Abfrage erstellt, die Kunden mit verknüpften Objekten nach Land gefiltert zurückgibt. Dieses Ergebnis wird an die <xref:System.Windows.FrameworkElement.DataContext%2A>-Eigenschaft des <xref:System.Windows.Controls.StackPanel> gebunden, der das Stammlayoutsteuerelement für das WPF-Fenster ist.  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderswpf.xaml.cs#wpfdatabinding)]
 [!code-vb[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf.xaml.vb#wpfdatabinding)]  
  
## <a name="example"></a>Beispiel  
 Der folgende XAML-Code definiert das Fenster `SalesOrders` in WPF für das vorherige Beispiel.  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf.xaml#wpfdatabindingxaml)]
