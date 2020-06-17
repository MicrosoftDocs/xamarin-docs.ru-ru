---
title: "Удаление Xamarin" description: "Этот документ описывает удаление Xamarin из Visual Studio в Windows".
ms.prod: xamarin ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b author: conceptdev ms.author: crdun ms.date: 22.01.2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="uninstall-xamarin-from-visual-studio"></a>Удаление Xamarin из Visual Studio

Это руководство поясняет, как удалить Xamarin из Visual Studio в Windows.

<a name="uninstallvs2017"></a>

## <a name="visual-studio-2019-and-visual-studio-2017"></a>Visual Studio 2019 и Visual Studio 2017

Xamarin можно удалить из Visual Studio 2019 и Visual Studio 2017 с помощью приложения установщика.

1. Откройте **установщик Visual Studio** из меню **Пуск**.

2. Нажмите кнопку **Изменить**, предварительно выбрав изменяемый экземпляр.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Press the modify button")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. На вкладке **Рабочие нагрузки** отмените выбор параметра **Разработка мобильных приложений на платформе .NET** (в разделе **Мобильные приложения и игры**).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Uncheck the Mobile Development workload")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Нажмите кнопку **Изменить** в правом нижнем углу окна.

5. Установщик удалит компоненты, выбор которых отменен (прежде чем установщик сможет вносить изменения, необходимо закрыть Visual Studio 2017).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Press the Modify button")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Чтобы удалить отдельные компоненты Xamarin (например, Profiler или Workbooks), на шаге 3 перейдите на вкладку **Отдельные компоненты** и отмените их выбор.

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Uninstall individual components")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Чтобы удалить Visual Studio 2017 полностью, в трехстрочном меню рядом с кнопкой **Запустить** выберите пункт **Удалить**.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Uninstall Visual Studio completely")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> При наличии двух (или более) параллельно установленных (SxS) экземпляров Visual Studio (например, версии выпуска и предварительной версии) удаление одного экземпляра может привести к удалению некоторых компонентов Xamarin из других экземпляров Visual Studio, включая:
>
> - Xamarin Profiler
> - Xamarin Workbooks или Inspector
> - Симулятор iOS удаленной работы для Xamarin
> - Пакет SDK для Apple Bonjour
>
> При определенных условиях удаление одного из параллельно установленных экземпляров может привести к некорректному удалению этих компонентов. В результате может быть снижена производительность платформы Xamarin в экземплярах Visual Studio, оставшихся в системе после удаления экземпляра SxS.
>
>Для решения этой проблемы запустите в установщике Visual Studio функцию **Исправить**, которая повторно установит отсутствующие компоненты.

<a name="uninstallvs2015"></a>

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 и более ранние версии

Инструкции по полному удалению Visual Studio 2015 см. в [ответе службы поддержки на сайте visualstudio.com](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin можно удалить с компьютера Windows через **панель управления**. Перейдите к элементу **Программы и компоненты** или **Программы > Удаление программы**, как показано ниже:

 [![](uninstalling-xamarin-images/image3.png "Navigate to Programs and Features or Programs  Uninstall a Program as illustrated here")](uninstalling-xamarin-images/image3.png#lightbox)

В панели управления удалите любые из следующих имеющихся компонентов.

- Xamarin
- Xamarin для Windows
- Xamarin.Android
- Xamarin.iOS
- Xamarin для Visual Studio

В обозревателе удалите все оставшиеся файлы из папок расширения Xamarin для Visual Studio (все версии, включая обе папки — Program Files и Program Files (x86)).

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Удалите каталог кэша компонента MEF для Visual Studio, который должен находиться здесь.

```
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Проверьте, содержит ли каталог **VirtualStore** какие-либо сохраненные Windows файлы наложения для каталогов **Extensions\Xamarin** или **ComponentModelCache**:

```
%LOCALAPPDATA%\VirtualStore
```

Откройте редактор реестра (regedit) и найдите следующий раздел.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Найдите и удалите все записи, соответствующие этому шаблону.

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Найдите следующий раздел.

```
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Удалите все записи, которые могут иметь отношение к Xamarin. Например, все записи, содержащие термины `mono` или `xamarin`.

Откройте командную строку (cmd.exe) от имени администратора и выполните команды `devenv /setup` и `devenv /updateconfiguration` для каждой установленной версии Visual Studio. Например, для Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```
