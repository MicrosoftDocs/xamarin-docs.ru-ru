---
title: Настройка элемента управления Entry
description: Элемент управления Xamarin.Forms Entry допускает редактирование одной строки текста. В этой статье показано, как создать пользовательский отрисовщик для элемента управления Entry, чтобы переопределить собственный способ отрисовки по умолчанию с помощью настройки для конкретной платформы.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c04626a3139a7fa44b2b13b2c1dd343188322f3c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93368509"
---
# <a name="customizing-an-entry"></a>Настройка элемента управления Entry

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/customrenderers-entry)

_Элемент управления Xamarin.Forms Entry допускает редактирование одной строки текста. В этой статье показано, как создать пользовательский отрисовщик для элемента управления Entry, чтобы переопределить собственный способ отрисовки по умолчанию с помощью настройки для конкретной платформы._

Каждый элемент управления Xamarin.Forms имеет сопутствующий отрисовщик для каждой платформы, который создает экземпляр собственного элемента управления. Когда элемент управления [`Entry`](xref:Xamarin.Forms.Entry) отображается в приложении Xamarin.Forms, в iOS создается экземпляр класса `EntryRenderer`, который, в свою очередь, создает собственный элемент управления `UITextField`. На платформе Android класс `EntryRenderer` создает элемент управления `EditText`. На универсальной платформе Windows (UWP) класс `EntryRenderer` создает элемент управления `TextBox`. Дополнительные сведения об отрисовщике и классах собственных элементов управления, с которыми сопоставляются элементы управления Xamarin.Forms, см. в статье [Базовые классы и собственные элементы управления отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между элементом управления [`Entry`](xref:Xamarin.Forms.Entry) и соответствующими собственными элементами управления, которые его реализуют:

![Связь между элементом управления Entry и реализующими нативными элементами управления](entry-images/entry-classes.png)

Процесс отрисовки можно использовать для реализации настроек для конкретных платформ путем создания пользовательского отрисовщика для элемента управления [`Entry`](xref:Xamarin.Forms.Entry) на каждой платформе. Этот процесс выглядит следующим образом:

1. [Создание](#creating-the-custom-entry-control) пользовательского элемента управления Xamarin.Forms.
1. [Использование](#consuming-the-custom-control) пользовательского элемента управления в Xamarin.Forms.
1. [Создание](#creating-the-custom-renderer-on-each-platform) пользовательского отрисовщика для элемента управления на каждой платформе.

Мы рассмотрим каждый пункт по порядку, чтобы реализовать элемент управления [`Entry`](xref:Xamarin.Forms.Entry) с разным цветом фона на каждой платформе.

> [!IMPORTANT]
> В этой статье описывается, как создать простой пользовательский отрисовщик. Однако чтобы реализовать элемент управления `Entry` с разным цветом фона на каждой платформе, создавать пользовательский отрисовщик необязательно. Проще будет воспользоваться классом [`Device`](xref:Xamarin.Forms.Device) или расширением разметки `OnPlatform`, указав значения для конкретных платформ. Дополнительные сведения см. в разделах [Указание значений для конкретных платформ](~/xamarin-forms/platform/device.md#provide-platform-specific-values) и [Расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="creating-the-custom-entry-control"></a>Создание пользовательского элемента управления Entry

Пользовательский элемент управления [`Entry`](xref:Xamarin.Forms.Entry) можно создать путем создания подкласса элемента управления `Entry`, как показано в следующем примере кода.

```csharp
public class MyEntry : Entry
{
}
```

Элемент управления `MyEntry` создается в проекте библиотеки .NET Standard и представляет собой попросту элемент управления [`Entry`](xref:Xamarin.Forms.Entry). Настройка элемента управления будет осуществляться в пользовательском отрисовщике, поэтому дополнительная реализация в элементе управления `MyEntry` не требуется.

## <a name="consuming-the-custom-control"></a>Использование пользовательского элемента управления

На элемент управления `MyEntry` можно ссылаться в XAML в проекте библиотеки .NET Standard, объявив пространство имен для его расположения и используя префикс пространства имен в элементе управления. В следующем примере кода показано, как элемент управления `MyEntry` может использоваться страницей XAML.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

Префикс пространства имен `local` может иметь любое имя. Тем не менее значения `clr-namespace` и `assembly` должны соответствовать данным пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показано, как элемент управления `MyEntry` может использоваться страницей C#.

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Этот код создает объект [`ContentPage`](xref:Xamarin.Forms.ContentPage), который отображает элементы управления [`Label`](xref:Xamarin.Forms.Label) и `MyEntry`, размещаемые в центре страницы как по вертикали, так и по горизонтали.

Теперь в каждый проект приложения можно добавить пользовательский отрисовщик для настройки внешнего вида элемента управления на каждой платформе.

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского отрисовщика на каждой платформе

Процесс создания класса пользовательского отрисовщика выглядит следующим образом:

1. Создайте подкласс класса `EntryRenderer`, который отрисовывает собственный элемент управления.
1. Переопределите метод `OnElementChanged`, который отрисовывает собственный элемент управления, и напишите логику для его настройки. Этот метод вызывается при создании соответствующего элемента управления Xamarin.Forms.
1. Добавьте атрибут `ExportRenderer` в класс настраиваемого отрисовщика, чтобы указать, что он будет использоваться для отрисовки элемента управления Xamarin.Forms. Этот атрибут используется для регистрации пользовательского отрисовщика в Xamarin.Forms.

> [!NOTE]
> Предоставлять пользовательский отрисовщик в проекте для каждой платформы не обязательно. Если пользовательский отрисовщик не зарегистрирован, будет использоваться отрисовщик по умолчанию для базового класса элемента управления.

На следующей схеме показаны обязанности каждого проекта в примере приложения, а также связи между ними:

![Задачи проекта пользовательского отрисовщика MyEntry](entry-images/solution-structure.png)

Элемент управления `MyEntry` отрисовывается с помощью зависящих от платформы классов `MyEntryRenderer`, которые являются производными от класса `EntryRenderer` для каждой платформы. Это приводит к тому, что каждый элемент управления `MyEntry` отрисовывается с использованием цвета фона для конкретной платформы, как показано на следующих снимках экрана.

![Элемент управления MyEntry на каждой платформе](entry-images/screenshots.png)

Класс `EntryRenderer` предоставляет метод `OnElementChanged`, который вызывается при создании элемента управления Xamarin.Forms для отрисовки соответствующего собственного элемента управления. Этот метод принимает параметр `ElementChangedEventArgs`, содержащий свойства `OldElement` и `NewElement`. Эти свойства представляют элемент Xamarin.Forms, к которому *был* присоединен отрисовщик, и элемент Xamarin.Forms, к которому *присоединен* отрисовщик, соответственно. В примере приложения свойство `OldElement` будет иметь значение `null`, а свойство `NewElement` будет содержать ссылку на элемент управления `MyEntry`.

Настройка собственного элемента управления выполняется в переопределенной версии метода `OnElementChanged` в классе `MyEntryRenderer`. Типизированную ссылку на собственный элемент управления, используемый на платформе, можно получить через свойство `Control`. Кроме того, ссылку на отрисовываемый элемент управления Xamarin.Forms можно получить с помощью свойства `Element`, хотя оно не используется в примере приложения.

Каждый класс пользовательского отрисовщика дополняется атрибутом `ExportRenderer`, который регистрирует отрисовщик в Xamarin.Forms. Атрибут принимает два параметра — имя типа отрисовываемого элемента управления Xamarin.Forms и имя типа пользовательского отрисовщика. Префикс `assembly` в атрибуте указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация класса пользовательского отрисовщика `MyEntryRenderer` для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского отрисовщика в iOS

В следующем примере кода показан пользовательский отрисовщик для платформы iOS:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления iOS `UITextField` со ссылкой на элемент управления, присваиваемый свойству `Control` отрисовщика. Затем с помощью метода `UIColor.FromRGB` задается светло-сиреневый цвет фона.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского отрисовщика в Android

В следующем примере кода показан пользовательский отрисовщик для платформы Android:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления Android `EditText` со ссылкой на элемент управления, присваиваемый свойству `Control` отрисовщика. Затем с помощью метода `Control.SetBackgroundColor` задается светло-зеленый цвет фона.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского отрисовщика на универсальной платформе Windows

В следующем примере кода показан пользовательский отрисовщик для платформы UWP:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

В результате вызова метода `OnElementChanged` базового класса создается элемент управления `TextBox` со ссылкой на элемент управления, присваиваемый свойству `Control` отрисовщика. Затем путем создания экземпляра `SolidColorBrush` задается голубой цвет фона.

## <a name="summary"></a>Сводка

В этой статье было показано, как создать пользовательский отрисовщик для элемента управления Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry), чтобы переопределить собственный способ отрисовки по умолчанию с помощью настройки для конкретной платформы. Пользовательские отрисовщики обеспечивают удобный подход к настройке внешнего вида элементов управления Xamarin.Forms. Их можно использовать для незначительных изменений стиля или сложной настройки макетов и поведения в зависимости от платформы.

## <a name="related-links"></a>Связанные ссылки

- [CustomRendererEntry (пример)](/samples/xamarin/xamarin-forms-samples/customrenderers-entry)