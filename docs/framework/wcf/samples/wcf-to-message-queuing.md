---
title: Windows Communication Foundation zu Message Queuing
ms.date: 03/30/2017
ms.assetid: 78d0d0c9-648e-4d4a-8f0a-14d9cafeead9
ms.openlocfilehash: 1551ab407049e871a9275d148b1c84dc2791ccad
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59343384"
---
# <a name="windows-communication-foundation-to-message-queuing"></a>Windows Communication Foundation zu Message Queuing
In diesem Beispiel wird veranschaulicht, wie eine Windows Communication Foundation (WCF)-Anwendung eine Nachricht zu einer Message Queuing (MSMQ)-Anwendung senden kann. Der Dienst ist eine selbst gehostete Konsolenanwendung, die es Ihnen ermöglicht, den Dienst beim Empfang von Nachrichten in der Warteschlange zu beobachten. Der Dienst und der Client müssen dazu nicht gleichzeitig ausgeführt werden.

 Der Dienst empfängt Nachrichten von der Warteschlange und verarbeitet Bestellungen. Der Dienst erstellt eine Transaktionswarteschlage und richtet einen "Nachricht empfangen"-Nachrichtenhandler ein, wie im folgenden Beispielcode gezeigt.

```csharp
static void Main(string[] args)
{
    if (!MessageQueue.Exists(
              ConfigurationManager.AppSettings["queueName"]))
       MessageQueue.Create(
           ConfigurationManager.AppSettings["queueName"], true);
        //Connect to the queue
        MessageQueue Queue = new
    MessageQueue(ConfigurationManager.AppSettings["queueName"]);
    Queue.ReceiveCompleted +=
                 new ReceiveCompletedEventHandler(ProcessOrder);
    Queue.BeginReceive();
    Console.WriteLine("Order Service is running");
    Console.ReadLine();
}
```

 Wenn die Nachricht in der Warteschlange empfangen wird, wird der Nachrichtenhandler `ProcessOrder` aufgerufen.

```csharp
public static void ProcessOrder(Object source,
    ReceiveCompletedEventArgs asyncResult)
{
    try
    {
        // Connect to the queue.
        MessageQueue Queue = (MessageQueue)source;
        // End the asynchronous receive operation.
        System.Messaging.Message msg =
                     Queue.EndReceive(asyncResult.AsyncResult);
        msg.Formatter = new System.Messaging.XmlMessageFormatter(
                                new Type[] { typeof(PurchaseOrder) });
        PurchaseOrder po = (PurchaseOrder) msg.Body;
        Random statusIndexer = new Random();
        po.Status = PurchaseOrder.OrderStates[statusIndexer.Next(3)];
        Console.WriteLine("Processing {0} ", po);
        Queue.BeginReceive();
    }
    catch (System.Exception ex)
    {
        Console.WriteLine(ex.Message);
    }

}
```

 Der Dienst extrahiert die `ProcessOrder` aus dem MSMQ-Nachrichtentext heraus und verarbeitet die Bestellung.

 Der Name der MSMQ-Warteschlange wird im appSettings-Abschnitt der Konfigurationsdatei angegeben, wie in der folgenden Beispielkonfiguration gezeigt.

```xml
<appSettings>
    <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>
```

> [!NOTE]
>  Im Warteschlangennamen wird ein Punkt (.) für den lokalen Computer verwendet, und in der Pfadangabe werden umgekehrte Schrägstriche als Trennzeichen verwendet.

 Der Client erstellt eine Bestellung und reicht die Bestellung im Geltungsbereich der Transaktion ein, wie im folgenden Beispielcode gezeigt.

```csharp
// Create the purchase order
PurchaseOrder po = new PurchaseOrder();
// Fill in the details
...

OrderProcessorClient client = new OrderProcessorClient("OrderResponseEndpoint");

MsmqMessage<PurchaseOrder> ordermsg = new MsmqMessage<PurchaseOrder>(po);
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{
    client.SubmitPurchaseOrder(ordermsg);
    scope.Complete();
}
Console.WriteLine("Order has been submitted:{0}", po);

//Closing the client gracefully closes the connection and cleans up resources
client.Close();
```

 Der Client verwendet einen benutzerdefinierten Client zum Senden der MSMQ-Nachricht an die Warteschlange. Da die Anwendung, die empfangen und verarbeitet die Nachricht eine MSMQ-Anwendung und kein WCF-Anwendung ist, besteht kein implizierter Dienstvertrag zwischen den beiden Anwendungen. Deshalb kann in diesem Szenario kein Proxy mit dem Tool "Svcutil.exe" erstellt werden.

 Der benutzerdefinierte Client ist im Wesentlichen identisch für alle WCF-Anwendungen, mit denen die `MsmqIntegration` -Bindung zum Senden von Nachrichten. Im Gegensatz zu anderen Clients enthält dieser keinen Bereich von Dienstvorgängen. Es ist nur ein Sende-Nachricht-Vorgang.

