---
ms.openlocfilehash: b7f31060fd845f7618479fd064350b3cb5532fd3
ms.sourcegitcommit: 0aca6c5d166d7961a1e354c248495645b97a1dc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58761080"
---
### <a name="xmlwriter-throws-on-invalid-surrogate-pairs"></a>XmlWriter löst bei ungültigen Ersatzzeichenpaaren Ausnahmen aus

|   |   |
|---|---|
|Details|Bei Apps, die auf .NET Framework 4.5.2 oder niedrigere Versionen abzielen, löst das Schreiben eines ungültigen Ersatzzeichenpaars mithilfe der Fallbackbehandlung nicht immer eine Ausnahme aus. Bei Apps mit der Zielplattform .NET Framework 4.6 löst der Versuch, ein ungültiges Ersatzzeichenpaar zu schreiben, eine <xref:System.ArgumentException?displayProperty=name> aus.|
|Vorschlag|Falls erforderlich, kann dieser Fehler umgangen werden, indem als Zielplattform.NET Framework 4.5.2 oder eine ältere Version verwendet wird. Alternativ können ungültige Ersatzzeichenpaare vor dem Schreiben auch zuerst in gültigen XML-Code umgewandelt werden.|
|Bereich|Microsoft Edge|
|Version|4.6|
|Typ|Neuzuweisung|
|Betroffene APIs|<ul><li><xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteAttributeString(System.String,System.String,System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteAttributeStringAsync(System.String,System.String,System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteCData(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteCDataAsync(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteChars(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteCharsAsync(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteComment(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteCommentAsync(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteEntityRef(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteEntityRefAsync(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteRaw(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteProcessingInstruction(System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteProcessingInstructionAsync(System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteRaw(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteRawAsync(System.Char[],System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteRawAsync(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteString(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteStringAsync(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteSurrogateCharEntity(System.Char,System.Char)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteSurrogateCharEntityAsync(System.Char,System.Char)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlWriter.WriteValue(System.String)?displayProperty=nameWithType></li></ul>|

