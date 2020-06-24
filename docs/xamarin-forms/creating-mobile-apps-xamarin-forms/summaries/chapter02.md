---
title: Сводка по главе 2. Анатомия приложения
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводка по главе 2. Анатомия приложения
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 78da3ed91acea0c056074d712d368de70b251392
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136924"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Сводка по главе 2. Анатомия приложения

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> Примечания на этой странице указывают области, в которых Xamarin.Forms имеет расхождения с материалом, представленным в книге.

В приложении Xamarin.Forms любые объекты, занимающие место на экране, называются *визуальными элементами* и инкапсулируются в класс [`VisualElement`](xref:Xamarin.Forms.VisualElement). Визуальные элементы можно разделить на три категории, соответствующие следующим классам:

- [Страница](xref:Xamarin.Forms.Page)
- [Макет](xref:Xamarin.Forms.Layout)
- [Вид](xref:Xamarin.Forms.View)

Производные `Page` занимают весь или почти весь экран. Дочерний объект страницы обычно наследуется от `Layout` и служит для организации дочерних визуальных элементов. Дочерние объекты `Layout` также могут быть классами `Layout`или производными от `View`, и тогда они называются *элементами*. Сюда относятся текстовые поля, точечные рисунки, ползунки, кнопки, списки и т. д.

В этой главе показано, как создать приложение на основе [`Label`](xref:Xamarin.Forms.Label), — производного элемента от `View` для отображения текста.

## <a name="say-hello"></a>Начнем с приветствия

После установки платформы Xamarin можно создать новое решение Xamarin.Forms с помощью Visual Studio или Visual Studio для Mac. Решение [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) использует переносимую библиотеку классов для общего кода.

> [!NOTE]
> Переносимые библиотеки классов заменены библиотеками .NET Standard. Все примеры кода в этой книге преобразованы для использования библиотек .NET Standard.

В этом примере демонстрируется решение Xamarin.Forms, созданное в Visual Studio, без дополнительных изменений. Это решение состоит из четырех проектов:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello) — переносимая библиотека классов (PCL), совместно используемая другими проектами;
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid) — проект приложения для Android;
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS) — проект приложения для iOS;
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP) — проект приложения для универсальной платформы Windows (Windows 10 и Windows 10 Mobile).

> [!NOTE]
> Xamarin.Forms теперь не поддерживает Windows 8.1, Windows Phone 8.1 и Windows 10 Mobile, при этом приложения Xamarin.Forms выполняются в Windows 10 для настольных компьютеров.

Вы можете назначить любой из этих проектов приложения запускаемым проектом, а затем скомпилировать и запустить программу на реальном устройстве или в симуляторе.

Во многих программах Xamarin.Forms вам не нужно будет вносить изменения в проекты приложений. Чаще всего это лишь маленькие заглушки, которые нужны только для запуска программы. Основное внимание уделяется библиотеке, которая используется совместно всеми приложениями.

## <a name="inside-the-files"></a>Внутреннее строение файлов

Визуальные элементы, отображаемые программой **Hello**, определены в конструкторе класса [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs). `App` является производным от класса [`Application`](xref:Xamarin.Forms.Application) Xamarin.Forms.

> [!NOTE]
> Шаблоны решений Visual Studio для Xamarin.Forms создают страницу на основе файла XAML. Структура XAML не рассматривается в этой книге вплоть до [главы 7](chapter07.md).

Раздел **References** в проекте PCL **Hello** включает следующие сборки Xamarin.Forms:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

Разделы **References** всех пяти проектов приложений включают дополнительные сборки, которые нужны для конкретных платформ:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE]
> Разделы **References** этих проектов теперь не содержат списка сборок. Вместо этого файл проекта содержит теги **PackageReference**, которые указывают нужный пакет NuGet для Xamarin.Forms. В разделе **Ссылки** в Visual Studio указан пакет **Xamarin.Forms** , а не сборки Xamarin.Forms.

