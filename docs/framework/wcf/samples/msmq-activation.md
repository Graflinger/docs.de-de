---
title: MSMQ-Aktivierung
ms.date: 03/30/2017
ms.assetid: e3834149-7b8c-4a54-806b-b4296720f31d
ms.openlocfilehash: d83759f321abe7fa7e39202daadd4ceda82d8f23
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59295678"
---
# <a name="msmq-activation"></a>MSMQ-Aktivierung
Dieses Beispiel veranschaulicht das Hosten von Anwendungen in Windows Process Activation Service (WAS), die von einer Nachrichtenwarteschlange gelesen werden. Dieses Beispiel verwendet die `netMsmqBinding` und basiert auf der [bidirektionaler Kommunikation](../../../../docs/framework/wcf/samples/two-way-communication.md) Beispiel. In diesem Fall handelt es sich bei dem Dienst um eine im Internet gehostete Anwendung. Der Client ist selbst gehostet und gibt an die Konsole aus, um den Status eingereichter Bestellungen zu beobachten.  
  
> [!NOTE]
>  Die Setupprozedur und die Buildanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
> [!NOTE]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert. Suchen Sie nach dem folgenden Verzeichnis (Standardverzeichnis), bevor Sie fortfahren.  
>   
>  \<InstallDrive>:\WF_WCF_Samples  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, fahren Sie mit [Windows Communication Foundation (WCF) und Windows Workflow Foundation (WF) Samples für .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) zum Herunterladen aller WCF und [!INCLUDE[wf1](../../../../includes/wf1-md.md)] Beispiele. Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  \<InstallDrive>:\Samples\WCFWFCardSpace\WCF\Basic\Services\Hosting\WASHost\MsmqActivation.  
  
 Windows Process Activation Service (WAS), der neue Prozessaktivierungsmechanismus für [!INCLUDE[lserver](../../../../includes/lserver-md.md)], stellt IIS-ähnliche Funktionenbereit, die zuvor nur für HTTP-basierte Anwendungen zur Verfügung standen, die Nicht-HTTP-Protokolle verwenden. Windows Communication Foundation (WCF) verwendet die Listeneradapter-Schnittstelle, um aktivierungsanforderungen weiterzugeben, die über die von WCF, z. B. TCP, Named Pipes und MSMQ unterstützten nicht-HTTP-Protokolle empfangen werden. Die Funktionalität für den Empfang von Anforderungen über Nicht-HTTP-Protokolle wird von verwalteten Windows-Diensten gehostet, die in "SMSvcHost.exe" ausgeführt werden.  
  
 Der Net.Msmq-Listeneradapterdienst (NetMsmqActivator) aktiviert in der Warteschlange befindliche Anwendungen auf der Grundlage von Nachrichten in der Warteschlange.  
  
 Der Client sendet Bestellungen aus dem Bereich einer Transaktion an den Dienst. Der Dienst empfängt die Bestellungen in einer Transaktion und verarbeitet sie. Der Dienst führt dann einen Rückruf an den Client mit dem Status der Bestellung durch. Zur Vereinfachung der bidirektionalen Kommunikation verwenden sowohl der Client als auch der Dienst Warteschlangen für Bestellungen und den Auftragsstatus.  
  
 Der `IOrderProcessor`-Dienstvertrag definiert die unidirektionalen Dienstvorgänge, die mit Warteschlangen funktionieren. Der Dienstvorgang nutzt den Antwortendpunkt, um Bestellstatus an den Client zu senden. Die Adresse des Antwortendpunkts ist der URI der Warteschlange, die für die Rücksendung des Auftragsstatus an den Client verwendet wird. Die Anwendung für die Auftragsverarbeitung implementiert diesen Vertrag.  
  
