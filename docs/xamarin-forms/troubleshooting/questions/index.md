---
Title: " Xamarin.Forms часто задаваемые вопросы" MS. Topic: Устранение неполадок MS. произв. Xamarin MS. AssetID: 89364175-53BA-4A09-B3E2-44AC67DD971C MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч MS. Дата: 04/25/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.FormsЧасто задаваемые вопросы

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?](update-forms-template.md)
В этом руководством в Xamarin.Forms качестве примера используется шаблон библиотеки .NET Standard, но один и тот же общий метод также будет работать для Xamarin.Forms шаблона общего проекта.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?](forms-xaml-designer.md)
Xamarin.FormsСейчас не поддерживает визуальные конструкторы для файлов XAML.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Ошибка сборки Android: непредвиденный сбой задачи "Линкассемблиес"](android-linkassemblies-error.md)
`The "LinkAssemblies" task failed unexpectedly`При построении проекта Xamarin. Android, использующего формы, может появиться сообщение об ошибке. Это происходит, если компоновщик активен (обычно в сборке *выпуска* , чтобы уменьшить размер пакета приложения); Это происходит потому, что целевые объекты Android не обновляются до последней версии платформы. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>[«Почему Xamarin.Forms . Произошел сбой проекта Android с КОМПИЛЕТОДАЛВИК: НЕПРЕДВИДЕНная ошибка верхнего уровня? "](maps-compiletodalvik-error.md)
Эта ошибка может отображаться на панели ошибок Visual Studio для Mac или в окне вывода сборки Visual Studio. в проектах Android с помощью Xamarin.Forms . Указания. Чаще всего это устраняется путем увеличения размера кучи Java для проекта Xamarin. Android.