```csharp
[System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}

public partial class OrderProcessorClient : System.ServiceModel.ClientBase<IOrderProcessor>, IOrderProcessor
{
    public OrderProcessorClient(){}

    public OrderProcessorClient(string configurationName)
        : base(configurationName)
    { }

    public OrderProcessorClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress address)
        : base(binding, address)
    { }

    public void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg)
    {
        base.Channel.SubmitPurchaseOrder(msg);
    }
}
```

 Wenn Sie das Beispiel ausführen, werden die Client- und Dienstaktivitäten sowohl im Dienst- als auch im Clientkonsolenfenster angezeigt. Sie können sehen, wie der Dienst Nachrichten vom Client empfängt. Drücken Sie die EINGABETASTE in den einzelnen Konsolenfenstern, um den Dienst und den Client zu schließen. Beachten Sie, dass aufgrund der Verwendung einer Warteschlange der Client und der Dienst nicht gleichzeitig ausgeführt werden müssen. Sie können beispielsweise den Client ausführen, ihn schließen und anschließend den Dienst starten, der dann trotzdem noch die Nachrichten des Clients empfängt.

> [!NOTE]
>  Dieses Beispiel erfordert die Installation von Message Queuing. Zeigen Sie die installationsanweisungen in [Message Queuing-](https://go.microsoft.com/fwlink/?LinkId=94968).  
  
### <a name="to-setup-build-and-run-the-sample"></a>So richten Sie das Beispiel ein, erstellen es und führen es aus  
  
1. Stellen Sie sicher, dass Sie ausgeführt haben die [Schritte der Einrichtung einmaligen Setupverfahren für Windows Communication Foundation-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Wenn der Dienst zuerst ausgeführt wird, wird überprüft, ob die Warteschlange vorhanden ist. Ist die Warteschlange nicht vorhanden, wird sie vom Dienst erstellt. Sie können zuerst den Dienst ausführen, um die Warteschlange zu erstellen, oder Sie können sie über den MSMQ-Warteschlangen-Manager erstellen. Führen Sie zum Erstellen einer Warteschlange in Windows 2008 die folgenden Schritte aus:  
  
    1.  Öffnen Sie Server-Manager in Visual Studio 2012.  
  
    2.  Erweitern Sie die **Features** Registerkarte.  
  
    3.  Mit der rechten Maustaste **Private Meldungswarteschlangen**, und wählen Sie **neu**, **Private Warteschlange**.  
  
    4.  Überprüfen Sie die **transaktional** Feld.  
  
    5.  Geben Sie `ServiceModelSamplesTransacted` als Name der neuen Warteschlange.  
  
3. Um die C#- oder Visual Basic .NET-Edition der Projektmappe zu erstellen, befolgen Sie die unter [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)aufgeführten Anweisungen.  
  
4. Folgen Sie den Anweisungen, um das Beispiel in einer Konfiguration für die einzelnen Computer ausführen, [Ausführen der Windows Communication Foundation-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
### <a name="to-run-the-sample-across-computers"></a>So führen Sie das Beispiel computerübergreifend aus  
  
1. Kopieren Sie die Dienstprogrammdateien aus dem Ordner \service\bin\ (unterhalb des sprachspezifischen Ordners) auf den Dienstcomputer.  
  
2. Kopieren Sie die Clientprogrammdateien aus dem Ordner \client\bin\ (unterhalb des sprachspezifischen Ordners) auf den Clientcomputer.  
  
3. Ändern Sie in der Datei Client.exe.config die Clientendpunktadresse, und geben Sie anstelle von "." den Namen des Dienstcomputers an.  
  
4. Starten Sie auf dem Dienstcomputer Service.exe an einer Eingabeaufforderung.  
  
5. Starten Sie auf dem Clientcomputer Client.exe an einer Eingabeaufforderung.  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert. Suchen Sie nach dem folgenden Verzeichnis (Standardverzeichnis), bevor Sie fortfahren.  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, fahren Sie mit [Windows Communication Foundation (WCF) und Windows Workflow Foundation (WF) Samples für .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) alle Windows Communication Foundation (WCF) herunterladen und [!INCLUDE[wf1](../../../../includes/wf1-md.md)] Beispiele. Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\WcfToMsmq`  
  
## <a name="see-also"></a>Siehe auch

- [Vorgehensweise: Nachrichtenaustausch mit WCF-Endpunkten und Message Queuing-Anwendungen](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [Message Queuing](https://go.microsoft.com/fwlink/?LinkId=94968)