Каждый проект приложения содержит вызов статического метода `Forms.Init` из пространства имен `Xamarin.Forms`. Он инициализирует библиотеку Xamarin.Forms. Для каждой платформы определена собственная версия `Forms.Init`. Вызовы этого метода присутствуют в следующих классах:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs);
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs);
- UWP: класс [`App`, метод `OnLaunched`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs).

Кроме того, каждая платформа создает экземпляр расположения класса `App` в общей библиотеке. Это выполняется в вызове `LoadApplication` в следующих классах:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs);
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs).

Во всем остальном эти проекты приложений являются обычными программами, которые сами не выполняют никаких действий.

## <a name="pcl-or-sap"></a>PCL или SAP?

Вы можете создать решение Xamarin.Forms, разместив общий код в переносимой библиотеке классов (PCL) или проекте общих ресурсов (SAP). Чтобы создать решение на основе SAP, выберите в Visual Studio вариант "Общий". Решение [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) демонстрирует шаблон SAP без дополнительных изменений.

> [!NOTE]
> Переносимые библиотеки классов здесь заменены библиотеками .NET Standard. Все примеры кода в этой книге преобразованы для использования библиотек .NET Standard. В остальном библиотеки PCL и .NET Standard очень похожи.

При использовании библиотек весь общий код размещается в проекте библиотеки, на который ссылаются проекты приложений для каждой платформы. При использовании подхода SAP общий код существует во всех проектах приложений платформы и является общим для них.

Большинство разработчиков Xamarin.Forms предпочитают подход с использованием библиотек. В этой книге большинство решений используют библиотеки. В проектах, в которых используется SAP, к имени добавлен суффикс **Sap**.

В рамках подхода с использованием SAP код в общем проекте может выполнять разные коды на разных платформах, используя директивы препроцессора C# (`#if`, #`elif` и `#endif`) со следующими идентификаторами:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

В общей библиотеке вы можете определить, на какой платформе выполняется приложение, как будет показано далее в этой главе.

## <a name="labels-for-text"></a>Метки для текста

Решение [**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) демонстрирует, как добавить новый файл кода C# в проект **Greetings**. Этот файл определяет класс с именем `GreetingsPage`, производный от `ContentPage`. В этой книге большинство проектов содержат один производный класс `ContentPage`, имя которого включает имя проекта и суффикс `Page`.

Конструктор `GreetingsPage` создает экземпляр представления [`Label`](xref:Xamarin.Forms.Label), которое создано в Xamarin.Forms для отображения текста. Свойство [`Text`](xref:Xamarin.Forms.Label.Text) содержит текст, отображаемый элементом `Label`. Эта программа присваивает значение `Label` свойству `Content` элемента `ContentPage`. Затем конструктор класса `App` создает экземпляр `GreetingsPage` и сохраняет его в свойстве `MainPage`.

Соответствующий текст появляется в левом верхнем углу страницы. В iOS этот текст перекрывает строку состояния страницы. Эту проблему можно решить несколькими способами.

### <a name="solution-1-include-padding-on-the-page"></a>Решение 1. Добавление отбивки к странице

Присвойте значение свойству [`Padding`](xref:Xamarin.Forms.Page.Padding) страницы. `Padding` имеет тип [`Thickness`](xref:Xamarin.Forms.Thickness). Это структура с четырьмя свойствами:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` определяет область внутри страницы, где отсутствует содержимое. Это позволяет `Label` не перекрывать строку состояния iOS.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Решение 2. Добавление отбивки только для iOS (применимо только для SAP)

Задайте свойство Padding только для iOS, используя директиву препроцессора C# в решении на основе SAP. Этот метод представлен в решении [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap).

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Решение 3. Добавление отбивки только для iOS (применимо для PCL или SAP)

В той версии Xamarin.Forms, которая используется в этой книге, в PCL или SAP можно использовать характерное для iOS свойство `Padding`, выбираемое с помощью статического метода [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) или [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*). Сейчас эти методы считаются нерекомендуемыми.

Методы `Device.OnPlatform` используются для выполнения кода, зависящего от платформы, или для выбора значений, зависящих от платформы. На внутреннем уровне они используют статическое свойство только для чтения [`Device.OS`](xref:Xamarin.Forms.Device.OS), которое возвращает элемент перечисления [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform):

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) для устройств UWP.

Методы `Device.OnPlatform`, свойство `Device.OS` и перечисление `TargetPlatform` сейчас считаются нерекомендуемыми. Используйте вместо них свойство [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) и сравнивайте возвращаемое значение `string` со следующими статическими полями:

- [`iOS`](xref:Xamarin.Forms.Device.iOS) — строковое значение iOS;
- [`Android`](xref:Xamarin.Forms.Device.Android) — строковое значение Android;
- [`UWP`](xref:Xamarin.Forms.Device.UWP) — строковое значение UWP, которое обозначает универсальную платформу Windows.

Также есть связанное свойство только для чтения [`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom). Оно возвращает элемент [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom), который содержит следующие элементы:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) не используется.