```csharp  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOrderProcessor  
{  
    [OperationContract(IsOneWay = true)]  
    void SubmitPurchaseOrder(PurchaseOrder po,   
                                           string reportOrderStatusTo);  
}  
```  
  
 Der Antwortvertrag, an den der Bestellstatus gesendet wird, wird vom Client angegeben. Der Client implementiert den Auftragsstatusvertrag. Der Dienst verwendet den generierten Client dieses Vertrags, um den Bestellstatus an den Client zurückzusenden.  
  
```csharp  
[ServiceContract]  
public interface IOrderStatus  
{  
    [OperationContract(IsOneWay = true)]  
    void OrderStatus(string poNumber, string status);  
}  
```  
  
 Der Dienstvorgang verarbeitet die übermittelte Bestellung. <xref:System.ServiceModel.OperationBehaviorAttribute> wird auf den Dienstvorgang angewendet, um die automatische Eintragung in die Transaktion, die für den Empfang der Nachricht aus der Warteschlange verwendet wird, und die automatische Fertigstellung der Transaktion bei Abschluss des Dienstvorgangs festzulegen. Die `Orders`-Klasse kapselt die Auftragsverarbeitungsfunktion. In diesem Fall fügt sie die Bestellung einem Wörterbuch hinzu. Die Transaktion, in der der Dienstvorgang eingetragen wurde, steht den Vorgängen in der `Orders`-Klasse zur Verfügung.  
  
 Der Dienstvorgang verarbeitet die übermittelte Bestellung und meldet dem Client den Status der Bestellung.  
  
```csharp  
public class OrderProcessorService : IOrderProcessor  
{  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
    public void SubmitPurchaseOrder(PurchaseOrder po, string reportOrderStatusTo)  
    {  
        Orders.Add(po);  
        Console.WriteLine("Processing {0} ", po);  
        Console.WriteLine("Sending back order status information");  
        NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding();  
        msmqCallbackBinding.Security.Mode = NetMsmqSecurityMode.None;  
        OrderStatusClient client = new OrderStatusClient(msmqCallbackBinding, new EndpointAddress(reportOrderStatusTo));  
        // please note that the same transaction that is used to dequeue purchase order is used  
        // to send back order status  
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))  
        {  
            client.OrderStatus(po.PONumber, po.Status);  
            scope.Complete();  
        }  
    }
}
```  
  
 Die zu verwendende Clientbindung wird mithilfe einer Konfigurationsdatei festgelegt.  
  
 Der MSMQ-Warteschlangenname wird im appSettings-Abschnitt der Konfigurationsdatei angegeben. Der Endpunkt für den Dienst wird im Abschnitt "System.serviceModel" der Konfigurationsdatei definiert.  
  
> [!NOTE]
>  Der Name der MSMQ-Warteschlange und die Endpunktadresse verwenden geringfügig abweichende Adressierungskonventionen. Im MSQM-Warteschlangennamen wird ein Punkt (.) für den lokalen Computer verwendet, und in der Pfadangabe werden umgekehrte Schrägstriche als Trennzeichen verwendet. Die WCF-Endpunktadresse gibt ein net.msmq:-Schema verwendet "Localhost" für den lokalen Computer und Schrägstriche im Pfad. Um aus einer Warteschlange zu lesen, die auf einem Remotecomputer gehostet wird, ersetzen Sie "." und localhost durch den Namen des Remotecomputers.  
  
 Um den Dienstcode in WAS zu hosten, wird eine SVC-Datei mit dem Namen der Klasse verwendet.  
  
 Die Datei "Service.svc" selbst enthält eine Anweisung zur Erstellung von `OrderProcessorService`.  
  
```svc
<%@ServiceHost language="c#" Debug="true" Service="Microsoft.ServiceModel.Samples.OrderProcessorService"%>  
```  
  
 Die Datei „Service.svc“ enthält darüber hinaus eine Assemblyanweisung, um sicherzustellen, dass „System.Transactions.dll“ geladen wird.  
  
