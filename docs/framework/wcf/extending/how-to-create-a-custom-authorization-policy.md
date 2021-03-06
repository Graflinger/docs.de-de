---
title: 'Vorgehensweise: Erstellen einer benutzerdefinierten Autorisierungsrichtlinie'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 05b0549b-882d-4660-b6f0-5678543e5475
ms.openlocfilehash: 05130e809356369ee2b43d9af86acf69fe527e9a
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59295418"
---
# <a name="how-to-create-a-custom-authorization-policy"></a>Vorgehensweise: Erstellen einer benutzerdefinierten Autorisierungsrichtlinie
Die identitätsmodellinfrastruktur in Windows Communication Foundation (WCF) unterstützt ein anspruchsbasiertes Autorisierungsmodell. Ansprüche werden aus Token extrahiert, wahlweise von der benutzerdefinierten Autorisierungsrichtlinie verarbeitet und in einen <xref:System.IdentityModel.Policy.AuthorizationContext> platziert, der dann untersucht werden kann, um Autorisierungsentscheidungen zu treffen. Eine benutzerdefinierte Richtlinie kann zum Transformieren von Ansprüchen von eingehenden Token in von der Anwendung erwartete Ansprüche verwendet werden. Auf diese Weise kann die Anwendungsschicht gegenüber Details zu den verschiedenen Ansprüchen durch die verschiedenen Tokentypen, die weiterhin WCF unterstützt isoliert sein. In diesem Thema wird gezeigt, wie eine benutzerdefinierte Autorisierungsrichtlinie implementiert und wie diese Richtlinie einer Sammlung von Richtlinien, die von einem Dienst verwendet werden, hinzugefügt wird.  
  
### <a name="to-implement-a-custom-authorization-policy"></a>So implementieren Sie eine benutzerdefinierte Autorisierungsrichtlinie  
  
1. Definieren Sie eine neue Klasse, die von <xref:System.IdentityModel.Policy.IAuthorizationPolicy> abgeleitet wird.  
  
2. Implementieren Sie die schreibgeschützte <xref:System.IdentityModel.Policy.IAuthorizationComponent.Id%2A>-Eigenschaft, indem Sie eine eindeutige Zeichenfolge im Konstruktor für die Klasse generieren und diese Zeichenfolge bei jedem Zugriff auf die Eigenschaft zurückgeben.  
  
3. Implementieren Sie die schreibgeschützte <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Issuer%2A>-Eigenschaft, indem Sie einen <xref:System.IdentityModel.Claims.ClaimSet> zurückgeben, der den Richtlinienaussteller darstellt. Dies könnte ein `ClaimSet` sein, der die Anwendung darstellt oder einen integrierten `ClaimSet` (z. B. den `ClaimSet`, der von der statischen <xref:System.IdentityModel.Claims.ClaimSet.System%2A>-Eigenschaft zurückgegeben wurde).  
  
4. Implementieren Sie die <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29>-Methode, wie in der folgenden Prozedur beschrieben.  
  
### <a name="to-implement-the-evaluate-method"></a>So implementieren Sie die Evaluate-Methode  
  
1. Zwei Parameter werden an diese Methode übergeben: eine Instanz der <xref:System.IdentityModel.Policy.EvaluationContext>-Klasse und ein Objektverweis.  
  
2. Die benutzerdefinierte Autorisierungsrichtlinie fügt <xref:System.IdentityModel.Claims.ClaimSet> Instanzen unabhängig von den aktuellen Inhalt der <xref:System.IdentityModel.Policy.EvaluationContext>, dann fügen Sie jeden `ClaimSet` durch Aufrufen der <xref:System.IdentityModel.Policy.EvaluationContext.AddClaimSet%28System.IdentityModel.Policy.IAuthorizationPolicy%2CSystem.IdentityModel.Claims.ClaimSet%29> Methode, und geben `true` aus der <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%2A> Methode. Die Rückgabe von `true` zeigt der Autorisierungsinfrastruktur an, dass die Autorisierungsrichtlinie ihre Funktion erfüllt hat und nicht erneut aufgerufen werden muss.  
  
