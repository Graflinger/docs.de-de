---
title: 'Vorgehensweise: Programmgesteuertes Hinzufügen der Ermittelbarkeit zu einem WCF-Dienst und -Client'
ms.date: 03/30/2017
ms.assetid: 4f7ae7ab-6fc8-4769-9730-c14d43f7b9b1
ms.openlocfilehash: 54d838967fcc19501ff7385aba29e8d79025ce70
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59336589"
---
# <a name="how-to-programmatically-add-discoverability-to-a-wcf-service-and-client"></a>Vorgehensweise: Programmgesteuertes Hinzufügen der Ermittelbarkeit zu einem WCF-Dienst und -Client
In diesem Thema wird erläutert, wie Sie einen Windows Communication Foundation (WCF)-Dienst erkennbar zu machen. Es basiert auf der [Selbsthosting](https://go.microsoft.com/fwlink/?LinkId=145523) Beispiel.  
  
### <a name="to-configure-the-existing-self-host-service-sample-for-discovery"></a>So konfigurieren Sie das vorhandene Beispiel unter "Selbst gehostete Dienste" für die Suche  
  
1. Öffnen Sie die Self-Hosting-Projektmappe in Visual Studio 2012. Das Beispiel befindet sich im Verzeichnis "TechnologySamples\Basic\Service\Hosting\SelfHost".  
  
2. Fügen Sie dem Dienstprojekt einen Verweis auf `System.ServiceModel.Discovery.dll` hinzu. Sie möglicherweise eine Fehlermeldung angezeigt "System. ServiceModel.Discovery.dll oder eine ihrer Abhängigkeiten erfordert eine höhere Version von der [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] als die im Projekt... " Wenn diese Meldung angezeigt wird, mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen Sie **Eigenschaften**. In der **Projekteigenschaften** Fenster, stellen Sie sicher, dass die **Zielframework** ist [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)].  
  
3. Öffnen Sie die Datei "Service.cs", und fügen Sie die folgende `using`-Anweisung hinzu.  
  
    ```csharp  
    using System.ServiceModel.Discovery;  
    ```  
  
4. Fügen Sie in der `Main()`-Methode innerhalb der `using`-Anweisung dem Diensthost eine <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>-Instanz hinzu.  
  
    ```csharp  
    public static void Main()  
    {  
        // Create a ServiceHost for the CalculatorService type.  
        using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
        {  
            // Add a ServiceDiscoveryBehavior  
            serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());                  
  
            // ...  
        }  
    }  
    ```  
  
     Das <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>-Objekt gibt an, dass der Dienst, auf den dieses Verhalten angewendet wird, erkennbar ist.  
  
5. Fügen Sie dem Diensthost direkt nach dem Code, in dem <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> hinzugefügt wird, ein <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior>-Objekt hinzu.  
  
    ```csharp  
    // Add ServiceDiscoveryBehavior  
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
  
    // Add a UdpDiscoveryEndpoint  
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
    ```  
  
     Dieser Code gibt an, dass Suchmeldungen an den standardmäßigen UDP-Suchendpunkt gesendet werden sollen.  
  
### <a name="to-create-a-client-application-that-uses-discovery-to-call-the-service"></a>So erstellen Sie eine Clientanwendung, die die Suche zum Aufrufen des Diensts verwendet  
  
1. Fügen Sie der Projektmappe eine neue Konsolenanwendung mit dem Namen `DiscoveryClientApp` hinzu.  
  
2. Hinzufügen eines Verweises auf `System.ServiceModel.dll` und `System.ServiceModel.Discovery.dll`  
  
3. Kopieren Sie die Dateien "GeneratedClient.cs" und "App.config" aus dem vorhandenen Clientprojekt in das neue DiscoveryClientApp-Projekt. Dazu, mit der Maustaste der Dateien in die **Projektmappen-Explorer**Option **Kopie**, und wählen Sie dann die **DiscoveryClientApp** -Projekts mit der rechten Maustaste, und wählen Sie **Einfügen**.  
  
4. Öffnen Sie die Datei Program.cs.  
  
5. Fügen Sie die folgenden `using`-Anweisungen hinzu.  
  
    ```csharp  
    using System.ServiceModel;  
    using System.ServiceModel.Discovery;  
    using Microsoft.ServiceModel.Samples;  
    ```  
  
