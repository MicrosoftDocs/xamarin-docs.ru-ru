---
title: Настройка параметров памяти Java для конструктора Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 5a692a931bfcdc1e8eee534de3adfff0de688891
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457904"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Настройка параметров памяти Java для конструктора Android

Параметры памяти по умолчанию, используемые при запуске процесса `java`для конструктора Android, могут быть несовместимы с некоторыми конфигурациями системы.

Начиная с Xamarin Studio 5.7.2.7 (и более поздних версий Visual Studio для Mac) и Инструментов Visual Studio для Xamarin 3.9.344 эти параметры можно настроить отдельно для каждого проекта.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>Новые свойства конструктора Android и соответствующие параметры Java

Следующие имена свойств соответствуют указанному [параметру командной строки](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) Java

- **AndroidDesignerJavaRendererMinMemory** — Xms

- **AndroidDesignerJavaRendererMaxMemory** — Xmx

- **AndroidDesignerJavaRendererPermSize** — XX:MaxPermSize

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Откройте решение в Visual Studio.

2. Выберите каждый проект Android один за другим в обозревателе решений и дважды щелкните [Показать все файлы](/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) в каждом проекте. Вы можете пропустить проекты, которые не содержат файлы макета `.axml`. Этот шаг гарантирует, что каждый каталог проекта будет содержать файл `.csproj.user`.

3. Выйдите из Visual Studio.

4. Найдите файл `.csproj.user` для каждого из проектов, выполненных на шаге 2.

5. Измените каждый файл `.csproj.user` в текстовом редакторе.

6. Добавьте любые или все новые свойства памяти конструктора Android в элемент `<PropertyGroup>`. Вы можете добавить существующий элемент `<PropertyGroup>` или создать новый. Ниже приведен полный пример файла `.csproj.user`, который содержит все 3 атрибута, для которых заданы значения по умолчанию:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7. Сохраните и закройте все обновленные файлы `.csproj.user`.

8. Перезапустите Visual Studio и повторно откройте решение.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

1. Откройте решение в Visual Studio для Mac, чтобы убедится, что каталог решения содержит файл `.userprefs`.

2. Закройте Visual Studio для Mac.

3. Найдите файл `.userprefs` в каталоге решений.

4. Измените файл `.userprefs` в текстовом редакторе.

5. Найдите существующий элемент XML в следующем формате. Последняя часть этого имени элемента будет соответствовать имени проекта: в этом примере — "AndroidApplication1":

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. Если элемент `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` не существует, создайте его в любом месте внутри вложенного элемента `<Properties>`. Не забудьте заменить "AndroidApplication1" именем своего проекта.

7. Добавьте любые или все новые свойства памяти конструктора Android в качестве атрибутов элемента. Ниже приведен полный пример файла `.userprefs`, который содержит все 3 атрибута, для которых заданы значения по умолчанию:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8. Повторите шаги 5-7 для каждого проекта Android в решении, содержащем файлы макета `.axml`. (То есть добавьте один элемент `<MonoDevelop.Ide.ItemProperties.ProjectName>` для каждого проекта.)

9. Сохраните и закройте файл `.userprefs`.

10. Перезапустите Visual Studio для Mac и повторно откройте решение.

-----