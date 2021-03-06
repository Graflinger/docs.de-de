---
title: Transportkontingente
ms.date: 03/30/2017
helpviewer_keywords:
- transport quotas [WCF]
ms.assetid: 3e71dd3d-f981-4d9c-9c06-ff8abb61b717
ms.openlocfilehash: 12c266a473aa7c20ab35d6047fedafbfa04dac4a
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59299305"
---
# <a name="transport-quotas"></a>Transportkontingente
Transportkontingente sind ein Richtlinienmechanismus für die Entscheidung, wann eine Verbindung übermäßige Ressourcen belegt. Ein Kontingent ist eine harte Grenze, die eine Nutzung zusätzlicher Ressourcen nach Überschreiten des Kontingentwerts verhindert. Transportkontingente verhindern entweder böswillige oder unbeabsichtigte Denial-of-Service-Angriffe.  
  
 Windows Communication Foundation (WCF)-Transporte haben Standardkontingentwerte, die auf einer konservativen speicherbelegung von Ressourcen basieren. Diese Standardwerte sind für eine Entwicklungsumgebung und für kleine Installationsszenarien geeignet. Dienstadministratoren sollten Transportkontingente prüfen und individuelle Kontingentwerte anpassen, wenn einer Installation die Ressourcen ausgehen oder wenn Verbindungen eingeschränkt werden, obwohl zusätzliche Ressourcen zur Verfügung stehen.  
  
## <a name="types-of-transport-quotas"></a>Typen von Transportkontingenten  
 WCF-Transporte haben drei Typen von Kontingenten:  
  
-   *Timeouts* mindern Denial of Service-Angriffe, die abhängig sind, auf die Ressourcen für längere Zeit binden.  
  
-   *Zuordnung des Arbeitsspeicherlimits* zu verhindern, dass eine einzelne Verbindung Systemspeicher für schwellenwertbenachrichtigungen und Dienst zu anderen Verbindungen verweigert.  
  
-   *Grenzwerte für sammlungsgröße* begrenzen den Verbrauch von Ressourcen, die Speicher indirekt Belegen oder beschränkt zur Verfügung sind.  
  
## <a name="transport-quota-descriptions"></a>Transportkontingentbeschreibungen  
 In diesem Abschnitt werden die transportkontingente beschrieben für die standard-WCF-Transporte zur Verfügung: HTTP (S), TCP/IP und named Pipes. Benutzerdefinierte Transporte können eigene konfigurierbare Kontingente aufweisen, die nicht in dieser Liste enthalten sind. Weitere Informationen über diese Kontingente finden Sie in der Dokumentation zum benutzerdefinierten Transport.  
  
 Jede Kontingenteinstellung verfügt über einen Typ, einen minimalen Wert und einen Standardwert. Der Maximalwert eines Kontingents wird von seinem Typ beschränkt. Aufgrund von Computereinschränkungen ist es nicht immer möglich, ein Kontingent auf seinen Maximalwert festzulegen.  
  