6. Fügen Sie der `FindCalculatorServiceAddress()`-Klasse eine statische Methode mit dem Namen `Program` hinzu.  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
    }  
    ```  
  
     Diese Methode verwendet die Suche (Discovery) zum Suchen nach dem `CalculatorService`-Dienst.  
  
7. Erstellen Sie in der `FindCalculatorServiceAddress`-Methode eine neue <xref:System.ServiceModel.Discovery.DiscoveryClient>-Instanz, die einen <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> an den Konstruktor übergibt.  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
        // Create DiscoveryClient  
        DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
    }  
    ```  
  
     Diese teilt WCF, die die <xref:System.ServiceModel.Discovery.DiscoveryClient> Klasse sollte den standardmäßigen UDP-suchendpunkt zum Senden und Empfangen von Suchmeldungen verwenden.  
  
8. Rufen Sie in der nächsten Zeile die <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A>-Methode auf, und geben Sie eine <xref:System.ServiceModel.Discovery.FindCriteria>-Instanz an, die den zu suchenden Dienstvertrag enthält. Geben Sie in diesem Fall `ICalculator` an.  
  
    ```csharp  
    // Find ICalculatorService endpoints              
    FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
    ```  
  
9. Überprüfen Sie nach dem Aufruf von <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A>, ob mindestens ein übereinstimmender Dienst vorhanden ist, und geben Sie für den ersten übereinstimmenden Dienst <xref:System.ServiceModel.EndpointAddress> zurück. Geben Sie andernfalls `null` zurück.  
  
    ```csharp  
    if (findResponse.Endpoints.Count > 0)  
    {  
        return findResponse.Endpoints[0].Address;  
    }  
    else  
    {  
        return null;  
    }  
    ```  
  
10. Fügen Sie der `InvokeCalculatorService`-Klasse eine statische Methode mit dem Namen `Program` hinzu.  
  
    ```csharp  
    static void InvokeCalculatorService(EndpointAddress endpointAddress)  
    {  
    }  
    ```  
  
     Diese Methode verwendet die von `FindCalculatorServiceAddress` zurückgegebene Endpunktadresse zum Aufrufen des Rechnerdiensts.  
  
