---
title: '.NET Core-CLI-Befehl: dotnet tool uninstall'
description: Der Befehl „dotnet tool uninstall“ deinstalliert das angegebene globale .NET Core-Tool auf Ihrem Computer.
author: mairaw
ms.author: mairaw
ms.date: 05/29/2018
ms.openlocfilehash: 5cf80d1756dbf4e88bb52a8028d186d44978f440
ms.sourcegitcommit: bbf70abe6b46073148f78cbf0619de6092b5800c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34696937"
---
# <a name="dotnet-tool-uninstall"></a><span data-ttu-id="6b08b-103">dotnet tool uninstall</span><span class="sxs-lookup"><span data-stu-id="6b08b-103">dotnet tool uninstall</span></span>

[!INCLUDE [topic-appliesto-net-core-21plus.md](../../../includes/topic-appliesto-net-core-21plus.md)]

## <a name="name"></a><span data-ttu-id="6b08b-104">name</span><span class="sxs-lookup"><span data-stu-id="6b08b-104">Name</span></span>

<span data-ttu-id="6b08b-105">`dotnet tool uninstall`: Deinstalliert das angegebene [globale .NET Core-Tool](global-tools.md) auf Ihrem Computer.</span><span class="sxs-lookup"><span data-stu-id="6b08b-105">`dotnet tool uninstall` - Uninstalls the specified [.NET Core Global Tool](global-tools.md) from your machine.</span></span>

## <a name="synopsis"></a><span data-ttu-id="6b08b-106">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6b08b-106">Synopsis</span></span>

```
dotnet tool uninstall <PACKAGE_NAME> <-g|--global>
dotnet tool uninstall <PACKAGE_NAME> <--tool-path>
dotnet tool uninstall <-h|--help>
```

## <a name="description"></a><span data-ttu-id="6b08b-107">description</span><span class="sxs-lookup"><span data-stu-id="6b08b-107">Description</span></span>

<span data-ttu-id="6b08b-108">Der `dotnet tool uninstall`-Befehl ermöglicht Ihnen das Deinstallieren von globalen .NET Core-Tools auf Ihrem Computer.</span><span class="sxs-lookup"><span data-stu-id="6b08b-108">The `dotnet tool uninstall` command provides a way for you to uninstall .NET Core Global Tools from your machine.</span></span> <span data-ttu-id="6b08b-109">Um diesen Befehl verwenden zu können, müssen Sie entweder angeben, dass Sie ein benutzerweites Tool über die `--global`-Option entfernen möchten, oder Sie müssen mit der Option `--tool-path` einen Pfad zu dem Speicherort angeben, an dem das Tool installiert wird.</span><span class="sxs-lookup"><span data-stu-id="6b08b-109">To use the command, you either have to specify that you want to remove a user-wide tool using the `--global` option or specify a path to where the tool is installed using the `--tool-path` option.</span></span>

## <a name="arguments"></a><span data-ttu-id="6b08b-110">Argumente</span><span class="sxs-lookup"><span data-stu-id="6b08b-110">Arguments</span></span>

`PACKAGE_NAME`

<span data-ttu-id="6b08b-111">Name oder ID des NuGet-Pakets, das das zu deinstallierende globale .NET Core-Tool enthält.</span><span class="sxs-lookup"><span data-stu-id="6b08b-111">Name/ID of the NuGet package that contains the .NET Core Global Tool to uninstall.</span></span> <span data-ttu-id="6b08b-112">Mithilfe des Befehls [dotnet tool list](dotnet-tool-list.md) können Sie den Paketnamen abrufen.</span><span class="sxs-lookup"><span data-stu-id="6b08b-112">You can find the package name using the [dotnet tool list](dotnet-tool-list.md) command.</span></span>

## <a name="options"></a><span data-ttu-id="6b08b-113">Optionen</span><span class="sxs-lookup"><span data-stu-id="6b08b-113">Options</span></span>

`-g|--global`

<span data-ttu-id="6b08b-114">Gibt an, dass das zu entfernende Tool von einer benutzerweiten Installation stammt.</span><span class="sxs-lookup"><span data-stu-id="6b08b-114">Specifies that the tool to be removed is from a user-wide installation.</span></span> <span data-ttu-id="6b08b-115">Kann nicht mit der Option `--tool-path` kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="6b08b-115">Can't be combined with the `--tool-path` option.</span></span> <span data-ttu-id="6b08b-116">Wenn Sie diese Option nicht angeben, müssen Sie die Option `--tool-path` angeben.</span><span class="sxs-lookup"><span data-stu-id="6b08b-116">If you don't specify this option, you must specify the `--tool-path` option.</span></span>

`-h|--help`

<span data-ttu-id="6b08b-117">Druckt eine kurze Hilfe für den Befehl.</span><span class="sxs-lookup"><span data-stu-id="6b08b-117">Prints out a short help for the command.</span></span>

`--tool-path <PATH>`

<span data-ttu-id="6b08b-118">Gibt den Speicherort des globalen Tools an, das deinstalliert wird.</span><span class="sxs-lookup"><span data-stu-id="6b08b-118">Specifies the location where to uninstall the Global Tool.</span></span> <span data-ttu-id="6b08b-119">„PATH“ kann absolut oder relativ sein.</span><span class="sxs-lookup"><span data-stu-id="6b08b-119">PATH can be absolute or relative.</span></span> <span data-ttu-id="6b08b-120">Kann nicht mit der Option `--global` kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="6b08b-120">Can't be combined with the `--global` option.</span></span> <span data-ttu-id="6b08b-121">Wenn Sie diese Option nicht angeben, müssen Sie die Option `--global` angeben.</span><span class="sxs-lookup"><span data-stu-id="6b08b-121">If you don't specify this option, you must specify the `--global` option.</span></span>

## <a name="examples"></a><span data-ttu-id="6b08b-122">Beispiele</span><span class="sxs-lookup"><span data-stu-id="6b08b-122">Examples</span></span>

<span data-ttu-id="6b08b-123">Deinstalliert das globale Tool [dotnetsay](https://www.nuget.org/packages/dotnetsay/):</span><span class="sxs-lookup"><span data-stu-id="6b08b-123">Uninstalls the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool:</span></span>

`dotnet tool uninstall -g dotnetsay`

<span data-ttu-id="6b08b-124">Deinstalliert das globale Tool [dotnetsay](https://www.nuget.org/packages/dotnetsay/) aus einem bestimmten Windows-Ordner:</span><span class="sxs-lookup"><span data-stu-id="6b08b-124">Uninstalls the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool from a specific Windows folder:</span></span>

`dotnet tool uninstall dotnetsay --tool-path c:\global-tools`

<span data-ttu-id="6b08b-125">Deinstalliert das globale Tool [dotnetsay](https://www.nuget.org/packages/dotnetsay/) aus einem bestimmten Linux-/macOS-Ordner:</span><span class="sxs-lookup"><span data-stu-id="6b08b-125">Uninstalls the [dotnetsay](https://www.nuget.org/packages/dotnetsay/) Global Tool from a specific Linux/macOS folder:</span></span>

`dotnet tool uninstall dotnetsay --tool-path ~/bin`

## <a name="see-also"></a><span data-ttu-id="6b08b-126">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="6b08b-126">See also</span></span>

[<span data-ttu-id="6b08b-127">.NET Core Global Tools (Globale .NET Core-Tools)</span><span class="sxs-lookup"><span data-stu-id="6b08b-127">.NET Core Global Tools</span></span>](global-tools.md)