```svc  
<%@Assembly name="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"%>  
```  
  
 Der Client erstellt einen Geltungsbereich für die Transaktion. Die Kommunikation mit dem Dienst findet innerhalb des Geltungsbereichs der Transaktion statt, sodass diese in der Folge als unteilbare Einheit behandelt wird, in der alle Nachrichten entweder erfolgreich sind oder fehlschlagen. Für die Transaktion wird ein Commit ausgeführt, indem `Complete` im Geltungsbereich der Transaktion aufgerufen wird.  
  
```csharp  
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderStatusService)))  
{  
    // Open the ServiceHostBase to create listeners and start listening   
    // for order status messages.  
    serviceHost.Open();  
  
    // Create a proxy with given client endpoint configuration  
    OrderProcessorClient client = new OrderProcessorClient();  
  
    // Create the purchase order  
    PurchaseOrder po = new PurchaseOrder();  
    po.CustomerId = "somecustomer.com";  
    po.PONumber = Guid.NewGuid().ToString();  
  
    PurchaseOrderLineItem lineItem1 = new PurchaseOrderLineItem();  
    lineItem1.ProductId = "Blue Widget";  
    lineItem1.Quantity = 54;  
    lineItem1.UnitCost = 29.99F;  
  
    PurchaseOrderLineItem lineItem2 = new PurchaseOrderLineItem();  
    lineItem2.ProductId = "Red Widget";  
    lineItem2.Quantity = 890;  
    lineItem2.UnitCost = 45.89F;  
  
    po.orderLineItems = new PurchaseOrderLineItem[2];  
    po.orderLineItems[0] = lineItem1;  
    po.orderLineItems[1] = lineItem2;  
  
    //Create a transaction scope.  
    using (TransactionScope scope = new   
        TransactionScope(TransactionScopeOption.Required))  
    {  
        // Make a queued call to submit the purchase order  
        client.SubmitPurchaseOrder(po,   
       "net.msmq://localhost/private/ServiceModelSamplesOrder/OrderStatus");  
        // Complete the transaction.  
        scope.Complete();  
    }  
  
    //Closing the client gracefully closes the connection and cleans up   
    //resources  
    client.Close();  
  
    Console.WriteLine();  
    Console.WriteLine("Press <ENTER> to terminate client.");  
    Console.ReadLine();  
  
    // Close the ServiceHostBase to shutdown the service.  
    serviceHost.Close();  
    }  
```  
  
 Der Clientcode implementiert den `IOrderStatus`-Vertrag, um den Auftragsstatus vom Dienst zu empfangen. In diesem Fall druckt er den Auftragsstatus aus.  
  
```csharp  
[ServiceBehavior]  
public class OrderStatusService : IOrderStatus  
{  
    [OperationBehavior(TransactionAutoComplete = true,   
                        TransactionScopeRequired = true)]  
    public void OrderStatus(string poNumber, string status)  
    {  
        Console.WriteLine("Status of order {0}:{1} ",   
                                         poNumber , status);  
    }  
}  
```  
  
 Die Auftragsstatuswarteschlange wird in der `Main`-Methode erstellt. Die Clientkonfiguration beinhaltet die Dienstkonfiguration für den Auftragsstatus, um den Auftragsstatusdienst zu hosten, wie in der folgenden Beispielkonfiguration dargestellt.  
  
