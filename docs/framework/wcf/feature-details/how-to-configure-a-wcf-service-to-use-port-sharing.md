---
title: 'Vorgehensweise: Konfigurieren Sie einen Windows Communication Foundation-Dienstes zur Anschlussfreigabe'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6400bc71-a858-4ac2-8d5a-caa72d3b5482
ms.openlocfilehash: bc0c822659ee57ac8dd87a2adddcd32e934ea4fb
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59302393"
---
# <a name="how-to-configure-a-windows-communication-foundation-service-to-use-port-sharing"></a>Vorgehensweise: Konfigurieren Sie einen Windows Communication Foundation-Dienstes zur Anschlussfreigabe
Die einfachste Möglichkeit zur Verwendung von net.tcp://-Anschlussfreigabe in der Windows Communication Foundation (WCF)-Anwendung wird zum Bereitstellen eines Diensts mithilfe der <xref:System.ServiceModel.NetTcpBinding>.  
  
 Diese Bindung verfügt über eine <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A>-Eigenschaft, die festlegt, ob die net.tcp://-Anschlussfreigabe für den mit dieser Bindung zu konfigurierenden Dienst aktiviert ist.  
  
 Der folgende Vorgang zeigt, wie Sie die <xref:System.ServiceModel.NetTcpBinding>-Klasse zum Öffnen eines Endpunkts am Uniform Resource Identifier (URI) net.tcp://localhost/MyService verwenden können, zuerst im Code und dann mithilfe von Konfigurationselementen.  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-code"></a>So aktivieren Sie die net.tcp://-Anschlussfreigabe für eine NetTcpBinding im Code  
  
1. Erstellen Sie einen Dienst zum Implementieren der eines Vertrags mit dem Namen `IMyService` und nennen Sie sie `MyService`,.  
  
     [!code-csharp[c_ConfigurePortSharing#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#1)]
     [!code-vb[c_ConfigurePortSharing#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#1)]  
  
2. Erstellen Sie eine Instanz der <xref:System.ServiceModel.NetTcpBinding>-Klasse, und legen Sie die <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A>-Eigenschaft auf `true` fest.  
  
     [!code-csharp[c_ConfigurePortSharing#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#2)]
     [!code-vb[c_ConfigurePortSharing#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#2)]  
  
3. Erstellen Sie einen <xref:System.ServiceModel.ServiceHost>, und fügen Sie ihm den Dienstendpunkt für `MyService` hinzu, der die <xref:System.ServiceModel.NetTcpBinding>, für die die Anschlussfreigabe aktiviert wurde, verwendet und der am Endpunkt-Adress-URI "net.tcp://localhost/MyService" eine Abhörung durchführt.  
  
     [!code-csharp[c_ConfigurePortSharing#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#3)]
     [!code-vb[c_ConfigurePortSharing#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#3)]  
  
    > [!NOTE]
    >  In diesem Beispiel wird der Standard-TCP-Anschluss 808 verwendet, da der Endpunkt-Adress-URI keine andere Anschlussnummer angibt. Da die Anschlussfreigabe explizit für die Transportbindung aktiviert wurde, kann dieser Dienst den Anschluss 808 für andere Dienste in anderen Vorgängen freigeben. Wenn die Anschlussfreigabe nicht zulässig wäre und eine andere Anwendung bereits den Anschluss 808 verwenden würde, würde dieser Dienst beim Öffnen eine <xref:System.ServiceModel.AddressAlreadyInUseException> ausgeben.  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-configuration"></a>So aktivieren Sie die net.tcp://-Anschlussfreigabe für eine NetTcpBinding in der Konfiguration  
  
1. Das folgende Beispiel zeigt, wie Sie die Anschlussfreigabe aktivieren und den Dienstendpunkt mithilfe von Konfigurationselementen hinzufügen können.  
  
```xml  
<system.serviceModel>  
  <bindings>  
    <netTcpBinding name="portSharingBinding"   
                   portSharingEnabled="true" />  
  </bindings>  
  <services>  
    <service name="MyService">  
        <endpoint address="net.tcp://localhost/MyService"  
                  binding="netTcpBinding"  
                  contract="IMyService"  
                  bindingConfiguration="portSharingBinding" />  
    </service>  
  </services>  
</system.serviceModel>  
```  
  
## <a name="see-also"></a>Siehe auch

- [Net.TCP-Anschlussfreigabe](../../../../docs/framework/wcf/feature-details/net-tcp-port-sharing.md)
- [Vorgehensweise: Aktivieren des Net.TCP-Portfreigabediensts](../../../../docs/framework/wcf/feature-details/how-to-enable-the-net-tcp-port-sharing-service.md)
