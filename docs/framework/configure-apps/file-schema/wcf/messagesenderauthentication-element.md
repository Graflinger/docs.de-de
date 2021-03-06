---
title: <messageSenderAuthentication> Element
ms.date: 03/30/2017
ms.assetid: 8d979dfc-a6f9-42ec-96d5-7fbc13a48118
ms.openlocfilehash: 410fffd541926b9a2e75c04d26a2a1e08a262939
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59135056"
---
# <a name="messagesenderauthentication-element"></a>\<MessageSenderAuthentication >-Element
Gibt die Authentifizierungsoptionen für Peer-to-Peer-Nachrichtenabsender an.  
  
 Weitere Informationen zur Peer-zu-Peer-Programmierung finden Sie unter [Peer-zu-Peer-Netzwerke](../../../../../docs/framework/wcf/feature-details/peer-to-peer-networking.md).  
  
 \<system.ServiceModel>  
\<behaviors>  
\<endpointBehaviors>  
\<behavior>  
\<clientCredentials>  
\<peer>  
\<messageSenderAuthentication>  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<messageSenderAuthentication customCertificateValidatorType= "namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                             certificateValidationMode = "ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                             revocationMode="NoCheck/Online/Offline"
                             trustedStoreLocation="CurrentUser/LocalMachine" />
```  
  
## <a name="attributes-and-elements"></a>Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute, untergeordnete Elemente sowie übergeordnete Elemente beschrieben.  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|  
|---------------|-----------------|  
|customCertificateValidatorType|Ein Typ und eine Assembly, die zum Überprüfen eines benutzerdefinierten Typs verwendet werden. Das Attribut muss festgelegt werden, wenn für `certificateValidationMode` der Wert `Custom` festgelegt wurde.|  
|certifcateValidationMode|Gibt einen der drei für die Überprüfung von Anmeldeinformationen verwendeten Modi an. Wenn dies auf `Custom` festgelegt wurde, muss auch ein `customCertificateValidator` bereitgestellt werden.|  
|revocationMode|Einer der Modi zum Prüfen auf eine Liste gesperrter Zertifikate.|  
|trustedStoreLocation|Einer der beiden Systemspeicherorte: `LocalMachine` oder `CurrentUser`. Dieser Wert wird verwendet, wenn ein Dienstzertifikat mit dem Client ausgehandelt wird. Validierung wird ausgeführt, für die **vertrauenswürdige Personen** am angegebenen Speicherort zu speichern.|  
  
## <a name="customcertificatevalidatortype-attribute"></a>customCertificateValidatorType-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|Zeichenfolge|Dies ist optional. Gibt den vollständigen Typnamen und die Assembly sowie weitere Daten zum Suchen des Typs an. Mindestens ein Namespace und Typname sind erforderlich. Zu den optionalen Informationen zählen: Assemblyname, Versionsnummer, Kultur und Token des öffentlichen Schlüssels.|  
  
## <a name="certificatevalidationmode-attribute"></a>certificateValidationMode-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|Enumeration|Dies ist optional. Einer der folgenden Werte: `None`, `PeerTrust`, `ChainTrust`, `PeerOrChainTrust`, `Custom`. Die Standardeinstellung ist `ChainTrust`. Die Standardeinstellung ist `ChainTrust`.<br /><br /> Weitere Informationen finden Sie unter [arbeiten mit Zertifikaten](../../../../../docs/framework/wcf/feature-details/working-with-certificates.md).|  
  
## <a name="revocationmode-attribute"></a>revocationMode-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|Enumeration|Einer der folgenden Werte: `NoCheck`, `Online`, `Offline`. Die Standardeinstellung ist `Online`.<br /><br /> Weitere Informationen finden Sie unter [arbeiten mit Zertifikaten](../../../../../docs/framework/wcf/feature-details/working-with-certificates.md).|  
  
## <a name="trustedstorelocation-attribute"></a>trustedStoreLocation-Attribut  
  
|Wert|Beschreibung|  
|-----------|-----------------|  
|Enumeration|Einer der folgenden Werte: `LocalMachine` oder `CurrentUser`. Die Standardeinstellung ist `CurrentUser`. Wenn die Clientanwendung über ein Systemkonto ausgeführt wird, befindet sich das Zertifikat in der Regel in `LocalMachine`. Wenn die Clientanwendung über ein Benutzerkonto ausgeführt wird, befindet sich das Zertifikat in der Regel in `CurrentUser`. Die Standardeinstellung ist `CurrentUser`.|  
  
### <a name="child-elements"></a>Untergeordnete Elemente  
 Keine  
  
### <a name="parent-elements"></a>Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|-----------------|  
|[\<peer>](../../../../../docs/framework/configure-apps/file-schema/wcf/peer-of-clientcredentials-element.md)|Gibt Anmeldeinformationen an, die zur Authentifizierung des Clients bei einem Peerdienst verwendet werden.|  
  
## <a name="remarks"></a>Hinweise  
 Dieses Element muss konfiguriert werden, wenn die Nachrichtenauthentifizierung ausgewählt wird. Für Ausgabekanäle wird jede Nachricht ist signiert unter Verwendung des Zertifikats von bereitgestellten [ \<Zertifikat >](../../../../../docs/framework/configure-apps/file-schema/wcf/certificate-element.md). Alle Nachrichten werden vor dem Zustellen zur Anwendung mithilfe des durch das `customCertificateValidatorType`-Attribut dieses Elements angegebenen Validierungssteuerelements mit den Nachrichtenanmeldeinformationen verglichen. Das Validierungssteuerelement kann die Anmeldeinformationen akzeptieren oder ablehnen.  
  
## <a name="example"></a>Beispiel  
 Mit dem folgenden Code wird der Validierungsmodus des Nachrichtenabsenders auf `PeerOrChainTrust` festgelegt.  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="MyEndpointBehavior">
      <clientCredentials>
        <peer>
          <certificate findValue="www.contoso.com"
                       storeLocation="LocalMachine"
                       x509FindType="FindByIssuerName" />
          <messageSenderAuthentication certificateValidationMode="PeerOrChainTrust" />
          <messageSenderAuthentication certificateValidationMode="None" />
        </peer>
      </clientCredentials>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>Siehe auch

- <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>
- <xref:System.ServiceModel.Security.PeerCredential.MessageSenderAuthentication%2A>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement.MessageSenderAuthentication%2A>
- <xref:System.ServiceModel.Configuration.X509PeerCertificateAuthenticationElement>
- [Verwenden von Zertifikaten](../../../../../docs/framework/wcf/feature-details/working-with-certificates.md)
- [Peer-to-Peer-Netzwerke](../../../../../docs/framework/wcf/feature-details/peer-to-peer-networking.md)
- [Peerkanal-Nachrichtenauthentifizierung](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [Benutzerdefinierte Peerkanal-Authentifizierung](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [Sichern von Peerkanalanwendungen](../../../../../docs/framework/wcf/feature-details/securing-peer-channel-applications.md)
