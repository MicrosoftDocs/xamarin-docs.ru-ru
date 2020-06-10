---
Title: "Каталог изображений по умолчанию в Windows" Описание: "особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать особенности платформы Windows, определяющие каталог в проекте, из которого будут загружаться ресурсы изображений.
MS. произв. Xamarin MS. AssetID: 537A032B-74DD-4D43-864E-7D7113286D0D MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 01/16/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="default-image-directory-on-windows"></a>Каталог изображений по умолчанию в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows зависит от конкретной платформы определяет каталог в проекте, из которого будут загружаться ресурсы изображений. Он используется в XAML путем задания для параметра значения `Application.ImageDirectory` `string` , представляющего каталог проекта, в котором содержатся ресурсы изображений:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
             ...
             windows:Application.ImageDirectory="Assets">
    ...
</Application>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
Application.Current.On<Windows>().SetImageDirectory("Assets");
```

`Application.On<Windows>`Метод указывает, что данная платформа будет запускаться только на универсальная платформа Windows. `Application.SetImageDirectory`Метод в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для указания каталога проекта, из которого будут загружаться образы. Кроме того, `GetImageDirectory` метод можно использовать для возврата `string` , который представляет каталог проекта, содержащий ресурсы образа приложения.

В результате все изображения, используемые в приложении, будут загружены из указанного каталога проекта.

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
