---
title: Ошибка отсутствующих пакетов после обновления пакетов NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: a1ed4a2be63973e9e26dcb06163a3f9fd7eae649
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728100"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Ошибка отсутствующих пакетов после обновления пакетов NuGet

Эта проблема в основном была указана в примерах решений Xamarin. Forms, но потенциальная проблема может возникнуть в любом проекте, использующем пакеты NuGet.

Если после обновления пакетов NuGet в проекте или решении появится сообщение об ошибке, которое ссылается на старые номера версий пакетов, например:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

В этом примере *Xamarin. Forms. 1.3.1.6296* — это старый номер версии, который был удален вместе с обновлением пакета NuGet.

Это может произойти, если элементы XML в CSPROJ-файле, которые ссылаются на старый номер версии пакета, были добавлены или изменены вручную, NuGet не будет удалять или обновлять их, если они были добавлены или изменены вручную, поэтому теперь проект ищет пакеты, которые были удалены.

Чтобы устранить эту проблему, вручную измените CSPROJ-файлы и удалите все элементы, которые ссылаются на старый номер версии.

Примеры удаляемых элементов (если они имеют старый номер версии пакета):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```
