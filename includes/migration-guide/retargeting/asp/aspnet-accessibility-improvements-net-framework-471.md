---
ms.openlocfilehash: c7ca6965f90cf1e74d4d46d6271a9049f3a29230
ms.sourcegitcommit: 0aca6c5d166d7961a1e354c248495645b97a1dc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58760247"
---
### <a name="aspnet-accessibility-improvements-in-net-framework-471"></a>Verbesserungen der Barrierefreiheit von ASP.NET in .NET Framework 4.7.1

|   |   |
|---|---|
|Details|Ab .NET Framework 4.7.1 arbeiten .ASP.NET-Websteuerelemente effizienter mit den Funktionen für die Barrierefreiheit in Visual Studio zusammen, wodurch ASP.NET-Kunden besser unterstützt werden.  Folgende Änderungen wurden u.a. vorgenommen:<ul><li>Änderungen, durch die fehlende Barrierefreiheitsmuster für Steuerelemente der Benutzeroberfläche implementiert werden. Zu diesen Steuerelementen zählen z.B. das Dialogfeld „Feld hinzufügen“ im Detailansicht-Assistenten oder das Dialogfeld „ListView konfigurieren“ im ListView-Assistenten.</li><li>Änderungen zur Verbesserung der Anzeige im Modus für hohe Kontraste, z.B. beim DataPager-Feld-Editor.</li><li>Änderungen zur Verbesserung der Benutzerfreundlichkeit bei der Tastaturnavigation für Steuerelemente, z.B. beim Dialogfeld „Felder“ im Assistenten für das Bearbeiten von Pagerfeldern des DataPager-Steuerelements, beim Dialogfeld „ObjectContext konfigurieren“ oder beim Dialogfeld „Datenauswahl konfigurieren“ des Assistenten zum Konfigurieren der Datenquelle.</li></ul>|
|Vorschlag|<strong>Aktivieren oder Deaktivieren dieser Änderungen:</strong> Damit diese Änderungen in Visual Studio Designer eingesetzt werden können, muss das Tool unter .NET Framework 4.7.1 oder höher ausgeführt werden. Die Webanwendung kann diese Änderungen nutzen, wenn Sie folgende Schritte durchführen:<ul><li>Installieren Sie Visual Studio 2017 15.3 oder höher. Ab dieser Version werden die neuen Barrierefreiheitsfeatures mit der unten aufgeführten AppContext-Option standardmäßig unterstützt.</li><li>Deaktivieren Sie die Legacy-Barrierefreiheitsverhalten, indem Sie in der Konfigurationsdatei „devenv.exe“ dem Abschnitt <code>&lt;runtime&gt;</code> die AppContext-Option <code>Switch.UseLegacyAccessibilityFeatures</code> hinzufügen und sie auf <code>false</code> festlegen, wie im folgenden Beispiel dargestellt wird.</li></ul><pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;...&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false&#39;  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false&quot; /&gt;&#13;&#10;...&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Bei Anwendungen, die .NET Framework 4.7.1 oder höher als Zielplattform verwenden und die Legacy-Barrierefreiheitsverhalten beibehalten sollen, können Sie die Verwendung des veralteten Features für die Barrierefreiheit aktivieren, indem Sie die AppContext-Option auf <code>true</code> festlegen.|
|Bereich|Gering|
|Version|4.7.1|
|Typ|Neuzuweisung|