```xml  
<appSettings>  
    <!-- use appSetting to configure MSMQ queue name -->  
    <add key="targetQueueName" value=".\private$\ServiceModelSamples/service.svc" />  
    <add key="responseQueueName" value=".\private$\ServiceModelSamples/OrderStatus" />  
  </appSettings>  
  
<system.serviceModel>  
  
    <services>  
      <service   
         name="Microsoft.ServiceModel.Samples.OrderStatusService">  
        <!-- Define NetMsmqEndpoint -->  
        <endpoint address="net.msmq://localhost/private/ServiceModelSamples/OrderStatus"  
                  binding="netMsmqBinding"  
                  contract="Microsoft.ServiceModel.Samples.IOrderStatus" />  
      </service>  
    </services>  
  
    <client>  
      <!-- Define NetMsmqEndpoint -->  
      <endpoint name="OrderProcessorEndpoint"  
                address="net.msmq://localhost/private/ServiceModelSamples/service.svc"   
                binding="netMsmqBinding"   
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
    </client>  
  
  </system.serviceModel>  
```  
  
 Wenn Sie das Beispiel ausführen, werden die Client- und Dienstaktivitäten sowohl im Server- als auch im Clientkonsolenfenster angezeigt. Sie können sehen, wie der Server Nachrichten vom Client empfängt. Drücken Sie die EINGABETASTE in den einzelnen Konsolenfenstern, um den Server und den Client zu schließen.  
  
 Der Client zeigt die vom Server gesendeten Bestellstatusinformationen an.  
  
```console  
Press <ENTER> to terminate client.  
Status of order 70cf9d63-3dfa-4e69-81c2-23aa4478ebed :Pending  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>So können Sie das Beispiel einrichten, erstellen und ausführen  
  
1. Stellen Sie sicher, dass [!INCLUDE[iisver](../../../../includes/iisver-md.md)] installiert ist, da dies zur WAS-Aktivierung erforderlich ist.  
  
2. Stellen Sie sicher, dass Sie ausgeführt haben die [Schritte der Einrichtung einmaligen Setupverfahren für Windows Communication Foundation-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md). Darüber hinaus müssen Sie die WCF-nicht-HTTP-Aktivierung-Komponenten installieren:  
  
    1.  Wählen Sie im Menü **Start** die **Systemsteuerung** aus.  
  
    2.  Wählen Sie **Programme und Funktionen**.  
  
    3.  Klicken Sie auf **Windows-Funktionen ein- oder ausschalten**.  
  
    4.  Klicken Sie unter **Featureübersicht**, klicken Sie auf **Features hinzufügen**.  
  
    5.  Erweitern Sie die **Microsoft .NET Framework 3.0** Knoten und überprüfen Sie die **Windows Communication Foundation-nicht-HTTP-Aktivierung** Feature.  
  
3. Um die C#- oder Visual Basic .NET-Edition der Projektmappe zu erstellen, befolgen Sie die unter [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)aufgeführten Anweisungen.  
  
4. Führen Sie den Client aus, indem Sie "client.exe" von einem Befehlsfenster ausführen. Auf diese Weise wird die Warteschlange erstellt und eine Nachricht an sie gesendet. Der Client verbleibt in der Ausführung, um das Ergebnis des Lesens der Nachricht durch den Dienst zu sehen.  
  
5. Der MSMQ-Aktivierungsdienst wird standardmäßig als Netzwerkdienst ausgeführt. Daher muss die Warteschlange, die zur Aktivierung der Anwendung verwendet wird, über Empfangs- und Einsehberechtigungen für den Netzwerkdienst verfügen. Diese können durch Verwendung von Message Queuing MMC hinzugefügt werden:  
  
    1.  Von der **starten** Menü klicken Sie auf **ausführen**, geben Sie dann `Compmgmt.msc` und drücken Sie EINGABETASTE.  
  
    2.  Klicken Sie unter **Dienste und Anwendungen**, erweitern Sie **Message Queuing-**.  
  
    3.  Klicken Sie auf **Private Warteschlangen**.  
  
    4.  Mit der rechten Maustaste in der Warteschlange (servicemodelsamples/Service.svc), und wählen Sie **Eigenschaften**.  
  
    5.  Auf der **Sicherheit** auf **hinzufügen** Peek gewähren, und erhalten Sie die Berechtigungen für Netzwerkdienst.  
  
6. Konfigurieren Sie den Windows Process Activation Service (WAS), um die MSMQ-Aktivierung zu unterstützen.  
  
     Zur Vereinfachung sind die folgenden beiden Schritte in der Batchdatei AddMsmqSiteBinding.cmd implementiert, die sich im Beispielverzeichnis befindet.  
  
    1.  Zur Unterstützung der net.msmq-Aktivierung muss die Standardwebsite zuerst an das net.msmq-Protokoll gebunden werden. Sie können hierzu das Tool appcmd.exe verwenden, das mit dem [!INCLUDE[iisver](../../../../includes/iisver-md.md)]-Verwaltungstoolset installiert wird. Führen Sie an einer Eingabeaufforderung auf höherer Ebene (Administrator) den folgenden Befehl aus.  
  
        ```console  
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"   
        -+bindings.[protocol='net.msmq',bindingInformation='localhost']  
        ```  
  
        > [!NOTE]
        >  Dieser Befehl ist eine einzelne Textzeile.  
  
         Dieser Befehl fügt der Standardwebsite eine net.msmq-Sitebindung hinzu.  
  
    2.  Alle Anwendungen innerhalb einer Site nutzen zwar eine gemeinsame net.msmq-Bindung, aber jede Anwendung kann die net.msmq-Unterstützung unabhängig von den anderen Anwendungen aktivieren. Um net.msmq für die Anwendung /servicemodelsamples zu aktivieren, führen Sie den folgenden Befehl in einer Eingabeaufforderung auf höherer Ebene (Administrator) aus.  
  
        ```console  
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.msmq  
        ```  
  
        > [!NOTE]
        >  Dieser Befehl ist eine einzelne Textzeile.  
  
         Dieser Befehl aktiviert die Anwendung/servicemodelsamples darauf zugegriffen werden kann `http://localhost/servicemodelsamples` und `net.msmq://localhost/servicemodelsamples`.
  
