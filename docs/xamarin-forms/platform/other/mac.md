---
Title: "Настройка платформы Mac" Описание: "в этой статье объясняется, как добавить в проект проект Mac Xamarin.Forms , который будет создавать приложение, которое может выполняться на macOS Sierra и MacOS El Capitan".
MS. произв. Xamarin MS. AssetID: EEC549E0-F182-4F9C-B2BA-B31D19569AA5 MS. Technology: Xamarin-Forms MS. Custom: ксаму — автор видео: давидбритч MS. author: дабритч МС. Дата: 05/03/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="mac-platform-setup"></a>Настройка платформы Mac

![Preview (Предварительный просмотр)](~/media/shared/preview.png)

Перед началом создайте (или используйте существующий) Xamarin.Forms проект. Добавлять можно только приложения Mac с помощью Visual Studio для Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Добавление проекта macOS в Xamarin.Forms видео**

## <a name="adding-a-mac-app"></a>Добавление приложения Mac

Выполните эти инструкции, чтобы добавить приложение Mac, которое будет выполняться на macOS Sierra и macOS El Capitan:

1. В Visual Studio для Mac щелкните существующее решение правой кнопкой мыши Xamarin.Forms и выберите **Добавить > добавить новый проект...**

2. В окне **Новый проект** выберите **Mac > приложение > приложение Cocoa** и нажмите кнопку **Далее**.

3. Введите **имя приложения** (при необходимости выберите другое имя для элемента Dock), а затем нажмите кнопку **Далее**.

4. Проверьте конфигурацию и нажмите кнопку **создать**. Эти шаги показаны ниже.

    ![Анимированные инструкции, демонстрирующие Добавление приложения Cocoa](mac-images/add-macos-proj.gif)

5. В проекте Mac щелкните правой кнопкой мыши **пакеты > добавить пакеты...** , чтобы добавить [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) NuGet. Кроме того, следует обновить другие проекты, чтобы использовать ту же версию Xamarin.Forms пакета NuGet.

6. В проекте Mac щелкните правой кнопкой мыши **ссылки** и добавьте ссылку на Xamarin.Forms проект (общий проект или проект библиотеки .NET Standard).

    ![Добавление ссылки на Xamarin.Forms проект общего кода](mac-images/references-sml.png)

7. Обновите **Main.CS** , чтобы инициализировать `AppDelegate` :

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Обновите `AppDelegate` для инициализации Xamarin.Forms , создайте окно и загрузите Xamarin.Forms приложение (не забудьте задать соответствующий параметр `Title` ). _Если у вас есть другие зависимости, которые необходимо инициализировать, это также можно сделать._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Дважды щелкните **Main. Storyboard** для редактирования в Xcode. Выберите **окно** и снимите _флажок_ **является начальным контроллером** (это связано с тем, что в приведенном выше коде создается окно):

    [![Снимите флажок "является начальным контроллером" в Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    Вы можете изменить систему меню в раскадровке, чтобы удалить ненужные элементы.

10. Наконец, добавьте локальные ресурсы (например, файлы изображений) из существующих проектов платформы, которые необходимы.

11. Теперь проект Mac должен выполнять Xamarin.Forms код в macOS!

## <a name="next-steps"></a>Next Steps

### <a name="styling"></a>Задание стиля

Последние изменения, внесенные в `OnPlatform` , теперь можно ориентировать на любое количество платформ. В том числе macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Обратите внимание, что вы также можете удвоить такие платформы: `<On Platform="iOS, macOS" ...>` .

### <a name="window-size-and-position"></a>Размер и расположение окна

Начальный размер и расположение окна можно изменить в `AppDelegate` :

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Известные проблемы

Это предварительная версия, поэтому следует рассчитывать на то, что все готово к производству. Ниже приведены некоторые моменты, которые могут возникнуть при добавлении macOS в проекты.

### <a name="not-all-nugets-are-ready-for-macos"></a>Не все NuGet готовы для macOS

Возможно, некоторые библиотеки, которые вы используете, еще не поддерживают macOS. В этом случае необходимо отправить запрос в сопровождение проекта, чтобы добавить его. Пока у них не будет поддержки, может потребоваться найти альтернативные варианты.

### <a name="missing-xamarinforms-features"></a>Отсутствующие Xamarin.Forms компоненты

Xamarin.FormsВ этой предварительной версии не все функции завершены. Дополнительные сведения см. в разделе [поддержка платформ MacOS состояние](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) в [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) репозитории GitHub.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Mac](~/mac/index.yml)