11. Erstellen Sie innerhalb der `InvokeCalculatorService`-Methode eine Instanz der `CalculatorServiceClient`-Klasse. Diese Klasse wird definiert, durch die [Selbsthosting](https://go.microsoft.com/fwlink/?LinkId=145523) Beispiel. Sie wurde mithilfe von "Svcutil.exe" generiert.  
  
    ```csharp  
    // Create a client  
    CalculatorClient client = new CalculatorClient();  
    ```  
  
12. Legen Sie in der nächsten Zeile die Endpunktadresse des Clients auf die Endpunktadresse fest, die von `FindCalculatorServiceAddress()` zurückgegeben wurde.  
  
    ```csharp  
    // Connect to the discovered service endpoint  
    client.Endpoint.Address = endpointAddress;  
    ```  
  
13. Rufen Sie direkt nach dem Code für den vorherigen Schritt die vom Rechnerdienst verfügbar gemachten Methoden auf.  
  
    ```csharp  
    Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
  
    // Call the Add service operation.  
    double result = client.Add(value1, value2);  
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Subtract service operation.  
    result = client.Subtract(value1, value2);  
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Multiply service operation.  
    result = client.Multiply(value1, value2);  
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Divide service operation.  
    result = client.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
    Console.WriteLine();  
  
    //Closing the client gracefully closes the connection and cleans up resources  
    client.Close();  
    ```  
  
14. Fügen Sie der `Main()`-Methode in der `Program`-Klasse Code zum Aufrufen von `FindCalculatorServiceAddress` hinzu.  
  
    ```csharp  
    public static void Main()  
    {  
        EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
    }  
    ```  
  
15. Rufen Sie in der nächsten Zeile `InvokeCalculatorService()` auf, und übergeben Sie die Endpunktadresse, die von `FindCalculatorServiceAddress()` zurückgegeben wurde.  
  
    ```csharp  
    if (endpointAddress != null)  
    {  
        InvokeCalculatorService(endpointAddress);  
    }  
  
    Console.WriteLine("Press <ENTER> to exit.");  
    Console.ReadLine();  
    ```  
  
### <a name="to-test-the-application"></a>So testen Sie die Anwendung  
  
1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechte, und führen Sie "Service.exe" aus.  
  
2. Öffnen Sie eine Eingabeaufforderung, und führen Sie "Discoveryclientapp.exe" aus.  
  
3. Die Ausgabe der Datei "service.exe" sollte der folgenden Ausgabe ähneln.  
  
    ```Output  
    Received Add(100,15.99)  
    Return: 115.99  
    Received Subtract(100,15.99)  
    Return: 84.01  
    Received Multiply(100,15.99)  
    Return: 1599  
    Received Divide(100,15.99)  
    Return: 6.25390869293308  
    ```  
  
4. Die Ausgabe der Datei "Discoveryclientapp.exe" sollte der folgenden Ausgabe ähneln.  
  
    ```Output  
    Invoking CalculatorService at http://localhost:8000/ServiceModelSamples/service  
    Add(100,15.99) = 115.99  
    Subtract(100,15.99) = 84.01  
    Multiply(100,15.99) = 1599  
    Divide(100,15.99) = 6.25390869293308  
  
    Press <ENTER> to exit.  
    ```  
  
## <a name="example"></a>Beispiel  
 Es folgt eine Auflistung des Codes für dieses Beispiel. Da dieser Code basiert die [Selbsthosting](https://go.microsoft.com/fwlink/?LinkId=145523) Beispiel nur die geänderten Dateien werden aufgeführt. Weitere Informationen über das Self-Hosting-Beispiel finden Sie unter [Setupanweisungen](https://go.microsoft.com/fwlink/?LinkId=145522).  
  
```csharp  
// Service.cs  
using System;  
using System.Configuration;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
  
namespace Microsoft.ServiceModel.Samples  
{  
    // See SelfHost sample for service contract and implementation  
    // ...  
  
        // Host the service within this EXE console application.  
        public static void Main()  
        {  
            // Create a ServiceHost for the CalculatorService type.  
            using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
            {  
                // Add the ServiceDiscoveryBehavior to make the service discoverable  
                serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
                serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
  
                // Open the ServiceHost to create listeners and start listening for messages.  
                serviceHost.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
            }  
        }  
    }  
}  
```  
  
```csharp  
// Program.cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
using Microsoft.ServiceModel.Samples;  
using System.Text;  
  
namespace DiscoveryClientApp  
{  
    class Program  
    {  
        static EndpointAddress FindCalculatorServiceAddress()  
        {  
            // Create DiscoveryClient  
            DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
            // Find ICalculatorService endpoints              
            FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
  
            if (findResponse.Endpoints.Count > 0)  
            {  
                return findResponse.Endpoints[0].Address;  
            }  
            else  
            {  
                return null;  
            }  
        }  
  
        static void InvokeCalculatorService(EndpointAddress endpointAddress)  
        {  
            // Create a client  
            CalculatorClient client = new CalculatorClient();  
  
            // Connect to the discovered service endpoint  
            client.Endpoint.Address = endpointAddress;  
  
            Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
            double value1 = 100.00D;  
            double value2 = 15.99D;  
  
            // Call the Add service operation.  
            double result = client.Add(value1, value2);  
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Subtract service operation.  
            result = client.Subtract(value1, value2);  
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Multiply service operation.  
            result = client.Multiply(value1, value2);  
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Divide service operation.  
            result = client.Divide(value1, value2);  
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
            Console.WriteLine();  
  
            //Closing the client gracefully closes the connection and cleans up resources  
            client.Close();  
        }  
        static void Main(string[] args)  
        {  
            EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
  
            if (endpointAddress != null)  
            {  
                InvokeCalculatorService(endpointAddress);  
            }  
  
            Console.WriteLine("Press <ENTER> to exit.");  
            Console.ReadLine();  
  
        }  
    }  
}  
```  

## <a name="see-also"></a>Siehe auch

- [Übersicht über die WCF-Suche](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)
- [Objektmodell der WCF-Suche](../../../../docs/framework/wcf/feature-details/wcf-discovery-object-model.md)
