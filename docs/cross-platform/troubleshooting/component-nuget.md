---
title: Обновление ссылок на компоненты в NuGet
description: В этом документе описывается, как заменить ссылки на компоненты с помощью пакетов NuGet на будущие версии приложений, так как хранилище компонентов Xamarin больше не поддерживается.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
ms.openlocfilehash: b9b771efe338fbcc250aa6e7a83b73f35269d3bf
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996816"
---
# <a name="updating-component-references-to-nuget"></a>Обновление ссылок на компоненты в NuGet

> [!IMPORTANT]
> Хранилище компонентов было прекращено до 15 мая 2018 (это замыкание было первоначально [объявлено](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) в ноябре 2017 ноября).
>
> Компоненты Xamarin больше не поддерживаются в Visual Studio и должны быть заменены пакетами NuGet. Следуйте приведенным ниже инструкциям, чтобы вручную удалить ссылки на компоненты из проектов.

Дополнительные сведения о добавлении пакетов NuGet в [Windows](https://docs.microsoft.com/nuget/quickstart/use-a-package) или [Mac](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)см. в этих инструкциях.

Доступен список популярных [подключаемых модулей и библиотек](https://github.com/xamarin/XamarinComponents/blob/master/README.md) Xamarin, которые помогают найти альтернативы компонентам, недоступным в качестве пакетов NuGet.

## <a name="manually-removing-component-references"></a>Удаление ссылок на компоненты вручную

Выпуск 15,6 Visual Studio и 7,4 Visual Studio для Mac больше не поддерживают компоненты в проекте.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

При загрузке проекта в Visual Studio отображается следующее диалоговое окно, объясняющее, что необходимо вручную удалить все компоненты из проекта:

![Диалоговое окно предупреждения с объяснением того, что компонент найден в проекте и должен быть удален](component-nuget-images/component-alert-vs.png)

Чтобы удалить компонент из проекта, выполните следующие действия.

1. Откройте **CSPROJ**-файл. Для этого щелкните правой кнопкой мыши имя проекта и выберите команду **Выгрузить проект**.

2. Щелкните правой кнопкой мыши выгруженный проект и выберите **Изменить {имя_проекта-Project-Name}. csproj**.

3. Найдите в файле все ссылки на `XamarinComponentReference` . Он должен выглядеть следующим образом:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Удалите ссылки на `XamarinComponentReference` и сохраните файл. В приведенном выше примере можно полностью удалить все `ItemGroup` .

5. После сохранения файла щелкните правой кнопкой мыши имя проекта и выберите **Перезагрузить проект**.

6. Повторите описанные выше действия для каждого проекта в решении.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

При загрузке проекта в Visual Studio для Mac отображается следующее диалоговое окно, объясняющее, что необходимо вручную удалить все компоненты из проекта:

![Диалоговое окно предупреждения с объяснением того, что компонент найден в проекте и должен быть удален](component-nuget-images/component-alert.png)

Чтобы удалить компонент из проекта, выполните следующие действия.

1. Откройте CSPROJ-файл. Для этого щелкните правой кнопкой мыши имя проекта и выберите **сервис > изменить файл**.

2. Найдите в файле все ссылки на `XamarinComponentReference` . Он должен выглядеть следующим образом:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Удалите ссылки на `XamarinComponentReference` и сохраните файл. В приведенном выше примере можно спокойно удалить все`ItemGroup`

4. Повторите описанные выше действия для каждого проекта в решении.

-----

> [!WARNING]
> Следующие инструкции работают только с более старыми версиями Visual Studio.
> Узел **Components** больше не доступен в текущих выпусках Visual Studio 2017 или Visual Studio для Mac.

В следующих разделах объясняется, как обновить существующие решения Xamarin для изменения ссылок на компоненты в пакетах NuGet.

- [Компоненты, содержащие пакеты NuGet](#contain)
- [Компоненты с заменой NuGet](#replace)

Большинство компонентов относятся к одной из указанных выше категорий.
Если вы используете компонент, который не имеет эквивалентного пакета NuGet, ознакомьтесь с разделом [компоненты без пути миграции NuGet](#require-update) ниже.

<a name="contain"></a>

## <a name="components-that-contain-nuget-packages"></a>Компоненты, содержащие пакеты NuGet

Многие компоненты уже содержат пакеты NuGet, а путь миграции — просто удалить ссылку на компонент.

Чтобы определить, включает ли компонент пакет NuGet, дважды щелкнув его в решении:

![Развернутый узел компонентов](component-nuget-images/solution-sml.png)

На вкладке **пакеты** будут перечислены все пакеты NuGet, входящие в компонент:

![Вкладка "пакеты" содержит NuGet](component-nuget-images/packages-tab-sml.png)

Обратите внимание, что вкладка **сборки** будет пустой:

![Вкладка "сборки" пуста](component-nuget-images/assemblies-tab-empty-sml.png)

### <a name="updating-the-solution"></a>Обновление решения

Чтобы обновить решение, удалите запись **компонента** из решения:

![Удалить компонент](component-nuget-images/delete-component-sml.png)

Пакет NuGet будет отображаться в узле **пакеты** , и приложение будет скомпилировано и запущено как обычно. В будущем обновления для этого пакета будут выполняться с помощью функции обновления **NuGet** .

![Обновление пакета NuGet](component-nuget-images/nuget-update-sml.png)

<a name="replace"></a>

## <a name="components-with-nuget-replacements"></a>Компоненты с заменой NuGet

Если вкладка **сборки** сведений о компонентах содержит записи, как показано ниже, необходимо найти эквивалентный пакет NuGet вручную.

![Содержит сборки](component-nuget-images/assemblies-tab-sml.png)

Обратите внимание, что вкладка **пакеты** , вероятно, будет пустой:

![Вкладка "Пакеты"](component-nuget-images/packages-tab-empty-sml.png)

_Он может содержать зависимости NuGet, но их можно игнорировать._

Чтобы проверить, существует ли заменяющий пакет NuGet, выполните поиск по запросу [NuGet.org](https://www.nuget.org/packages), используя имя компонента или автор.

В качестве примера можно найти популярный пакет **SQLite-NET-PCL** , выполнив поиск по следующим параметрам:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl)— Название продукта.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum)— Профиль автора.

### <a name="updating-the-solution"></a>Обновление решения

Убедившись, что компонент доступен в NuGet, выполните следующие действия.

#### <a name="delete-the-component"></a>Удаление компонента

Щелкните правой кнопкой мыши компонент в решении и выберите **Удалить**:

![Удалить компонент](component-nuget-images/remove-component-sml.png)

Это приведет к удалению компонента и всех ссылок. Это приведет к нарушению сборки, пока вы не добавите эквивалентный пакет NuGet, чтобы заменить его.

#### <a name="add-the-nuget-package"></a>Добавление пакета NuGet

1. Щелкните правой кнопкой мыши узел **пакеты** и выберите команду **Добавить пакеты.**...
2. Поиск замены NuGet по имени или автору:

    ![Поиск NuGet](component-nuget-images/nuget-search-sml.png)

3. Нажмите кнопку **Добавить пакет**.

Пакет NuGet будет добавлен в проект вместе с любыми зависимостями.
Это должно исправить сборку. Если сборка продолжится неудачно, изучите каждую ошибку и проверьте, появились ли различия в API между компонентом и пакетом NuGet.

<a name="require-update"></a>

## <a name="components-without-a-nuget-migration-path"></a>Компоненты без пути миграции NuGet

Не стоит беспокоиться, если вы не найдете замену компонентов, используемых в приложении. Существующие компоненты продолжат работать в Visual Studio 15,5, и узел **компоненты** будет отображаться в решении как обычно.

Однако будущие выпуски Visual Studio _не_ будут восстанавливать или обновлять компоненты.
Это означает, что при открытии решения на новом компьютере компонент не будет загружен и установлен. Автор не сможет предоставить вам обновления. Следует запланировать следующее.

- Извлеките сборки из компонента и сослаться на них непосредственно в проекте.
- Обратитесь к автору компонента и попросите о планах перейти на NuGet.
- Изучите альтернативные пакеты NuGet или найдите исходный код, если компонент является открытым исходным кодом.

Многие поставщики компонентов по-прежнему работают над переносом в NuGet, а другие (включая коммерческие продукты) могут изучить альтернативные варианты доставки.

## <a name="related-links"></a>Связанные ссылки

- [Список популярных подключаемых модулей и библиотек Xamarin](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Установка и использование пакета NuGet (Windows)](https://docs.microsoft.com/nuget/quickstart/use-a-package)
- [Включение пакета NuGet (Mac)](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)