|Name|Typ|Min.-<br /><br /> Wert|Standard<br /><br /> Wert|Beschreibung|  
|----------|----------|--------------------|-----------------------|-----------------|  
|`ChannelInitializationTimeout`|TimeSpan|1 Teilstrich|5 Sek.|Maximale Zeit für das Warten darauf, dass eine Verbindung die Präambel während des anfänglichen Lesens sendet. Diese Daten werden empfangen, bevor die Authentifizierung eintritt. Diese Einstellung ist im Allgemeinen viel kleiner als der `ReceiveTimeout`-Kontingentwert.|  
|`CloseTimeout`|TimeSpan|0|1 Min.|Maximale Zeit für das Warten darauf, dass eine Verbindung geschlossen wird, bevor der Transport eine Ausnahme auslöst.|  
|`ConnectionBufferSize`|Ganze Zahl|1|8 kB|Größe der Übertragungs- und Empfangspuffer des zugrunde liegenden Transports, in Bytes,. Eine Erhöhung der Puffergröße kann beim Senden großer Nachrichten den Durchsatz erhöhen.|  
|`IdleTimeout`|TimeSpan|0|2 Min.|Maximale Zeit, die eine zusammengeführte Verbindung im Leerlauf bleiben kann, bevor sie geschlossen wird.<br /><br /> Diese Einstellung gilt nur für zusammengeführte Verbindungen.|  
|`LeaseTimeout`|TimeSpan|0|5 Min.|Maximale Lebensdauer einer aktiven zusammengeführten Verbindung. Nachdem die angegebene Zeit verstrichen ist, schließt die Verbindung, sobald die aktuelle Anforderung verarbeitet ist.<br /><br /> Diese Einstellung gilt nur für zusammengeführte Verbindungen.|  
|`ListenBacklog`|Ganze Zahl|1|10|Maximale Anzahl an Verbindungen, die ein Listener nicht verarbeitet hat, bevor zusätzliche Verbindungen zu diesem Endpunkt abgelehnt werden.|  
|`MaxBufferPoolSize`|Long|0|512 kB|Maximaler Arbeitsspeicher in Bytes, die der Transport dem Zusammenlegen von wiederverwendbaren Nachrichtenpuffern widmet. Wenn der Pool keinen Nachrichtenpuffer bieten kann, wird ein neuer Puffer zur temporären Verwendung belegt.<br /><br /> Installationen, die viele Kanalfactorys oder Listeners erstellen, können große Speichermengen für Pufferpools belegen. Die Reduzierung dieser Puffergröße kann die Speicherauslastung in diesem Szenario entscheidend verringern.|  
|`MaxBufferSize`|Ganze Zahl|1|64 KB|Maximale Größe eines Puffers in Bytes, der für das Streaming von Daten verwendet wird. Ist dieses Transportkontingent nicht festgelegt oder verwendet der Transport kein Streaming, entspricht der Kontingentwert dem kleineren der `MaxReceivedMessageSize`-Kontingentwerte und <xref:System.Int32.MaxValue>.|  
|`MaxOutboundConnectionsPerEndpoint`|Ganze Zahl|1|10|Maximale Anzahl an ausgehenden Verbindungen, die einem bestimmten Endpunkt zugeordnet werden können.<br /><br /> Diese Einstellung gilt nur für zusammengeführte Verbindungen.|  
|`MaxOutputDelay`|TimeSpan|0|200 ms|Maximale Zeit für das Warten nach einem Sendevorgang zum Stapeln zusätzlicher Nachrichten in einem einzelnen Vorgang. Nachrichten werden früher gesendet, wenn der Puffer des zugrunde liegenden Transports voll ist. Ein weiteres Senden von Nachrichten setzt den Verzögerungszeitraum nicht zurück.|  
|`MaxPendingAccepts`|Ganze Zahl|1|1|Maximale Anzahl der Annahmen für Kanäle, die der Listener im Wartezustand haben kann.<br /><br /> Es liegt ein Zeitintervall zwischen dem Abschluss der Annahme und dem Starten einer neuen Annahme. Durch die Erhöhung der Sammlungsgröße kann verhindert werden, dass Clients, die während dieses Intervalls eine Verbindung aufbauen, gelöscht werden.|  
|`MaxPendingConnections`|Ganze Zahl|1|10|Maximale Anzahl an Verbindungen, die für einen Listener darauf warten können, von der Anwendung angenommen zu werden. Wenn dieser Kontingentwert überstiegen wird, werden neue eingehende Verbindungen gelöscht, statt weiter auf die Annahme zu warten.<br /><br /> Verbindungsfunktionen, wie Nachrichtensicherheit, können dazu führen, dass ein Client mehr als eine Verbindung öffnet. Dienstadministratoren sollten diese zusätzlichen Verbindungen bei der Einrichtung des Kontingentwerts berücksichtigen.|  
|`MaxReceivedMessageSize`|Long|1|64 KB|Maximale Größe einer empfangenen Nachricht in Bytes, einschließlich der Header, bevor der Transport eine Ausnahme auslöst.|  
|`OpenTimeout`|TimeSpan|0|1 Min.|Maximale Wartezeit für den Aufbau einer Verbindung, bevor der Transport eine Ausnahme auslöst.|  
|`ReceiveTimeout`|TimeSpan|0|10 Min.|Maximale Wartezeit für den Abschluss eines Lesevorgangs, bevor der Transport eine Ausnahme auslöst.|  
|`SendTimeout`|Timespan|0|1 Min.| Maximale Wartezeit für den Abschluss eines Schreibvorgangs, bevor der Transport eine Ausnahme auslöst.|  
  
 Die Transportkontingente `MaxPendingConnections` und `MaxOutboundConnectionsPerEndpoint` werden zu einem einzelnen Transportkontingent namens `MaxConnections` kombiniert, wenn dies durch die Bindung oder die Konfiguration eingerichtet ist. Nur das Bindungselement ermöglicht die einzelne Einrichtung dieser Kontingentwerte. Das `MaxConnections`-Transportkontingent verfügt über die gleichen Mindest- und Standardwerte.  
  
