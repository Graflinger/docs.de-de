---
title: 'Vorgehensweise: Angeben des Ausführungsmodus in PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to use execution mode
ms.assetid: e52ff26c-c5d3-4fab-9fec-c937fb387963
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c67b23da6742af3cb65da6da49dbab982a0248bb
ms.sourcegitcommit: 6b308cf6d627d78ee36dbbae8972a310ac7fd6c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54694619"
---
# <a name="how-to-specify-the-execution-mode-in-plinq"></a>Vorgehensweise: Angeben des Ausführungsmodus in PLINQ
Dieses Beispiel zeigt, wie erzwungen wird, dass PLINQ seine Standardheuristik umgeht und eine Abfrage unabhängig von der Form parallelisiert.  
  
> [!WARNING]
>  Dieses Beispiel soll die Nutzung darstellen und wird möglicherweise nicht schneller ausgeführt als die entsprechende sequenzielle LINQ to Objects-Abfrage. Weitere Informationen finden Sie unter [Grundlagen zur Beschleunigung in PLINQ](../../../docs/standard/parallel-programming/understanding-speedup-in-plinq.md).  
  
## <a name="example"></a>Beispiel  
 [!code-csharp[PLINQ#22](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#22)]
 [!code-vb[PLINQ#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#22)]  
  
 PLINQ ist dazu konzipiert, Gelegenheiten zur Parallelisierung auszunutzen. Nicht alle Abfragen profitieren jedoch von der parallelen Ausführung. Wenn eine Abfrage z.B. einen einzelnen Benutzerdelegaten enthält, der nur sehr wenig ausführt, ist die sequenzielle Ausführung der Abfrage in der Regel schneller. Dies liegt daran, dass sich der Mehraufwand zum Aktivieren der parallelen Ausführung im Vergleich zur erzielten Beschleunigung nicht rentiert. Darum parallelisiert PLINQ nicht automatisch jede Abfrage. Zunächst werden die Form der Abfrage und die verschiedenen enthaltenen Operatoren untersucht. Auf Grundlage dieser Analyse kann PLINQ im Standardausführungsmodus entscheiden, die Abfrage zum Teil oder im Ganzen sequenziell auszuführen. In einigen Fällen wissen Sie jedoch vielleicht mehr über die Abfrage, als PLINQ in seiner Analyse ermitteln kann. Beispielsweise können Sie wissen, dass ein Delegat sehr aufwändig ist und die Abfrage definitiv von einer Parallelisierung profitiert. In solchen Fällen können Sie mit der <xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A>-Methode den <xref:System.Linq.ParallelExecutionMode.ForceParallelism>-Wert angeben und PLINQ anweisen, die Abfrage stets parallel auszuführen.  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Schneiden Sie diesen Code aus, fügen Sie ihn in das [PLINQ-Datenbeispiel](../../../docs/standard/parallel-programming/plinq-data-sample.md) ein, und rufen Sie die Methode in `Main` auf.  
  
## <a name="see-also"></a>Siehe auch

- <xref:System.Linq.ParallelEnumerable.AsSequential%2A>
- [Parallel LINQ (PLINQ) (Paralleles LINQ (PLINQ))](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
