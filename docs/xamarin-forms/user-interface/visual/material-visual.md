---
Title: " Xamarin.Forms Описание визуального элемента" материалы ". Xamarin.Forms визуальный элемент" материалы "можно использовать для создания Xamarin.Forms приложений, которые выглядят примерно одинаково в iOS и Android."
MS. произв. Xamarin MS. AssetID: B774F68C-EF9E-49E1-B738-CDC64879ADA2 MS. Technology: Xamarin-Forms author: профексоржеек MS. author: жусжохнс МС. Дата: 11/25/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-material-visual"></a>Визуальный элемент материала Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Проектирование материалов](https://material.io) — это упрямого система разработки, созданная Google, которая предписывает размер, цвет, расстояния и другие аспекты того, как должны выглядеть и работать представления и макеты.

Xamarin.FormsВизуальный элемент "материалы" можно использовать для применения правил проектирования материалов к Xamarin.Forms приложениям, создавая приложения, которые выглядят примерно одинаково в iOS и Android. Если визуальный элемент "материалы" включен, Поддерживаемые представления применяют один и тот же проект кросс-платформенный режим, создавая единообразный внешний вид.

[![Визуальные снимки экрана материалов](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

Процесс включения Xamarin.Forms визуального элемента "материалы" в приложении:

1. Добавьте [ Xamarin.Forms . Пакет NuGet Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) для проектов на платформе iOS и Android. Этот пакет NuGet предоставляет оптимизированные для разработки материалов модули подготовки отчетов в iOS и Android. В iOS пакет предоставляет транзитивное зависимость для [Xamarin. iOS. материалкомпонентс](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), которая представляет собой привязку C# к [компонентам материалов Google для iOS](https://material.io/develop/ios/). В Android пакет предоставляет целевые объекты сборки, чтобы обеспечить правильную настройку TargetFramework.
1. Инициализация визуального элемента "материалы" в каждом проекте платформы. Дополнительные сведения см. в разделе [Инициализация визуального элемента Material](#initialize-material-visual).
1. Создайте визуальные элементы управления "материал", задав [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) для свойства значение `Material` на всех страницах, которые должны применять правила дизайна материалов. Дополнительные сведения см. в разделе Использование модулей подготовки отчетов к [просмотру](#apply-material-visual).
1. используемых Настройка элементов управления материалами. Дополнительные сведения см. в разделе [Настройка элементов управления материалами](#customize-material-visual).

> [!IMPORTANT]
> В Android визуальный элемент "материал" требует минимальную версию 5,0 (API 21) или более TargetFramework, а также версию 9,0 (API 28). Кроме того, проект платформы требует наличия библиотек поддержки Android 28.0.0 или более поздней версии, и его тема должна наследовать от темы "компоненты материала" или продолжить наследовать от темы AppCompat. Дополнительные сведения см. в статье [Начало работы с материальными компонентами для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Визуальный элемент "материалы" в настоящее время поддерживает следующие элементы управления:

- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Button`](xref:Xamarin.Forms.Button)
- [`CheckBox`](xref:Xamarin.Forms.CheckBox)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

Элементы управления материалами реализуются модулями подготовки материалов, которые применяют правила проектирования материалов. Функционально, модули подготовки материалов не отличаются от модулей подготовки отчетов по умолчанию. Дополнительные сведения см. в разделе [Настройка визуального элемента Material](#customize-material-visual).

## <a name="initialize-material-visual"></a>Инициализация визуального элемента материалов

После установки [ Xamarin.Forms . Пакет NuGet для Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) , модули подготовки материалов должны быть инициализированы в каждом проекте платформы.

В iOS это должно произойти в **AppDelegate.CS** путем вызова `Xamarin.Forms.FormsMaterial.Init` метода *после* `Xamarin.Forms.Forms.Init` метода:

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

В Android это должно произойти в **MainActivity.CS** путем вызова `Xamarin.Forms.FormsMaterial.Init` метода *после* `Xamarin.Forms.Forms.Init` метода:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="apply-material-visual"></a>Применение визуального элемента "материалы"

Приложения могут включить визуальный элемент "материалы", задав [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) свойство на странице, макете или представлении, чтобы `Material` :

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Установка `VisualElement.Visual` свойства для `Material` направления приложения на использование визуальных модулей подготовки материалов к просмотру вместо модулей подготовки отчетов по умолчанию. [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)Свойству можно задать любой тип, реализующий `IVisual` , с [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) классом, который предоставляет следующие `IVisual` Свойства:

- `Default`— Указывает, что представление должно отображаться с помощью модуля подготовки отчетов по умолчанию.
- `MatchParent`— Указывает, что представление должно использовать тот же модуль подготовки отчетов, что и его непосредственного родителя.
- `Material`— Указывает, что представление должно быть отображено с помощью модуля подготовки материалов.

> [!IMPORTANT]
> [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)Свойство определяется в [`VisualElement`](xref:Xamarin.Forms.VisualElement) классе с представлениями, которые наследуют `Visual` значение свойства от своих родителей. Таким образом, установка `Visual` свойства для [`ContentPage`](xref:Xamarin.Forms.ContentPage) гарантирует, что все поддерживаемые представления на странице будут использовать этот визуальный элемент. Кроме того, `Visual` свойство может быть переопределено в представлении.

На следующих снимках экрана показан пользовательский интерфейс, отображаемый с помощью модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию в iOS и Android](material-visual-images/default-renderers.png "Представления, использующие модули подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показан один и тот же пользовательский интерфейс, отображаемый с помощью модулей подготовки материалов:

[![Снимок экрана модулей подготовки материалов в iOS и Android](material-visual-images/material-renderers.png "Представления, использующие модули подготовки материалов")](material-visual-images/material-renderers-large.png#lightbox)

Основные видимые различия между модулями подготовки отчетов по умолчанию и модулями подготовки материалов, показанными здесь, заключаются в том, что модули подготовки отчетов записывают [`Button`](xref:Xamarin.Forms.Button) текст и округляют углы [`Frame`](xref:Xamarin.Forms.Frame) границ. Однако модули подготовки материалов используют собственные элементы управления, поэтому для таких областей, как шрифты, тени, цвета и повышения прав, могут существовать различия в пользовательском интерфейсе.

> [!NOTE]
> Компоненты проектирования материалов тесно соответствуют рекомендациям Google. В результате визуализаторы дизайна материалов перемещаются в сторону изменения размера и поведения. Если требуется больший контроль над стилями или поведением, можно по-прежнему создать собственный [результат](~/xamarin-forms/app-fundamentals/effects/index.md), [поведение](~/xamarin-forms/app-fundamentals/behaviors/index.md)или [пользовательский модуль подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для получения необходимых сведений.

## <a name="customize-material-visual"></a>Настройка визуального элемента "материалы"

Пакет для визуального элемента NuGet — это коллекция модулей подготовки отчетов, которые реализуют Xamarin.Forms элементы управления. Настройка визуальных элементов управления "материал" идентична настройке элементов управления по умолчанию.

Эффекты являются рекомендуемым методом, когда целью является Настройка существующего элемента управления. Если существует визуальный модуль подготовки материалов, то для настройки элемента управления с применением этого средства будет меньше работы, чем подкласс модуля подготовки отчетов. Дополнительные сведения о влиянии см. в разделе [ Xamarin.Forms эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

Пользовательские модули подготовки отчетов являются рекомендуемым методом, если модуль подготовки материалов не существует. В визуальный элемент Material включены следующие классы модуля подготовки отчетов:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

Подкласс модуля подготовки материалов практически идентичен модулям визуализации, не связанным с материалами. Однако при экспорте модуля подготовки отчетов, подклассов модуля подготовки отчетов, необходимо предоставить третий аргумент для `ExportRenderer` атрибута, указывающего `VisualMarker.MaterialVisual` Тип:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        //...
    }
}
```

В этом примере объект `ExportRendererAttribute` указывает, что `CustomMaterialProgressBarRenderer` класс будет использоваться для отрисовки [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) представления с `IVisual` типом, зарегистрированным в качестве третьего аргумента.

> [!NOTE]
> Модуль подготовки отчетов, указывающий `IVisual` тип (как часть его `ExportRendererAttribute` ), будет использоваться для визуализации входящих в представления, а не для модуля подготовки к просмотру по умолчанию. Во время выбора модуля визуализации `Visual` свойство представления проверяется и включается в процесс выбора модуля подготовки отчетов.

Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Создание Xamarin.Forms визуального модуля подготовки отчетов](create.md)
- [Xamarin.FormsПараметров](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