3. Wenn die benutzerdefinierte Autorisierungsrichtlinie Sätze von Ansprüchen nur hinzufügt, wenn bestimmte Ansprüche bereits im `EvaluationContext` vorhanden sind, suchen Sie diese Ansprüche, indem Sie die `ClaimSet`-Instanzen, die von der <xref:System.IdentityModel.Policy.EvaluationContext.ClaimSets%2A>-Eigenschaft zurückgegeben werden, analysieren. Wenn die Ansprüche vorhanden sind, fügen Sie den neuen Satz von Ansprüchen hinzu, indem Sie die <xref:System.IdentityModel.Policy.EvaluationContext.AddClaimSet%28System.IdentityModel.Policy.IAuthorizationPolicy%2CSystem.IdentityModel.Claims.ClaimSet%29>-Methode aufrufen, und geben Sie `true` zurück, wenn keine weiteren Sätze von Ansprüchen hinzugefügt werden müssen. Dies zeigt der Autorisierungsinfrastruktur an, dass die Autorisierungsrichtlinie ihre Funktion erfüllt hat. Wenn die Ansprüche nicht vorhanden sind, geben Sie `false` zurück. Dies gibt an, dass die Autorisierungsrichtlinie erneut aufgerufen werden muss, wenn andere Autorisierungsrichtlinien dem `EvaluationContext` weitere Sätze von Ansprüchen hinzufügen.  
  
4. In komplexeren Verarbeitungsszenarien wird der zweite Parameter der <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29>-Methode zum Speichern einer Zustandsvariablen verwendet, die von der Autorisierungsinfrastruktur bei jedem nachfolgenden Aufruf der <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29>-Methode für eine bestimmte Auswertung zurückgegeben wird.  
  
### <a name="to-specify-a-custom-authorization-policy-through-configuration"></a>So geben Sie eine benutzerdefinierte Autorisierungsrichtlinie durch Konfiguration an  
  
1. Geben Sie den Typ der benutzerdefinierten Autorisierungsrichtlinie im `policyType`-Attribut des `add`-Elements und des `authorizationPolicies`-Elements des `serviceAuthorization`-Elements an.  
  
    ```xml  
    <configuration>  
     <system.serviceModel>  
      <behaviors>  
        <serviceAuthorization serviceAuthorizationManagerType=  
                  "Samples.MyServiceAuthorizationManager" >  
          <authorizationPolicies>  
            <add policyType="Samples.MyAuthorizationPolicy" />  
          </authorizationPolicies>  
        </serviceAuthorization>  
      </behaviors>  
     </system.serviceModel>  
    </configuration>  
    ```  
  
### <a name="to-specify-a-custom-authorization-policy-through-code"></a>So geben Sie eine benutzerdefinierte Autorisierungsrichtlinie durch Code an  
  
1. Erstellen Sie eine <xref:System.Collections.Generic.List%601> von <xref:System.IdentityModel.Policy.IAuthorizationPolicy>.  
  
2. Erstellen Sie eine Instanz der benutzerdefinierten Autorisierungsrichtlinie.  
  
3. Fügen Sie der Liste die Autorisierungsrichtlinieninstanz hinzu.  
  
4. Wiederholen Sie die Schritte 2 und 3 für jede benutzerdefinierte Autorisierungsrichtlinie.  
  
5. Weisen Sie der <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ExternalAuthorizationPolicies%2A>-Eigenschaft eine schreibgeschützte Version der Liste zu.  
  
     [!code-csharp[c_CustomAuthPol#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthpol/cs/c_customauthpol.cs#8)]
     [!code-vb[c_CustomAuthPol#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthpol/vb/source.vb#8)]  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird eine vollständige Implementierung der <xref:System.IdentityModel.Policy.IAuthorizationPolicy> veranschaulicht.  
  
 [!code-csharp[c_CustomAuthPol#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customauthpol/cs/c_customauthpol.cs#5)]
 [!code-vb[c_CustomAuthPol#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customauthpol/vb/source.vb#5)]  
  
## <a name="see-also"></a>Siehe auch

- <xref:System.ServiceModel.ServiceAuthorizationManager>
- [Vorgehensweise: Vergleichen von Ansprüchen](../../../../docs/framework/wcf/extending/how-to-compare-claims.md)
- [Vorgehensweise: Erstellen eines benutzerdefinierten Autorisierungs-Managers für einen Dienst](../../../../docs/framework/wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md)
- [Autorisierungsrichtlinie](../../../../docs/framework/wcf/samples/authorization-policy.md)
