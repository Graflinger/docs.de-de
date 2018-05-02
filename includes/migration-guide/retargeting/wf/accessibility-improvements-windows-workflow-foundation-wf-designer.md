### <a name="accessibility-improvements-in-windows-workflow-foundation-wf-workflow-designer"></a>Verbesserungen der Bedienungshilfen im Workflow-Designer von Windows Workflow Foundation (WF)

|   |   |
|---|---|
|Details|Die Arbeitsweise von Workflow-Designer von Windows Workflow Foundation (WF) mit Technologien für die Barrierefreiheit wurde verbessert. Diese Verbesserungen umfassen folgende Änderungen:<ul><li>Die Aktivierreihenfolge wurde bei manchen Steuerelementen in „ Von links nach rechts“ und in „Von oben nach unten“ geändert:</li><li>Das Fenster „Korrelation initialisieren“ für das Festlegen von Korrelationsdaten für die <xref:System.ServiceModel.Activities.InitializeCorrelation>-Aktivität</li><li>Das Fenster „Inhaltsdefinition“ für die Aktivitäten <xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> und <xref:System.ServiceModel.Activities.ReceiveReply></li><li>Weitere Funktionen sind über die Tastatur verfügbar:</li><li>Beim Bearbeiten der Eigenschaften einer Aktivität können die Eigenschaftengruppen über die Tastatur reduziert werden, wenn diese zum ersten Mal fokussiert werden.</li><li>Auf Warnsymbole kann nun über die Tastatur zugegriffen werden.</li><li>Auf die Schaltfläche „Weitere Eigenschaften“ im Fenster „Eigenschaften“ kann nun über die Tastatur zugegriffen werden.</li><li>Tastaturbenutzer können nun auf die Headerelemente in den Bereichen „Argumente“ und „Variablen“ des Workflow-Designers zugreifen.</li><li>Verbesserte Sichtbarkeit von Elementen mit Fokus, z.B. in folgenden Fällen:</li><li>Hinzufügen von Zeilen zu Datenrastern, die vom Workflow-Designer und von Aktivitäts-Designern verwendet werden</li><li>Wechseln von Feldern mit der TAB-TASTE in den Aktivitäten <xref:System.ServiceModel.Activities.ReceiveReply> und <xref:System.ServiceModel.Activities.SendReply></li><li>Festlegen von Standardwerten für Variablen oder Argumente</li><li>Sprachausgaben können Folgendes nun richtig erkennen:</li><li>Breakpoints, die im Workflow-Designer festgelegt wurden</li><li>Die Aktivitäten <xref:System.Activities.Statements.FlowSwitch%601>, <xref:System.Activities.Statements.FlowDecision> und <xref:System.ServiceModel.Activities.CorrelationScope></li><li>Die Inhalte der <xref:System.ServiceModel.Activities.Receive>-Aktivität</li><li>Den Zieltyp für die <xref:System.Activities.Statements.InvokeMethod>-Aktivität</li><li>Das Kombinationsfeld „Ausnahme“ und den Abschnitt „Finally“ in der <xref:System.Activities.Statements.TryCatch>-Aktivität</li><li>Das Kombinationsfeld „Nachrichtentyp“, den Splitter im Fenster „Korrelationsinitialisierer hinzufügen“, das Fenster „Inhaltsdefinition“ und das Definitionsfenster „CorrelatesOn“ in den Messagingaktivitäten (<xref:System.ServiceModel.Activities.Receive>, <xref:System.ServiceModel.Activities.Send>, <xref:System.ServiceModel.Activities.SendReply> und <xref:System.ServiceModel.Activities.ReceiveReply>)</li><li>Übertragungen von Zustandsautomaten und Übertragungsziele</li><li>Anmerkungen und Connectors von <xref:System.Activities.Statements.FlowDecision>-Aktivitäten</li><li>Die per Rechtsklick aufrufbaren Kontextmenüs von Aktivitäten</li><li>Die Editors für Eigenschaftswerte, die Schaltfläche, „Suche löschen“, die Sortierschaltflächen „Nach Kategorie“ und „Alphabetisch“ sowie das Dialogfeld „Ausdrucks-Editor“ im Eigenschaftenraster</li><li>Den Zoomprozentwert im Workflow-Designer</li><li>Das Trennzeichen in den Aktivitäten <xref:System.Activities.Statements.Parallel> und <xref:System.Activities.Statements.Pick></li><li>Die <xref:System.Activities.Statements.InvokeDelegate>-Aktivität</li><li>Das Fenster „Typen auswählen“ für Wörterbuchaktivitäten (<code>Microsoft.Activities.AddToDictionary&lt;TKey,TValue&gt;</code>, <code>Microsoft.Activities.RemoveFromDictionary&lt;TKey,TValue&gt;</code> usw.)</li><li>Das Fenster „.NET-Typ suchen und auswählen“</li><li>Breadcrumbs im Workflow-Designer</li><li>Benutzer, die Designs mit hohem Kontrast verwenden, werden viele Verbesserungen in der Sichtbarkeit des Workflow-Designers und dessen Steuerelementen feststellen. Dazu zählen verbesserte Kontrastverhältnisse zwischen Elementen und besser erkennbare Auswahlfelder für Fokuselemente.</li></ul>|
|Vorschlag|Wenn Sie eine Anwendung mit einem neu gehosteten Workflow-Designer besitzen, kann Ihre Anwendung von diesen Änderungen profitieren, indem Sie eine der folgenden Aktionen durchführen:<ul><li>Rekompilieren Sie Ihre Anwendung, um .NET Framework 4.7.1 anzuzielen. Die Verbesserungen der Barrierefreiheit werden standardmäßig aktiviert.</li><li>Wenn Ihre Anwendung .NET Framework 4.7 oder früher anzielt, aber auf .NET Framework 4.7.1 ausgeführt wird, können Sie die veralteten Verhaltensweisen für die Barrierefreiheit deaktivieren, indem Sie folgendes [AppContext-Element](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) zum <code>&lt;runtime&gt;</code>-Abschnitt der app.config-Datei hinzufügen und dieses wie im folgenden Beispiel dargestellt auf <code>false</code> festlegen.</li></ul><pre><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;startup&gt;&#13;&#10;&lt;supportedRuntime version=&quot;v4.0&quot; sku=&quot;.NETFramework,Version=v4.7&quot;/&gt;&#13;&#10;&lt;/startup&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>Bei Anwendungen, die .NET Framework 4.7.1 oder höher als Zielplattform verwenden und die Legacy-Barrierefreiheitsverhalten beibehalten sollen, können Sie die Verwendung des veralteten Features für die Barrierefreiheit aktivieren, indem Sie die AppContext-Option auf <code>true</code> festlegen.|
|Bereich|Gering|
|Version|4.7.1|
|Typ|Neuzuweisung|
