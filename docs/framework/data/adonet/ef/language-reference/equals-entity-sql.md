---
title: = (Gleich) (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 948eb588-7080-4046-bb48-633b007393bf
ms.openlocfilehash: d50ede1964f6d6b9025a7214efe90e878aa55a0c
ms.sourcegitcommit: 558d78d2a68acd4c95ef23231c8b4e4c7bac3902
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59333157"
---
# <a name="-equals-entity-sql"></a>= (Gleich) (Entity SQL)
Überprüft zwei Ausdrücke auf Gleichheit.  
  
## <a name="syntax"></a>Syntax  
  
```  
expression = expression  
or   
expression == expression  
```  
  
## <a name="arguments"></a>Argumente  
 `expression`  
 Jeder gültige Ausdruck. Beide Ausdrücke müssen implizit konvertierbare Datentypen sein.  
  
## <a name="result-types"></a>Ergebnistypen  
 `true` Wenn der linke Ausdruck ungleich dem rechten Ausdruck ist. andernfalls `false`.  
  
## <a name="remarks"></a>Hinweise  
 Der Operator "==" ist äquivalent zum Operator "=".  
  
## <a name="example"></a>Beispiel  
 In der folgenden Entity SQL-Abfrage wird der Vergleichsoperator "=" verwendet, um zwei Ausdrücke auf Gleichheit zu überprüfen. Diese Abfrage beruht auf dem "AdventureWorks Sales"-Modell. Führen Sie folgende Schritte aus, um diese Abfrage zu kompilieren und auszuführen:  
  
1. Führen Sie die Verfahren in [Vorgehensweise: Ausführen einer Abfrage, die StructuralType-Ergebnisse zurückgibt](../../../../../../docs/framework/data/adonet/ef/how-to-execute-a-query-that-returns-structuraltype-results.md).  
  
2. Übergeben Sie die folgende Abfrage als Argument an die `ExecuteStructuralTypeQuery` -Methode:  
  
 [!code-csharp[DP EntityServices Concepts 2#EQUALS](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#equals)]  
  
## <a name="see-also"></a>Siehe auch

- [Entity SQL-Referenz](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)
