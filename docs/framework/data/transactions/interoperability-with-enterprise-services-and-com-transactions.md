---
title: Interoperabilität mit Enterprise Services und COM+-Transaktionen
ms.date: 03/30/2017
ms.assetid: d0fd0d26-fe86-443b-b208-4d57d39fa4aa
ms.openlocfilehash: 8b86a032e7cbc27332864c9cc96009f12b72c53d
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59301905"
---
# <a name="interoperability-with-enterprise-services-and-com-transactions"></a>Interoperabilität mit Enterprise Services und COM+-Transaktionen
Der <xref:System.Transactions>-Namespace unterstützt die Interoperabilität zwischen Transaktionsobjekten, die in diesem Namespace erstellt wurden, und mit COM+ erstellten Transaktionen.  
  
 Mithilfe der <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration können Sie beim Erstellen einer neuen <xref:System.Transactions.TransactionScope>-Instanz das Maß an Interoperabilität mit COM+ festlegen.  
  
 Standardmäßig wird bei Ihrem Anwendungscode die statische überprüft <xref:System.Transactions.Transaction.Current%2A> Eigenschaft <xref:System.Transactions> versucht, die für eine Transaktion zu suchen, die ansonsten aktuell ist oder ein <xref:System.Transactions.TransactionScope> -Objekt, das bestimmt, <xref:System.Transactions.Transaction.Current%2A> ist **Null**. Wenn weder eine Transaktion noch ein Objekt gefunden werden kann, die bzw. das diese Bedingungen erfüllt, fragt <xref:System.Transactions> eine Transaktion aus dem COM+-Kontext ab. Beachten Sie, dass <xref:System.Transactions> auch dann systemeigene <xref:System.Transactions>-Transaktionen bevorzugt, wenn eine Transaktion im COM+-Kontext gefunden wird  
  
## <a name="interoperability-levels"></a>Interoperabilitätsstufen  
 Die <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration definiert die Interoperabilitätsstufen <xref:System.Transactions.EnterpriseServicesInteropOption.None>, <xref:System.Transactions.EnterpriseServicesInteropOption.Full> und <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>.  
  
 Die <xref:System.Transactions.TransactionScope>-Klasse stellt Konstruktoren bereit, die <xref:System.Transactions.EnterpriseServicesInteropOption> als Parameter akzeptieren.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.None>, wie der Name schon sagt, impliziert, dass keine Interoperabilität zwischen <xref:System.EnterpriseServices> -Kontexten und Transaktionsbereichen. Wenn ein <xref:System.Transactions.TransactionScope>-Objekt mit <xref:System.Transactions.EnterpriseServicesInteropOption.None> erstellt wurde, werden die Änderungen an <xref:System.Transactions.Transaction.Current%2A> nicht im COM+-Kontext widergespiegelt. Ebenso werden Änderungen an Transaktionen im COM+-Kontext nicht in <xref:System.Transactions.Transaction.Current%2A> widergespiegelt. Dies ist der schnellste Betriebsmodus von <xref:System.Transactions>, da keine zusätzliche Synchronisierung erforderlich ist. <xref:System.Transactions.EnterpriseServicesInteropOption.None> ist der Standardwert, der von verwendeten <xref:System.Transactions.TransactionScope> alle Konstruktoren, die keine akzeptieren <xref:System.Transactions.EnterpriseServicesInteropOption> als Parameter.  
  
 Wenn Sie <xref:System.EnterpriseServices>-Transaktionen mit einer Ambient-Transaktion kombinieren möchten, müssen Sie entweder <xref:System.Transactions.EnterpriseServicesInteropOption.Full> oder <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> angeben. Beide Werte setzen eine Funktion namens Dienste ohne Komponenten voraus, und daher sollte beim Einsatz dieser Werte auf dem Computer Windows&amp;#160;XP Service&amp;#160;Pack&amp;#160;2 oder Windows&amp;#160;Server&amp;#160;2003 verwendet werden.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.Full> Gibt an, dass die ambient-Transaktionen für <xref:System.Transactions> und <xref:System.EnterpriseServices> sind immer gleich. Daraufhin wird ein neuer <xref:System.EnterpriseServices>-Transaktionskontext erstellt, und die Transaktion, die im <xref:System.Transactions.TransactionScope> aktuell ist, wird für diesen Kontext als aktuelle Transaktion übernommen. Folglich stimmt die Transaktion in <xref:System.Transactions.Transaction.Current%2A> völlig mit der Transaktion in <xref:System.EnterpriseServices.ContextUtil.Transaction%2A> überein. Durch diesen Wert wird die Leistung beeinträchtigt, da möglicherweise neue COM+-Kontexte erstellt werden müssen.  
  
 <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> Gibt an, die folgenden Anforderungen:  
  
-   Wenn <xref:System.Transactions.Transaction.Current%2A> überprüft wird, sollte <xref:System.Transactions> Transaktionen im COM+-Kontext unterstützen, wenn das Transaktionssystem erkennt, dass es in einem anderen Kontext als dem Standardkontext ausgeführt wird. Beachten Sie, dass der Standardkontext keine Transaktion enthalten kann. Im Standardkontext wird daher auch bei der Einstellung <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> die Transaktion, die im lokalen von <xref:System.Transactions> verwendeten Threadspeicher gespeichert wird, für <xref:System.Transactions.Transaction.Current%2A> zurückgegeben.  
  
