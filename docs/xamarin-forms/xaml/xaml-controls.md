---
title: Элементы управления XAML
description: На все представления, определенные в, Xamarin.Forms можно ссылаться из файлов XAML.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 16186ffcdda5e9d67c736556aa8da3dd8c305732
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93372201"
---
# <a name="xaml-controls"></a>Элементы управления XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/formsgallery)

Представления — это объекты пользовательского интерфейса, такие как метки, кнопки и ползунки, которые обычно называются *элементами управления* или *мини* -приложениями в других графических средах программирования. Представления, поддерживаемые Xamarin.Forms всеми, являются производными от [`View`](xref:Xamarin.Forms.View) класса.

На все представления, определенные в, Xamarin.Forms можно ссылаться из файлов XAML.

## <a name="views-for-presentation"></a>Визуальные элементы для представления данных

| Представление | Пример |
| --- | --- |
| <h3>BoxView</h3>Отображает прямоугольник определенного цвета.<p align="center">![Снимок экрана Боксвиев](xaml-controls-images/BoxView.png "BoxView")</p>[API-интерфейс](xref:Xamarin.Forms.BoxView)  /  [Руководством](~/xamarin-forms/user-interface/boxview.md) | <p valign="center"><pre>&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Эллипс</h3>Отображает эллипс или окружность.<p align="center">![Снимок экрана эллипса](xaml-controls-images/Ellipse.png "Эллипс")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Ellipse)  /  [Руководством](~/xamarin-forms/user-interface/shapes/ellipse.md) | <p valign="center"><pre>&lt;Ellipse Fill="Red"<br />         WidthRequest="150"<br />         HeightRequest="50"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Expander</h3>Предоставляет расширяемый контейнер для размещения любого содержимого.<p align="center">![Снимок экрана: расширитель](xaml-controls-images/Expander.png "Expander")</p>[Руководство](~/xamarin-forms/user-interface/expander.md) | <pre>&lt;Expander&gt;<br />    &lt;Expander.Header&gt;<br />        &lt;Label Text="Baboon" /&gt;<br />    &lt;/Expander.Header&gt;<br />    &lt;Image Source="Baboon.png"<br />           Aspect="AspectFill" /&gt;<br />&lt;/Expander&gt;</pre></p> |
| <h3>Образ —</h3>Отображает точечный рисунок.<p align="center">![Снимок экрана изображения](xaml-controls-images/Image.png "Изображение")</p>[API-интерфейс](xref:Xamarin.Forms.Image)  /  [Руководством](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Метка</h3>Отображает одну или несколько строк текста.<p align="center">![Снимок экрана метки](xaml-controls-images/Label.png "Метка")</p>[API-интерфейс](xref:Xamarin.Forms.Label)  /  [Руководством](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Линия</h3>Отображение линии.<p align="center">![Снимок экрана линии](xaml-controls-images/Line.png "Линия")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Line)  /  [Руководством](~/xamarin-forms/user-interface/shapes/line.md) | <p valign="center"><pre>&lt;Line X1="40"<br />      Y1="0"<br />      X2="0"<br />      Y2="120"<br />      Stroke="Red"<br />      HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Схема</h3>Отображает карту.<p align="center">![Снимок экрана с картой](xaml-controls-images/Map.png "Схема")</p>[API-интерфейс](xref:Xamarin.Forms.Maps.Map)  /  [Руководством](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>MediaElement</h3>Воспроизведение видео или аудио.<p align="center">![Снимок экрана элемента MediaElement](xaml-controls-images/MediaElement.png "MediaELement")</p>[API-интерфейс](xref:Xamarin.Forms.MediaElement)  /  [Руководством](~/xamarin-forms/user-interface/mediaelement.md) | <p valign="center"><pre>&lt;MediaElement Source="https://sec.ch9.ms/ch9/XamarinShow_mid.mp4"<br />              AutoPlay="True"<br />              ShowsPlaybackControls="True" /&gt;</pre></p> |
| <h3>путь</h3>Отображение кривых и сложных фигур.<p align="center">![Снимок экрана пути](xaml-controls-images/Path.png "Path")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Path)  /  [Руководством](~/xamarin-forms/user-interface/shapes/path.md) | <p valign="center"><pre>&lt;Path Stroke="Black"<br />      Aspect="Uniform"<br />      HorizontalOptions="Center"<br />      HeightRequest="100"<br />      WidthRequest="100"<br />      Data="M13.9,16.2<br />            L32,16.2 32,31.9 13.9,30.1Z<br />            M0,16.2<br />            L11.9,16.2 11.9,29.9 0,28.6Z<br />            M11.9,2<br />            L11.9,14.2 0,14.2 0,3.3Z<br />            M32,0<br />            L32,14.2 13.9,14.2 13.9,1.8Z" /&gt;</pre></p> |
| <h3>Многоугольник</h3>Отображение многоугольника.<p align="center">![Снимок экрана многоугольника](xaml-controls-images/Polygon.png "Polygon")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Polygon)  /  [Руководством](~/xamarin-forms/user-interface/shapes/polygon.md) | <p valign="center"><pre>&lt;Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96,<br/>                 50 96, 48 192, 150 200 144 48"<br />         Fill="Blue"<br />         Stroke="Red"<br />         StrokeThickness="3"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Ломаная линия</h3>Отображение ряда Соединенных прямых линий.<p align="center">![Снимок экрана ломаной линии](xaml-controls-images/Polyline.png "Ломаная линия")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Polyline)  /  [Руководством](~/xamarin-forms/user-interface/shapes/Polyline.md) | <p valign="center"><pre>&lt;Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0<br />                  43,60 48,30 100,30"<br />          Stroke="Red"<br />          HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Прямоугольник</h3>Отображение прямоугольника или квадрата.<p align="center">![Снимок экрана прямоугольника](xaml-controls-images/Rectangle.png "Прямоугольник")</p>[API-интерфейс](xref:Xamarin.Forms.Shapes.Rectangle)  /  [Руководством](~/xamarin-forms/user-interface/shapes/rectangle.md) | <p valign="center"><pre>&lt;Rectangle Fill="Red"<br />           WidthRequest="150"<br />           HeightRequest="50"<br />           HorizontalOptions="Center" /&gt;</pre></p> |  
| <h3>WebView</h3>Отображает веб-страницы или содержимое HTML.<p align="center">![Снимок экрана WebView](xaml-controls-images/WebView.png "WebView")</p>[API-интерфейс](xref:Xamarin.Forms.WebView)  /  [Руководством](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Визуальные элементы, инициирующие команды

| Представление | Пример |
| --- | --- |
| <h3>Кнопка</h3>Отображает текст в прямоугольном объекте.<p align="center">![Снимок экрана кнопки](xaml-controls-images/Button.png "Кнопка")</p>[API-интерфейс](xref:Xamarin.Forms.Button)  /  [Руководством](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Отображает изображение в прямоугольном объекте.<p align="center">![Снимок экрана ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API-интерфейс](xref:Xamarin.Forms.ImageButton)  /  [Руководством](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RadioButton</h3>Позволяет выбрать один вариант из набора.<p align="center">![Снимок экрана элемента RadioButton](xaml-controls-images/RadioButton.png "RadioButton")</p>[Руководство](~/xamarin-forms/user-interface/radiobutton.md) | <p valign="center"><pre>&lt;RadioButton Text="Pineapple"<br/>             CheckedChanged="OnRadioButtonCheckedChanged" /&gt;</pre></p> |
| <h3>RefreshView</h3>Предоставляет функции получения по обновлению для прокручиваемого содержимого.<p align="center">![Снимок экрана Рефрешвиев](xaml-controls-images/RefreshView.png "RefreshView")</p>[Руководство](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>Панель поиска</h3> Принимает вводимые пользователем данные, используемые для выполнения поиска.<p align="center">![Снимок экрана Сеарчбар](xaml-controls-images/SearchBar.png "Панель поиска")</p>[Руководство](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder="Enter search term"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> Предоставляет элементы контекстного меню, отображенные с помощью жеста прокрутки.<p align="center">![Снимок экрана Свипевиев](xaml-controls-images/SwipeView.png "SwipeView")</p>[Руководство](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Визуальные элементы для установки значений

| Представление | Пример |
| --- | --- |
| <h3>CheckBox</h3>Позволяет выбрать `boolean` значение.<p align="center">![Снимок экрана флажка](xaml-controls-images/CheckBox.png "CheckBox")</p> [Руководство](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Ползунок</h3>Позволяет выбрать `double` значение из непрерывного диапазона.<p align="center">![Снимок экрана ползунка](xaml-controls-images/Slider.png "Slider")</p>[API-интерфейс](xref:Xamarin.Forms.Slider)  /  [Руководством](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Шаговый переключатель</h3>Позволяет выбрать `double` значение из инкрементного диапазона.<p align="center">![Снимок экрана с пошаговыми средствами](xaml-controls-images/Stepper.png "Шаговый переключатель")</p>[API-интерфейс](xref:Xamarin.Forms.Stepper)  /  [Руководством](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Переключение</h3>Позволяет выбрать `boolean` значение.<p align="center">![Снимок экрана параметра](xaml-controls-images/Switch.png "Параметр")</p>[API-интерфейс](xref:Xamarin.Forms.Switch)  /  [Руководством](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Позволяет выбрать дату.<p align="center">![Снимок экрана DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API-интерфейс](xref:Xamarin.Forms.DatePicker)  /  [Руководством](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Позволяет выбрать время.<p align="center">![Снимок экрана TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API-интерфейс](xref:Xamarin.Forms.TimePicker)  /  [Руководством](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Визуальные элементы для редактирования текста

| Представление | Пример |
| --- | --- |
| <h3>Ввод</h3>Позволяет указать и изменить одну строку текста.<p align="center">![Снимок экрана записи](xaml-controls-images/Entry.png "Ввод")</p>[API-интерфейс](xref:Xamarin.Forms.Entry)  /  [Руководством](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Редактор</h3>Позволяет указать и изменить несколько строк текста.<p align="center">![Снимок экрана редактора](xaml-controls-images/Editor.png "Метка")</p>[API-интерфейс](xref:Xamarin.Forms.Editor)  /  [Руководством](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Визуальные элементы для обозначения действий

| Представление | Пример |
| --- | --- |
| <h3>ActivityIndicator</h3>Отображает анимацию, показывающую, что приложение вовлечено в длительное действие без указания хода выполнения.<p align="center">![Снимок экрана Активитиндикатор](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API-интерфейс](xref:Xamarin.Forms.ActivityIndicator)  /  [Руководством](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Отображает анимацию, показывающую, что приложение проходит продолжительное действие.<p align="center">![Снимок экрана элемента ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API-интерфейс](xref:Xamarin.Forms.ProgressBar)  /  [Руководством](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Визуальные элементы для отображения коллекций

| Представление | Пример |
| --- | --- |
| <h3>CarouselView</h3>Отображает прокручиваемый список элементов данных.<p align="center">![Снимок экрана Карауселвиев](xaml-controls-images/CarouselView.png "CarouselView")</p>[Руководство](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>Отображает прокручиваемый список выбираемых элементов данных с использованием различных спецификаций макета.<p align="center">![Снимок экрана CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Руководство](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />                ItemsLayout="VerticalGrid, 2" /&gt;</pre></p> |
| <h3>IndicatorView</h3>Отображает индикаторы, представляющие количество элементов в `CarouselView` .<p align="center">![Снимок экрана Индикаторвиев](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[Руководство](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView x:Name="indicatorView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>Отображает прокручиваемый список выбираемых элементов данных.<p align="center">![Снимок экрана: ListView](xaml-controls-images/ListView.png "ListView")</p>[API-интерфейс](xref:Xamarin.Forms.ListView)  /  [Руководством](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Средство выбора</h3>Отображает выбранный элемент из списка текстовых строк.<p align="center">![Снимок экрана средства выбора](xaml-controls-images/Picker.png "Средство выбора")</p>[API-интерфейс](xref:Xamarin.Forms.Picker)  /  [Руководством](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&gt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Отображает список интерактивных строк.<p align="center">![Снимок экрана Таблевиев](xaml-controls-images/TableView.png "TableView")</p>[API-интерфейс](xref:Xamarin.Forms.TableView)  /  [Руководством](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Forms Пример Формсгаллери](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Примеры для Xamarin.Forms](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms Документация по API](/dotnet/api/xamarin.forms?view=xamarin-forms)