7. Falls noch nicht geschehen, stellen Sie sicher, dass der MSMQ-Aktivierungsdienst aktiviert ist. Von der **starten** Menü klicken Sie auf **ausführen**, und geben `Services.msc`. Durchsuchen Sie die Liste der Dienste für die **Net.Msmq-Listeneradapter**. Mit der rechten Maustaste, und wählen Sie **Eigenschaften**. Legen Sie die **Starttyp** zu **automatische**, klicken Sie auf **übernehmen** , und klicken Sie auf die **starten** Schaltfläche. Dieser Schritt muss nur einmal vor der ersten Verwendung des Net.Msmq-Listeneradapterdiensts durchgeführt werden.  
  
8. Folgen Sie den Anweisungen, um das Beispiel in einer Konfiguration für die einzelnen-Computer oder computerübergreifend auszuführen, [Ausführen der Windows Communication Foundation-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md). Ändern Sie zusätzlich den Code auf dem Client, der die Bestellung einsendet, sodass beim Einsenden der Bestellung der Computername im URI der Warteschlange angegeben wird. Verwenden Sie folgenden Code:  
  
    ```csharp  
    client.SubmitPurchaseOrder(po, "net.msmq://localhost/private/ServiceModelSamples/OrderStatus");  
    ```  
  