-   Wenn ein neues <xref:System.Transactions.TransactionScope>-Objekt erstellt wird, und die Erstellung in einem anderen Kontext als dem Standardkontext erfolgt, dann sollte die Transaktion, die für das <xref:System.Transactions.TransactionScope>-Objekt aktuell ist, in COM+ widergespiegelt werden. In diesem Fall verhält sich <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> insofern wie <xref:System.Transactions.EnterpriseServicesInteropOption.Full>, als dass ein neuer COM+-Kontext erstellt wird.  
  
 Wenn <xref:System.Transactions.Transaction.Current%2A> sowohl in <xref:System.Transactions.EnterpriseServicesInteropOption.Full> als auch in <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> festgelegt wurde, bewirken diesen beiden Modi, dass <xref:System.Transactions.Transaction.Current%2A> nicht direkt festgelegt werden kann.  Jeder Versuch, <xref:System.Transactions.Transaction.Current%2A> direkt und nicht durch die Erstellung eines <xref:System.Transactions.TransactionScope>-Objekts festzulegen, resultiert in einer Ausnahme des Typs <xref:System.InvalidOperationException>. Der <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumerationswert wird an neue Transaktionsbereiche vererbt, für die nicht explizit festgelegt wird, welcher Wert verwendet erden soll. Wenn Sie beispielsweise ein <xref:System.Transactions.TransactionScope>-Objekt unter Angabe von <xref:System.Transactions.EnterpriseServicesInteropOption.Full> erstellen, und dann ein zweites <xref:System.Transactions.TransactionScope>-Objekt erstellen, aber keinen <xref:System.Transactions.EnterpriseServicesInteropOption>-Wert angeben, verfügt das zweite <xref:System.Transactions.TransactionScope>-Objekt ebenso über die Einstellung <xref:System.Transactions.EnterpriseServicesInteropOption.Full>.  
  
 Zusammenfasst gelten die folgenden Regeln für die Erstellung eines neuen Transaktionsbereichs:  
  
1. <xref:System.Transactions.Transaction.Current%2A> wird überprüft, um festzustellen, ob eine Transaktion vorhanden ist. Diese Prüfung resultiert in:  
  
    -   Eine Prüfung, um festzustellen, ob ein Bereich vorhanden ist.  
  
    -   Wenn ein Bereich vorhanden ist, wird überprüft, welcher Wert der <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration bei der ursprünglichen Erstellung des Bereichs übergeben wurde.  
  
    -   Wenn die <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration auf <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic> festgelegt wurde, dann hat die COM+-Transaktion (<xref:System.EnterpriseServices>-Transaktion) Vorrang vor der <xref:System.Transactions>-Transaktion im verwalteten lokalen Threadspeicher.  
  
         Wenn der Wert auf <xref:System.Transactions.EnterpriseServicesInteropOption.None> festgelegt wurde, dann hat die <xref:System.Transactions>-Transaktion im verwalteten lokalen Threadspeicher Vorrang.  
  
         Wenn der Wert <xref:System.Transactions.EnterpriseServicesInteropOption.Full> lautet, ist nur eine Transaktion vorhanden, und dies ist eine COM+-Transaktion.  
  
2. Es wird der Wert der <xref:System.Transactions.TransactionScopeOption>-Enumeration überprüft, der vom <xref:System.Transactions.TransactionScope>-Konstruktor übergeben wird. Damit wird bestimmt, ob eine neue Transaktion erstellt werden muss.  
  
3. Wenn eine neue Transaktion erstellt werden muss, bewirken die folgenden Werte von <xref:System.Transactions.EnterpriseServicesInteropOption> Folgendes:  
  
    -   <xref:System.Transactions.EnterpriseServicesInteropOption.Full>: eine COM+-Kontext zugeordnete Transaktion wird erstellt.  
  
    -   <xref:System.Transactions.EnterpriseServicesInteropOption.None>: eine <xref:System.Transactions> Transaktion erstellt.  
  
    -   <xref:System.Transactions.EnterpriseServicesInteropOption.Automatic>: bei ein COM+-Kontext wird eine Transaktion erstellt und an den Kontext angefügt ist.  
  
 Die folgende Tabelle veranschaulicht den Enterprise Services (ES)-Kontext und einen Transaktionsbereich, der erfordert, dass Transaktionen die <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration verwenden.  
  
|ES-Kontext|Keiner|Automatisch|Vollständig|  
|----------------|----------|---------------|----------|  
|Standardkontext|Standardkontext|Standardkontext|Einen neuen <br />Transaktionskontext erstellen|  
|Kein Standardkontext|Den Kontexts des Clients beibehalten|Einen neuen Transaktionskontext erstellen|Einen neuen Transaktionskontext erstellen|  
  
 Die folgende Tabelle zeigt, welche Art von Ambient-Transaktion eingesetzt wird, wenn ein bestimmter <xref:System.EnterpriseServices>-Kontext und ein Transaktionsbereich gegeben sind, der eine Transaktion erfordert, in der die <xref:System.Transactions.EnterpriseServicesInteropOption>-Enumeration verwendet wird.  
  
|ES-Kontext|Keiner|Automatisch|Vollständig|  
|----------------|----------|---------------|----------|  
|Standardkontext|ST|ST|ES|  
|Kein Standardkontext|ST|ES|ES|  
  
 Erläuterungen zur vorstehenden Tabelle:  
  
-   ST bedeutet, dass die Ambient-Transaktion des Bereichs von <xref:System.Transactions> getrennt von allen anderen gegebenenfalls vorhandenen Transaktionen des <xref:System.EnterpriseServices>-Kontexts verwaltet wird.  
  
-   ES bedeutet, dass die Ambient-Transaktion des Bereichs mit der Transaktion des <xref:System.EnterpriseServices>-Kontexts identisch ist.
