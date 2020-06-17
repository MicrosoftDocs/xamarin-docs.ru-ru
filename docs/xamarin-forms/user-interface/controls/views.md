---
Title: " Xamarin.Forms views" Description: " Xamarin.Forms представления — это стандартные блоки кросс-платформенных мобильных пользовательских интерфейсов. В этой статье перечислены представления, входящие в Xamarin.Forms .
MS. произв. Xamarin MS. AssetID: AC070686-A423-4A98-8BB6-0B9F94C062CC MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 04/16/2020 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-views"></a>Xamarin.FormsПредставления

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Представления Xamarin. Forms — это стандартные блоки межплатформенных мобильных пользовательских интерфейсов._

Представления — это объекты пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементами управления* или *мини* -приложениями в других графических средах программирования. Представления, поддерживаемые Xamarin.Forms всеми, являются производными от [`View`](xref:Xamarin.Forms.View) класса. Их можно разделить на несколько категорий:

## <a name="views-for-presentation"></a>Визуальные элементы для представления данных

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)отображает сплошной прямоугольник, окрашенный в [`Color`](xref:Xamarin.Forms.BoxView.Color) свойство. `BoxView`по умолчанию имеет запрос размера 40x40. Для других размеров присвойте [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Свойства и [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .<br /><br />[Документация по API](xref:Xamarin.Forms.BoxView)  /  [Руководством](~/xamarin-forms/user-interface/boxview.md)  /  [Примеры 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)и [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Пример Боксвиев](views-images/BoxView.png "Пример Боксвиев")](views-images/BoxView-Large.png#lightbox "Пример Боксвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| `Expander`предоставляет расширяемый контейнер для размещения любого содержимого и состоит из заголовка и содержимого. Задайте `Header` для свойства значение [`View`](xref:Xamarin.Forms.View) , которое будет отображаться в качестве заголовка, а `Content` свойство — [`View`](xref:Xamarin.Forms.View) , которое будет отображаться при раскрытии заголовка касанием.<br /><br />[Руководством](~/xamarin-forms/user-interface/expander.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![Пример расширителя](views-images/Expander.png "Пример расширителя")](views-images/Expander-Large.png#lightbox "Пример расширителя")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml) |
|     |     |

### <a name="label"></a>Метка

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)Отображает однострочные текстовые строки или многострочные блоки текста с постоянным форматированием или переменной. Присвойте [`Text`](xref:Xamarin.Forms.Label.Text) свойству значение String для постоянного форматирования или задайте [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) для свойства [`FormattedString`](xref:Xamarin.Forms.FormattedString) объект для форматирования переменных.<br /><br />[Документация по API](xref:Xamarin.Forms.Label)  /  [Руководством](~/xamarin-forms/user-interface/text/label.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример метки](views-images/Label.png "Пример метки")](views-images/Label-Large.png#lightbox "Пример метки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Изображение

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)отображает точечный рисунок. Точечные рисунки можно загружать через Интернет, внедрять как ресурсы в проекты общих проектов или платформ или создавать с помощью `Stream` объекта .NET.<br /><br />[Документация по API](xref:Xamarin.Forms.Image)  /  [Руководством](~/xamarin-forms/user-interface/images.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Пример изображения](views-images/Image.png "Пример изображения")](views-images/Image-Large.png#lightbox "Пример изображения")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="map"></a>Схема

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)отображает карту. Объект ** Xamarin.Forms . **Должен быть установлен пакет NuGet Maps. Для Android и универсальная платформа Windows требуется ключ авторизации на карте.<br /><br />[Документация по API](xref:Xamarin.Forms.Maps.Map)  /  [Руководством](~/xamarin-forms/user-interface/map/index.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Пример Map](views-images/Map.png "Пример Map")](views-images/Map-Large.png#lightbox "Пример Map")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)воспроизводит видео или аудио. Мультимедиа можно воспроизводить с URL-адреса или из локального файла в зависимости от того, [`Source`](xref:Xamarin.Forms.MediaElement.Source) задано ли для свойства значение [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) или [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) .<br /><br />[Документация по API](xref:Xamarin.Forms.MediaElement)  /  [Руководством](~/xamarin-forms/user-interface/mediaelement.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![Пример MediaElement](views-images/MediaElement.png "Пример MediaElement")](views-images/MediaElement-Large.png#lightbox "Пример MediaElement")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>опенглвиев

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)Отображает графические изображения OpenGL в проектах iOS и Android. Универсальная платформа Windows не поддерживается. Для проектов iOS и Android требуется ссылка на сборку **опентк-1,0** или сборку **опентк** версии 1.0.0.0. `OpenGLView`проще в использовании в общем проекте; Если используется в библиотеке .NET Standard, потребуется также служба зависимостей (как показано в примере кода).<br /><br />Это единственная графическая возможность, встроенная в Xamarin.Forms , но Xamarin.Forms приложение может также визуализировать графику с помощью [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) , или [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md) .<br /><br />[Документация по API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Пример Опенглвиев](views-images/OpenGLView.png "Пример Опенглвиев")](views-images/OpenGLView-Large.png#lightbox "Пример Опенглвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)отображает веб-страницы или содержимое HTML в зависимости от того, [`Source`](xref:Xamarin.Forms.WebView.Source) задано ли для свойства [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) объект или.<br /><br />[Документация по API](xref:Xamarin.Forms.WebView)  /  [Руководством](~/xamarin-forms/user-interface/webview.md)  /  [Пример 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) и [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![Пример WebView](views-images/WebView.png "Пример WebView")](views-images/WebView-Large.png#lightbox "Пример WebView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Визуальные элементы, инициирующие команды

### <a name="button"></a>Кнопка

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)прямоугольный объект, отображающий текст, который запускает [`Clicked`](xref:Xamarin.Forms.Button.Clicked) событие при нажатии.<br /><br />[Документация по API](xref:Xamarin.Forms.Button)  /  [Руководством](~/xamarin-forms/user-interface/button.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Пример кнопки](views-images/Button.png "Пример кнопки")](views-images/Button-Large.png#lightbox "Пример кнопки")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`прямоугольный объект, отображающий изображение, который запускает `Clicked` событие при нажатии.<br /><br /> [Руководством](~/xamarin-forms/user-interface/imagebutton.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Пример с ImageButton](views-images/ImageButton.png "Пример с ImageButton")](views-images/ImageButton-Large.png#lightbox "Пример с ImageButton")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| `RadioButton`позволяет выбрать один вариант из набора и запускает `CheckedChanged` событие при выборе.<br /><br />[Руководством](~/xamarin-forms/user-interface/radiobutton.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![Пример RadioButton](views-images/RadioButton.png "Пример RadioButton")](views-images/RadioButton-Large.png#lightbox "Пример RadioButton")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`— это контейнерный элемент управления, который предоставляет функции сквозного обновления для прокручиваемого содержимого. Объект, `ICommand` определяемый `Command` свойством, выполняется при активации обновления, а `IsRefreshing` свойство указывает текущее состояние элемента управления.<br /><br /> [Руководством](~/xamarin-forms/user-interface/refreshview.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Пример Рефрешвиев](views-images/RefreshView.png "Пример Рефрешвиев")](views-images/RefreshView-Large.png#lightbox "Пример Рефрешвиев")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>Панель поиска

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)Отображает область, в которой пользователь должен ввести текстовую строку, и кнопку (или клавишу с клавиатуры), которая сигнализирует приложению выполнить поиск. [`Text`](xref:Xamarin.Forms.InputView.Text)Свойство предоставляет доступ к тексту, а [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) событие указывает на нажатую кнопку.<br /><br />[Документация по API](xref:Xamarin.Forms.SearchBar)  /  [Руководством](~/xamarin-forms/user-interface/searchbar.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![Пример Сеарчбар](views-images/SearchBar.png "Пример Сеарчбар")](views-images/SearchBar-Large.png#lightbox "Пример Сеарчбар")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`— это контейнерный элемент управления, который обходит элемент содержимого и предоставляет элементы контекстного меню, которые выводятся с помощью жеста прокрутки. Каждый элемент меню представлен `SwipeItem` свойством, который имеет `Command` свойство, выполняющееся `ICommand` при касании элемента.<br /><br /> [Руководством](~/xamarin-forms/user-interface/swipeview.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Пример Свипевиев](views-images/SwipeView.png "Пример Свипевиев")](views-images/SwipeView-Large.png#lightbox "Пример Свипевиев")<br /> [Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Визуальные элементы для установки значений

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`позволяет пользователю выбрать логическое значение с помощью типа кнопки, которая может быть либо установлена, либо пустой. `IsChecked`Свойство является состоянием `CheckBox` , а `CheckedChanged` событие возникает при изменении состояния.<br /><br />Документация по API [Guide](~/xamarin-forms/user-interface/checkbox.md)/  /  [образец](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) руководства | [![Пример флажка](views-images/CheckBox.png "Пример флажка")](views-images/CheckBox-Large.png#lightbox "Пример флажка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)позволяет пользователю выбрать `double` значение из непрерывного диапазона, указанного с помощью [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) свойств и.<br /><br />[Документация по API](xref:Xamarin.Forms.Slider)  /  [Руководством](~/xamarin-forms/user-interface/slider.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Пример ползунка](views-images/Slider.png "Пример ползунка")](views-images/Slider-Large.png#lightbox "Пример ползунка")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Шаговый переключатель

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)позволяет пользователю выбрать `double` значение из диапазона добавочных значений, указанных с помощью [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) свойств, и [`Increment`](xref:Xamarin.Forms.Stepper.Increment) .<br /><br />[Документация по API](xref:Xamarin.Forms.Stepper)   /  [Руководством](~/xamarin-forms/user-interface/stepper.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Пример многошагового режима](views-images/Stepper.png "Пример многошагового режима")](views-images/Stepper-Large.png#lightbox "Пример многошагового режима")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Параметр

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)принимает вид переключателя вкл/выкл, чтобы позволить пользователю выбрать логическое значение. [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)Свойство является состоянием переключателя, и [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) событие возникает при изменении состояния.<br /><br />[Документация по API](xref:Xamarin.Forms.Switch)  /  [Руководством](~/xamarin-forms/user-interface/switch.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Пример параметра](views-images/Switch.png "Пример параметра")](views-images/Switch-Large.png#lightbox "Пример параметра")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)позволяет пользователю выбрать дату с помощью средства выбора даты платформы. Задайте диапазон допустимых дат с помощью [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) свойств и. [`Date`](xref:Xamarin.Forms.DatePicker.Date)Свойство является выбранной датой, и [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) событие возникает при изменении этого свойства.<br /><br />[Документация по API](xref:Xamarin.Forms.DatePicker)  /  [Руководством](~/xamarin-forms/user-interface/datepicker.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![Пример DatePicker](views-images/DatePicker.png "Пример DatePicker")](views-images/DatePicker-Large.png#lightbox "Пример DatePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)позволяет пользователю выбрать время с помощью средства выбора времени платформы. [`Time`](xref:Xamarin.Forms.TimePicker.Time)Свойство выбрано по времени. Приложение может отслеживать изменения в `Time` свойстве, устанавливая обработчик для [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) события.<br /><br />[Документация по API](xref:Xamarin.Forms.TimePicker)  /  [Руководством](~/xamarin-forms/user-interface/timepicker.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![Пример TimePicker](views-images/TimePicker.png "Пример TimePicker")](views-images/TimePicker-Large.png#lightbox "Пример TimePicker")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Визуальные элементы для редактирования текста

Эти два класса являются производными от [`InputView`](xref:Xamarin.Forms.InputView) класса, который определяет [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) свойство.

### <a name="entry"></a>Ввод

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)позволяет пользователю вводить и редактировать одну строку текста. Текст доступен в качестве [`Text`](xref:Xamarin.Forms.InputView.Text) свойства, а [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) [`Completed`](xref:Xamarin.Forms.Entry.Completed) события и запускаются при изменении текста или при завершении пользователем путем нажатия клавиши ВВОД.<br /><br />Используйте [`Editor`](#editor) для ввода и редактирования нескольких строк текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Entry)  /  [Руководством](~/xamarin-forms/user-interface/text/entry.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Entry.png "Пример записи")](views-images/Entry-Large.png#lightbox "Пример записи")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>Редактор

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)позволяет пользователю вводить и редактировать несколько строк текста. Текст доступен в качестве [`Text`](xref:Xamarin.Forms.InputView.Text) свойства, а [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) [`Completed`](xref:Xamarin.Forms.Editor.Completed) события и запускаются при изменении текста или при завершении пользовательского сигнала.<br /><br />Используйте [`Entry`](#entry) представление для ввода и редактирования одной строки текста.<br /><br />[Документация по API](xref:Xamarin.Forms.Editor)  /  [Руководством](~/xamarin-forms/user-interface/text/editor.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Пример записи](views-images/Editor.png "Пример редактора")](views-images/Editor-Large.png#lightbox "Пример редактора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Визуальные элементы для обозначения действий

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)использует анимацию для индикации того, что приложение вовлечено в длительное действие без указания хода выполнения. [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)Свойство управляет анимацией.<br /><br />Если ход выполнения действия известен, используйте [`ProgressBar`](#progressbar) вместо него.<br /><br />[Документация по API](xref:Xamarin.Forms.ActivityIndicator)  /  [Руководством](~/xamarin-forms/user-interface/activityindicator.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![Пример Активитиндикатор](views-images/ActivityIndicator.png "Пример Активитиндикатор")](views-images/ActivityIndicator-Large.png#lightbox "Пример Активитиндикатор")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)использует анимацию, чтобы продемонстрировать, что приложение проходит продолжительное действие. Задайте [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) для свойства значение от 0 до 1, чтобы указать ход выполнения.<br /><br />Если ход выполнения действия неизвестен, используйте [`ActivityIndicator`](#activityindicator) вместо него.<br /><br />[Документация по API](xref:Xamarin.Forms.ProgressBar)  /  [Руководством](~/xamarin-forms/user-interface/progressbar.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![Пример для ProgressBar](views-images/ProgressBar.png "Пример для ProgressBar")](views-images/ProgressBar-Large.png#lightbox "Пример для ProgressBar")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Визуальные элементы для отображения коллекций

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)Отображает прокручиваемый список элементов данных. Присвойте `ItemsSource` свойству коллекцию объектов и задайте `ItemTemplate` для свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объект, описывающий способ форматирования элементов. `CurrentItemChanged`Событие сигнализирует, что отображаемый в данный момент элемент был изменен, который доступен как `CurrentItem` свойство.<br /><br />[Руководством](~/xamarin-forms/user-interface/carouselview/index.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![Пример Карауселвиев](views-images/CarouselView.png "Пример Карауселвиев")](views-images/CarouselView-Large.png#lightbox "Пример Карауселвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)Отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView) . Присвойте `ItemsSource` свойству коллекцию объектов и задайте `ItemTemplate` для свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объект, описывающий способ форматирования элементов. `SelectionChanged`Событие сигнализирует, что сделан выбор, который доступен как `SelectedItem` свойство.<br /><br />[Руководством](~/xamarin-forms/user-interface/collectionview/index.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![Пример CollectionView](views-images/CollectionView.png "Пример CollectionView")](views-images/CollectionView-Large.png#lightbox "Пример CollectionView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView`Отображает индикаторы, представляющие количество элементов в `CarouselView` . Задайте `CarouselView.IndicatorView` для свойства объект, `IndicatorView` чтобы отобразить индикаторы для `CarouselView` . <br /><br />[Руководством](~/xamarin-forms/user-interface/indicatorview.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![Пример Индикаторвиев](views-images/IndicatorView.png "Пример Индикаторвиев")](views-images/IndicatorView-Large.png#lightbox "Пример Индикаторвиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)является производным от [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) и отображает прокручиваемый список выбираемых элементов данных. Присвойте [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) свойству коллекцию объектов и задайте [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) для свойства [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объект, описывающий способ форматирования элементов. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)Событие сигнализирует, что сделан выбор, который доступен как [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) свойство.<br /><br />[Документация по API](xref:Xamarin.Forms.ListView)  /  [Руководством](~/xamarin-forms/user-interface/listview/index.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![Пример ListView](views-images/ListView.png "Пример ListView")](views-images/ListView-Large.png#lightbox "Пример ListView")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Средство выбора

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)Отображает выбранный элемент из списка текстовых строк и позволяет выбрать этот элемент при касании представления. Присвойте [`Items`](xref:Xamarin.Forms.Picker.Items) свойству список строк или [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) свойство коллекции объектов. [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)Событие возникает при выборе элемента.<br /><br />`Picker`Отображает список элементов, только если он выбран. Используйте [`ListView`](#listview) или [`TableView`](#tableview) для прокручиваемого списка, который остается на странице.<br /><br />[Документация по API](xref:Xamarin.Forms.Picker)  /  [Руководством](~/xamarin-forms/user-interface/picker/index.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Пример средства выбора](views-images/Picker.png "Пример средства выбора")](views-images/Picker-Large.png#lightbox "Пример средства выбора")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) с [кодом программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)Отображает список строк типа [`Cell`](xref:Xamarin.Forms.Cell) с необязательными заголовками и подзаголовками. Задайте [`Root`](xref:Xamarin.Forms.TableView.Root) для свойства объект типа [`TableRoot`](xref:Xamarin.Forms.TableRoot) и добавьте [`TableSection`](xref:Xamarin.Forms.TableSection) в него объекты `TableRoot` . Каждый `TableSection` представляет собой коллекцию `Cell` объектов.<br /><br />[Документация по API](xref:Xamarin.Forms.TableView)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md)  /  [Пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![Пример Таблевиев](views-images/TableView.png "Пример Таблевиев")](views-images/TableView-Large.png#lightbox "Пример Таблевиев")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs)  /  [Страница XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsПример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для Xamarin.Forms](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsДокументация по API](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
