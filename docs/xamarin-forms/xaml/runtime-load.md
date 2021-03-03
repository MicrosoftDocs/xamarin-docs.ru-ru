---
title: Загрузка XAML во время выполнения в Xamarin.Forms
description: XAML можно загрузить и проанализировать во время выполнения с помощью методов расширения Лоадфромксамл.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c027ef35462e6d2d43acf4ea5241a38abe15d41f
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374281"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Загрузка XAML во время выполнения в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

[`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml)Пространство имен содержит два [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) метода расширения, которые можно использовать для загрузки и синтаксического анализа XAML во время выполнения.

## <a name="background"></a>Историческая справка

При Xamarin.Forms создании класса XAML [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) метод вызывается неявно. Это происходит потому, что файл кода программной части для класса XAML вызывает `InitializeComponent` метод из своего конструктора:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Когда Visual Studio создает проект, содержащий файл XAML, он анализирует XAML-файл для создания файла кода C# (например, **MainPage.XAML.g.CS**), содержащего определение `InitializeComponent` метода:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent`Метод вызывает метод, [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) чтобы извлечь XAML-файл (или его скомпилированный двоичный файл) из библиотеки .NET Standard. После извлечения он инициализирует все объекты, определенные в файле XAML, соединяет их вместе в отношениях типа «родители-потомки», присоединяет обработчики событий, определенные в коде к событиям, заданным в файле XAML, и устанавливает результирующее дерево объектов в качестве содержимого страницы.

## <a name="loading-xaml-at-runtime"></a>Загрузка XAML во время выполнения

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)Методы являются `public` , и поэтому могут вызываться из Xamarin.Forms приложений для загрузки и анализировать XAML во время выполнения. Это позволяет выполнять такие сценарии, как приложение, которое загружает XAML из веб-службы, создает требуемое представление из XAML и отображает его в приложении.

> [!WARNING]
> Загрузка XAML во время выполнения имеет значительные затраты на производительность, и обычно ее следует избегать.

В следующем примере кода показано простое использование:

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

В этом примере [`Button`](xref:Xamarin.Forms.Button) создается экземпляр, а его [`Text`](xref:Xamarin.Forms.Button.Text) значение свойства задается из XAML, определенного в `string` . `Button`Затем добавляется в объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) , который был ОПРЕДЕЛЕН в XAML для страницы.

> [!NOTE]
> [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)Методы расширения позволяют указать аргумент универсального типа. Однако нередко требуется указывать аргумент типа, так как он будет выведен из типа экземпляра, на котором он работает.

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)Метод можно использовать для расширения любого XAML-кода, в следующем примере — путем расширения [`ContentPage`](xref:Xamarin.Forms.ContentPage) и перехода к нему.

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>Доступ к элементам

Загрузка XAML во время выполнения с помощью [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) метода не допускайте строго типизированного доступа к ЭЛЕМЕНТАМ XAML, которые имеют указанные имена объектов среды выполнения (с использованием `x:Name` ). Однако эти элементы XAML можно получить с помощью [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) метода, а затем получить к ним доступ по мере необходимости:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

В этом примере XAML для для [`ContentPage`](xref:Xamarin.Forms.ContentPage) является неизменным. Этот код XAML включает [`Label`](xref:Xamarin.Forms.Label) именованный объект `monkeyName` , который извлекается с помощью [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) метода до [`Text`](xref:Xamarin.Forms.Label.Text) задания его свойства.

## <a name="related-links"></a>Связанные ссылки

- [Лоадрунтимексамл (пример)](/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)