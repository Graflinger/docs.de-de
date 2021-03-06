---
title: Synchrone Szenarien mit HTTP, TCP oder benannten Pipes
ms.date: 03/30/2017
ms.assetid: 7e90af1b-f8f6-41b9-a63a-8490ada502b1
ms.openlocfilehash: 28e612b190f4993e1ce7da0d1083c4e55f827d4a
ms.sourcegitcommit: 15ab532fd5e1f8073a4b678922d93b68b521bfa0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58654002"
---
# <a name="synchronous-scenarios-using-http-tcp-or-named-pipe"></a>Synchrone Szenarien mit HTTP, TCP oder benannten Pipes
In diesem Thema werden die Aktivitäten und Übertragungen für verschiedene synchrone Anforderungs-/Antwortszenarien beschrieben. Dabei werden HTTP, TCP oder benannte Pipes mit einem Singlethreadclient verwendet. Finden Sie unter [asynchrone Szenarien mit HTTP, TCP oder Named Pipes](../../../../../docs/framework/wcf/diagnostics/tracing/asynchronous-scenarios-using-http-tcp-or-named-pipe.md) für Weitere Informationen zu multithreadanforderungen.  
  
## <a name="synchronous-requestreply-without-errors"></a>Synchrone Anforderung/Antwort ohne Fehler  
 In diesem Abschnitt werden die Aktivitäten und Übertragungen für ein gültiges synchrones Anforderungs-/Antwortszenario mit Singlethreadclient beschrieben.  
  
### <a name="client"></a>Client  
  
#### <a name="establishing-communication-with-service-endpoint"></a>Herstellen einer Verbindung mit Dienstendpunkt  
 Ein Client wird erstellt und geöffnet. Für jede der folgenden Schritte aus (A) wird die umgebungsaktivität bzw. an eine "Konstruktclient" (B) und "Offene Client"-Aktivität (C) übertragen. Für jede Aktivität, auf die übertragen wird, wird die Umgebungsaktivität unterbrochen, bis eine Übertragung zurück erfolgt, d. h. bis ServiceModel-Code ausgeführt wird.  
  
#### <a name="making-a-request-to-service-endpoint"></a>Stellen einer Anforderung an einen Dienstendpunkt  
 Die umgebungsaktivität wird auf eine Aktivität "ProcessAction" (D) übertragen. Innerhalb dieser Aktivität wird eine Anforderungsnachricht gesendet, und eine Antwortmeldung wird empfangen. Die Aktivität endet, wenn das Steuerelement zum Benutzercode zurückkehrt. Da dies eine asynchrone Anforderung ist, unterbricht die Umgebungsaktivität, bis das Steuerelement einen Wert zurückgibt.  
  
#### <a name="closing-communication-with-service-endpoint"></a>Schließen einer Verbindung mit Dienstendpunkt  
 Die Aktivität "Schließen" (I) des Clients wird aus der Umgebungsaktivität erstellt. Dies ist identisch mit "neu" und "geöffnet".  
  
### <a name="server"></a>Server  
  
#### <a name="setting-up-a-service-host"></a>Einrichten eines Diensthosts  
 Die neuen und geöffneten Aktivitäten (N und O) des ServiceHost werden aus der Umgebungsaktivität (M) erstellt.  
  
 Eine Listeneraktivität (P) wird dadurch erstellt, dass ein ServiceHost für jeden Listener geöffnet wird. Die Listeneraktivität wartet darauf, Daten zu empfangen und zu verarbeiten.  
  
#### <a name="receiving-data-on-the-wire"></a>Empfangen von Daten durch Übertragung  
 Wenn Daten über das Netzwerk empfangen werden, wird eine "ReceiveBytes"-Aktivität erstellt, wenn es (Q) zum Verarbeiten der empfangenen Daten nicht bereits vorhanden ist. Diese Aktivität kann für mehrere Nachrichten in einer Verbindung oder einer Warteschlange wiederverwendet werden.  
  
 Die ReceiveBytes-Aktivität startet eine ProcessMessage-Aktivität (R), wenn ausreichend Daten vorhanden sind, um eine SOAP-Aktionsnachricht zu erstellen.  
  
 Bei der Aktivität R werden die Nachrichtenheader verarbeitet, und der activityID-Header wird überprüft. Wenn dieser Header vorhanden ist, wird die Aktivitäts-ID als ProcessAction-Aktivität festgelegt; andernfalls wird eine neue ID erstellt.  
  
 Eine ProcessAction-Aktivität (S) wird erstellt, und es wird auf sie übertragen, wenn der Aufruf verarbeitet wird. Diese Aktivität endet, wenn alle Verarbeitungsschritte bezüglich der eingehenden Nachricht abgeschlossen sind, ggf. einschließlich der Ausführung des Benutzercodes (T) und des Sendens der Antwortnachricht.  
  
#### <a name="closing-a-service-host"></a>Schließen eines Diensthosts  
 Die Aktivität "Schließen" (Z) des ServiceHost wird aus der Umgebungsaktivität erstellt.  
  
 ![Diagramm der synchrone Szenarien: HTTP, TCP oder named Pipes.](./media/synchronous-scenarios-using-http-tcp-or-named-pipe/synchronous-scenario-http-tcp-named-pipes.gif)  
  
 In \<A: Name >, `A` ein shortcutsymbol, das die Aktivität im vorangegangenen Text und in Tabelle 3 beschreibt. `Name` ist ein verkürzter Name der Aktivität.  
  
 Wenn `propagateActivity` = `true`, Verarbeitungsaktion auf dem Client und Dienst verfügen, die dieselbe Aktivitäts-ID.  
  
## <a name="synchronous-requestreply-with-errors"></a>Synchrone Anforderung/Antwort mit Fehlern  
 Der einzige Unterschied zu dem vorherigen Szenario besteht darin, dass eine SOAP-Fehlernachricht als Antwortnachricht zurückgegeben wird. Wenn `propagateActivity` = `true`, wird die Aktivitäts-ID der Anforderungsnachricht SOAP-Fehlernachricht hinzugefügt.  
  
## <a name="synchronous-one-way-without-errors"></a>Synchrone unidirektionale Kommunikation ohne Fehler  
 Der einzige Unterschied zu dem ersten Szenario ist, dass keine Nachricht an den Server zurückgegeben wird. Für HTTP-basierte Protokolle wird immer noch ein Status (gültig oder Fehler) an den Client zurückgegeben. Dies ist da HTTP das einzige Protokoll mit einem Anforderung / Antwort-Semantik ist, die für den WCF-Protokollstapel gehört. Da WCF TCP-Verarbeitung sichtbar ist, wird keine Bestätigung an den Client gesendet.  
  
## <a name="synchronous-one-way-with-errors"></a>Synchrone unidirektionale Kommunikation mit Fehlern  
 Wenn während der Verarbeitung der Nachricht ein Fehler auftritt (Q oder darüber hinaus), wird keine Benachrichtigung an den Client zurückgegeben. Dies ist identisch mit dem Szenario „Synchrone unidirektionale Kommunikation ohne Fehler“. Sie sollten kein unidirektionales Szenario verwenden, wenn Sie eine Fehlermeldung empfangen möchten.  
  
## <a name="duplex"></a>Duplex  
 Der Unterschied zu den vorherigen Szenarien besteht darin, dass der Client als Dienst fungiert, wobei er die ReceiveBytes- und die ProcessMessage-Aktivität erstellt, ähnlich wie bei den asynchronen Szenarien.