9. Entfernen Sie die net.msmq-Sitebindung, die Sie für dieses Beispiel hinzugefügt haben.  
  
     Zur Vereinfachung sind die folgenden beiden Schritte in einer Batchdatei namens RemoveMsmqSiteBinding.cmd implementiert, die sich im Beispielverzeichnis befindet:  
  
    1.  Entfernen Sie net.msmq aus der Liste der aktivierten Protokolle, indem Sie den folgenden Befehl an einer Eingabeaufforderung auf höherer Ebene (Administrator) ausführen.  
  
        ```console  
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http  
        ```  
  
        > [!NOTE]
        >  Dieser Befehl ist eine einzelne Textzeile.  
  
    2.  Entfernen Sie die net.msmq-Sitebindung, indem Sie den folgenden Befehl in einer Eingabeaufforderung auf höher Ebene ausführen.  
  
        ```console  
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.msmq',bindingInformation='localhost']  
        ```  
  
        > [!NOTE]
        >  Dieser Befehl ist eine einzelne Textzeile.  
  
    > [!WARNING]
    >  Durch die Ausführung der Batchdatei wird der DefaultAppPool zurückgesetzt und mit .NET Framework, Version 2.0, ausgeführt.  
  
 Standardmäßig wird mit der `netMsmqBinding`-Bindung die Transportsicherheit aktiviert. Der Typ der Transportsicherheit wird durch zwei Eigenschaften festgelegt: `MsmqAuthenticationMode` und `MsmqProtectionLevel`. Standardmäßig wird der Authentifizierungsmodus auf `Windows` festgelegt, und die Schutzebene wird auf `Sign` gesetzt. Damit MSMQ die Authentifizierungs- und Signierungsfunktion bereitstellt, muss es ein Teil einer Domäne sein. Wenn Sie in diesem Beispiel auf einem Computer, die nicht Teil einer Domäne ist ausführen, wird der folgende Fehler empfangen: "Des Benutzers interne Message Queuing-Zertifikat ist nicht vorhanden".  
  
### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>So führen Sie das Beispiel auf einem Computer aus, der zu einer Arbeitsgruppe gehört  
  
1. Wenn Ihr Computer nicht zu einer Domäne gehört, deaktivieren Sie die Transportsicherheit, indem Sie den Authentifizierungsmodus und die Schutzebene auf "None" festlegen, wie in der folgenden Beispielkonfiguration gezeigt.  
  
    ```xml  
    <bindings>  
        <netMsmqBinding>  
            <binding configurationName="TransactedBinding">  
                <security mode="None"/>  
            </binding>  
        </netMsmqBinding>  
    </bindings>  
    ```  
  
2. Ändern Sie die Konfiguration sowohl auf dem Server als auch auf dem Client, bevor Sie das Beispiel ausführen.  
  
    > [!NOTE]
    >  Das Festlegen von `security mode` auf `None` entspricht dem Festlegen von `MsmqAuthenticationMode`, `MsmqProtectionLevel` und der `Message`-Sicherheit auf `None`.  
  
3. Um die Aktivierung auf einem Computer zu ermöglichen, der zu einer Arbeitsgruppe gehört, müssen sowohl der Aktivierungsdienst als auch der Arbeitsprozess mit einem spezifischen Benutzerkonto ausgeführt werden (das gleiche Konto für beide), und die Warteschlange muss über ACLs für das spezifische Benutzerkonto verfügen.  
  
     So ändern Sie die Identität, unter der der Arbeitsprozess ausgeführt wird:  
  
    1.  Führen Sie "Inetmgr.exe" aus.  
  
    2.  Klicken Sie unter **Anwendungspools**, mit der rechten Maustaste die **AppPool** (in der Regel **DefaultAppPool**), und wählen Sie **Anwendungspoolstandardwerte festlegen...** .  
  
    3.  Ändern Sie die Identitätseigenschaften, um das bestimmte Benutzerkonto zu verwenden.  
  
     So ändern Sie die Identität, unter der der Aktivierungsdienst ausgeführt wird:  
  
    1.  Führen Sie "Services.msc" aus.  
  
    2.  Mit der rechten Maustaste die **Net.MsmqListener-Adapter**, und wählen Sie **Eigenschaften**.  
  
4. Ändern Sie das Konto in der **Anmeldung** Registerkarte.  
  
5. In der Arbeitsgruppe muss der Dienst auch mit einem uneingeschränkten Token ausgeführt werden. Führen Sie hierzu in einem Befehlsfenster Folgendes aus:  
  
    ```console  
    sc sidtype netmsmqactivator unrestricted  
    ```  
  
## <a name="see-also"></a>Siehe auch

- [AppFabric-Hosting- und -Persistenzbeispiele](https://go.microsoft.com/fwlink/?LinkId=193961)