## <a name="setting-transport-quotas"></a>Festlegen von Transportkontingenten  
 Transportkontingente werden durch das Transportbindungselement, die Transportbindung, die Anwendungskonfiguration oder die Hostrichtlinie festgelegt. Dieses Dokument deckt nicht die Einrichtung von Transporten über die Hostrichtlinie ab. Informationen über die Einrichtung von Hostrichtlinienkontingenten finden Sie in der Dokumentation des zugrunde liegenden Transports. Die [Konfigurieren von HTTP und HTTPS](../../../../docs/framework/wcf/feature-details/configuring-http-and-https.md) Thema beschreibt die kontingenteinrichtung des Http.sys-Treibers. Durchsuchen Sie die Microsoft Knowledge Base, um weitere Informationen über die Konfiguration von Windows-Beschränkungen auf HTTP-, TCP/IP- und Named Pipe-Verbindungen zu erhalten.  
  
 Andere Typen von Kontingenten gelten indirekt für Transporte. Der Nachrichtenencoder, den der Transport nutzt, um eine Nachricht in Bytes zu transformieren, kann über eigene Kontingenteinstellungen verfügen. Allerdings sind diese Kontingente vom verwendeten Transporttyp unabhängig.  
  
### <a name="controlling-transport-quotas-from-the-binding-element"></a>Kontrolle von Transportkontingenten vom Bindungselement  
 Das Festlegen von Transportkontingenten durch das Bindungselement bietet die größte Flexibilität in der Kontrolle des Transportverhaltens. Die Standardtimeouts für Schließen-, Öffnen-, Empfangs- und Sendevorgänge werden von der Bindung übernommen, wenn ein Kanal erstellt wird.  
  
|Name|HTTP|TCP/IP|Named Pipe|  
|----------|----------|-------------|----------------|  
|`ChannelInitializationTimeout`||X|X|  
|`CloseTimeout`||||  
|`ConnectionBufferSize`||X|X|  
|`IdleTimeout`||X|X|  
|`LeaseTimeout`||X||  
|`ListenBacklog`||X||  
|`MaxBufferPoolSize`|X|X|X|  
|`MaxBufferSize`|X|X|X|  
|`MaxOutboundConnectionsPerEndpoint`||X|X|  
|`MaxOutputDelay`||X|X|  
|`MaxPendingAccepts`||X|X|  
|`MaxPendingConnections`||X|X|  
|`MaxReceivedMessageSize`|X|X|X|  
|`OpenTimeout`||||  
|`ReceiveTimeout`||||  
|`SendTimeout`||||  
  
### <a name="controlling-transport-quotas-from-the-binding"></a>Kontrolle von Transportkontingenten von der Bindung  
 Die Einrichtung von Transportkontingenten durch die Bindung bietet einen vereinfachten Satz an Kontingenten, aus dem ausgesucht werden kann, während gleichzeitig der Zugriff auf die üblichsten Kontingentwerte aufrechterhalten bleibt.  
  
|Name|HTTP|TCP/IP|Named Pipe|  
|----------|----------|-------------|----------------|  
|`ChannelInitializationTimeout`||||  
|`CloseTimeout`|X|X|X|  
|`ConnectionBufferSize`||||  
|`IdleTimeout`||||  
|`LeaseTimeout`||||  
|`ListenBacklog`||X||  
|`MaxBufferPoolSize`|X|X|X|  
|`MaxBufferSize`|1|X|X|  
|`MaxOutboundConnectionsPerEndpoint`||2|2|  
|`MaxOutputDelay`||||  
|`MaxPendingAccepts`||||  
|`MaxPendingConnections`||2|2|  
|`MaxReceivedMessageSize`|X|X|X|  
|`OpenTimeout`|X|X|X|  
|`ReceiveTimeout`|X|X|X|  
|`SendTimeout`|X|X|X|  
  
1. Das `MaxBufferSize`-Transportkontingent steht nur auf der `BasicHttp`-Bindung zur Verfügung. Die `WSHttp`-Bindungen gelten für Szenarien, die keine Streamingtransportmodi unterstützen.  
  
2. Die Transportkontingente, `MaxPendingConnections` und `MaxOutboundConnectionsPerEndpoint` werden zu einem einzelnen Transportkontingent namens `MaxConnections` kombiniert.  
  
### <a name="controlling-transport-quotas-from-configuration"></a>Kontrolle von Transportkontingenten von der Konfiguration  
 Die Anwendungskonfiguration kann die gleichen Transportkontingente festlegen, wie der direkte Zugriff auf Eigenschaften auf der Bindung . In Konfigurationsdateien beginnt der Name eines Transportkontingents immer mit einem Kleinbuchstaben. Beispielsweise entspricht die `CloseTimeout`-Eigenschaft auf einer Bindung der `closeTimeout`-Einstellung in der Konfiguration, und die `MaxConnections`-Eigenschaft auf einer Bindung entspricht der `maxConnections`-Einstellung in der Konfiguration.  
  
## <a name="see-also"></a>Siehe auch

- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>
- <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