В iOS и Android срез между `Tablet` и `Phone` обозначает портретную ориентацию с шириной 600 единиц. В Windows `Desktop` обозначает приложение UWP под управлением Windows 10, а `Phone` — приложение UWP под управлением приложения Windows 10.

## <a name="solution-3a-set-margin-on-the-label"></a>Решение 3а. Добавление полей для метки

Свойство [`Margin`](xref:Xamarin.Forms.View.Margin) появилось после составления этой книги, но оно также имеет тип `Thickness`. Его можно задать для `Label`, чтобы определить область вокруг этого представления, которая учитывается при вычислении макета представления.

Свойство `Padding` доступно только для производных от [`Layout`](xref:Xamarin.Forms.Layout) и [`Page`](xref:Xamarin.Forms.Page). Свойство `Margin` доступно для всех производных от [`View`](xref:Xamarin.Forms.View).

## <a name="solution-4-center-the-label-within-the-page"></a>Решение 4. Центрирование метки по странице

Вы можете разместить `Label` по центру `Page` (или в одно из других восьми мест), задав для свойств [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) и [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) объекта `Label` значение с типом [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions). Структура `LayoutOptions` определяет два свойства.

- Свойство [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) с типом [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) является перечислением с четырьмя членами: [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start) для левого или верхнего края в зависимости от ориентации, [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End) для правого или нижнего края в зависимости от ориентации и [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Свойство [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) с типом `bool`.

Обычно эти свойства не используются напрямую. Каждое возможное сочетание этих двух свойств представлено восемью статическими свойствами только для чтения с типом `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` и `VerticalOptions` будут самыми важными свойствами в макете Xamarin.Forms. Они подробно описываются в [**главе 4. Прокрутка стека** ](chapter04.md).

Ниже приведен результат отображения, в котором свойства `HorizontalOptions` и `VerticalOptions` объекта `Label` имеют значение `LayoutOptions.Center`:

[![Снимок экрана с тремя изображениями программы приветствия](images/ch02fg05-small.png "Центрирование метки по горизонтали и вертикали")](images/ch02fg05-large.png#lightbox "Центрирование метки по горизонтали и вертикали")

## <a name="solution-5-center-the-text-within-the-label"></a>Решение 5. Центрирование текста по метке

Можно также центрировать текст (или поместить его в восемь других расположений на странице), присвоив свойствам [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) и [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) объекта `Label` значение одного из членов перечисления [`TextAlignment`](xref:Xamarin.Forms.TextAlignment):

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start) обозначает левый или верхний край (в зависимости от ориентации);
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End) обозначает правый или нижний край (в зависимости от ориентации).

Эти два свойства определяются только для `Label`, тогда как свойства `HorizontalAlignment` и `VerticalAlignment` определяются для `View` и наследуются всеми производными `View`. В визуальном представлении они очень похожи, но в следующей главе мы продемонстрируем их существенные отличия.

## <a name="related-links"></a>Связанные ссылки

- [Глава 2 — полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Глава 2 — примеры](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Глава 2 — примеры F#](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Начало работы с Xamarin.Forms](~/get-started/index.yml)
