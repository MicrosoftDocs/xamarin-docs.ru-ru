---
title: Можно ли изменить выходной путь к файлу IPA?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9d9ae44fc303af914d19f719f32e395accfd452e
ms.sourcegitcommit: e7a5d1ec9e50a09b3b24f4c57850a4763c3406d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/18/2021
ms.locfileid: "101087451"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Можно ли изменить выходной путь к файлу IPA?

## <a name="for-cycle-7-and-higher"></a>Для цикла 7 и более поздних версий
Да, для достижения этого можно использовать настраиваемые целевые объекты MSBuild. Самый простой способ — скопировать `.ipa` файл после его построения.

Эти действия будут работать для любого проекта iOS, использующего модуль сборки MSBuild на компьютерах Mac или Windows. (Примечание. все Unified API проекты используют подсистему сборки MSBuild.)

1. Откройте `.csproj` файл для проекта приложения iOS в текстовом редакторе, а затем добавьте в конец файла следующие строки (непосредственно перед закрывающим `</Project>` тегом):

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Задайте для DestinationFolder нужную выходную папку. Как правило, при необходимости можно использовать свойства MSBuild (например, $ (OutputPath)) в этом аргументе.

### <a name="notes"></a>Примечания

- `CreateIpaDependsOn`Свойство определяется в `Xamarin.iOS.Common.targets` файле, который является частью Xamarin. iOS. Он ведет себя, как описано в разделе [Переопределение предопределенных целевых объектов](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) в статье [как расширить процесс сборки Visual Studio](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Если вы предпочитаете, можно использовать задачу **перемещения** , а не задачу **копирования** . При выборе этого варианта и построении в Windows необходимо использовать полное имя задачи, `<Microsoft.Build.Tasks.Move>` чтобы избежать неоднозначности в задачах сборки ксамаринвс.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Для версий, предшествующих Xamarin Studio 6.0.0.5174 | Xamarin для Visual Studio 4.1.0.530

Да, для достижения этого можно использовать настраиваемые целевые объекты MSBuild. Самый простой способ — скопировать `.ipa` файл после его построения.

Эти действия будут работать для любого проекта iOS, использующего модуль сборки MSBuild на компьютерах Mac или Windows. (Примечание. все Unified API проекты используют подсистему сборки MSBuild.)

1. Откройте `.csproj` файл для проекта приложения iOS в текстовом редакторе, а затем добавьте следующие строки в конец (непосредственно перед закрывающим `</Project>` тегом).

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>

    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Присвойте свойству `DestinationFolder` требуемую выходную папку. Как правило, при необходимости можно использовать свойства MSBuild (например, `$(OutputPath)` ) в этом аргументе.

### <a name="notes"></a>Примечания

- `CreateIpaDependsOn`Свойство определяется в `Xamarin.iOS.Common.targets` файле, который является частью Xamarin. iOS. как описано в разделе [Переопределение предопределенных целевых объектов](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) в статье [как расширить процесс сборки Visual Studio](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Если вы предпочитаете, можно использовать задачу **перемещения** , а не задачу **копирования** . При выборе этого варианта и построении в Windows необходимо использовать полное имя задачи, `<Microsoft.Build.Tasks.Move>` чтобы избежать неоднозначности в задачах сборки ксамаринвс.