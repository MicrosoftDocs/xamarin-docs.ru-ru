---
title: Как скопировать выходные файлы IPA в папку сброса TFS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 2542eace19cf08a994be99379566e9e621c27190
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435586"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Как скопировать выходные файлы IPA в папку сброса TFS?

Откройте `.csproj` файл для проекта приложения iOS в текстовом редакторе, а затем добавьте в конец файла следующие строки (непосредственно перед закрывающим `</Project>` тегом):

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
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Примечания

- Это та же общая методика, о которой [можно изменить выходной путь к файлу IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Две важные точки задаются в `$(TF_BUILD_BINARIESDIRECTORY)` качестве папки назначения и добавляют дополнительное условие, которое `CopyIpa` будет выполняться только для сборок TFS.

- Описание `TF_BUILD_BINARIESDIRECTORY` см. в разделе [стандартные переменные сборки](/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Дополнительная справка

- [Документация по установке TFS для использования с Xamarin](/azure/devops/repos/tfvc/overview)
- [Задача сборки Azure DevOps: Xamarin. Android](/azure/devops/pipelines/tasks/build/xamarin-android)
- [Задача сборки Azure DevOps: Xamarin. iOS](/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Следующие шаги

В этом документе рассматривается текущее поведение 3.11.666 Xamarin для Visual Studio и Xamarin. iOS 8.10.3 на узле сборки Mac. Чтобы получить дополнительную помощь, или если проблема остается даже после использования приведенных выше сведений, ознакомьтесь с возможностями [поддержки Xamarin?](~/cross-platform/troubleshooting/support-options.md) для получения сведений о вариантах контактов, предложениях, а также о том, как при необходимости создать новую ошибку.