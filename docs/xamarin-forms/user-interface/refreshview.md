---
title: Xamarin.Forms RefreshView
description: Xamarin.Forms RefreshView — Это контейнерный элемент управления, предоставляющий функции обновления для прокручиваемого содержимого.
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- RefreshView
- Universal Windows Platform
ms.openlocfilehash: 83802683aee722468acf9bcc827ba66f45c05e6b
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225485"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.Forms RefreshView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

`RefreshView`— Это контейнерный элемент управления, предоставляющий функции обновления для прокручиваемого содержимого. Таким образом, дочерний элемент `RefreshView` должен быть прокручиваемым элементом управления, таким как [`ScrollView`](xref:Xamarin.Forms.ScrollView) , [`CollectionView`](xref:Xamarin.Forms.CollectionView) или [`ListView`](xref:Xamarin.Forms.ListView) .

`RefreshView` определяет следующие свойства:

- `Command`Тип `ICommand` , который выполняется при активации обновления.
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `IsRefreshing`Тип `bool` , который указывает текущее состояние `RefreshView` .
- `RefreshColor`Тип `Color` — Цвет круга хода выполнения, отображаемый во время обновления.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

> [!NOTE]
> В Universal Windows Platform можно задать направление извлечения для `RefreshView` конкретной платформы. Дополнительные сведения см. в разделе [ RefreshView направление опроса](~/xamarin-forms/platform/windows/refreshview-pulldirection.md).

## <a name="create-a-refreshview"></a>Создание класса RefreshView

В следующем примере показано, как создать экземпляр `RefreshView` в XAML:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

`RefreshView`Также можно создать в коде:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

В этом примере `RefreshView` функция предоставляет функцию Pull для обновления до, [`ScrollView`](xref:Xamarin.Forms.ScrollView) дочерним объектом которого является [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) . `FlexLayout`Компонент использует связываемый макет для создания его содержимого путем привязки к коллекции элементов и задает внешний вид каждого элемента с помощью [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Дополнительные сведения о макетах с возможностью привязки см. [в разделе макеты с возможностью привязки в Xamarin.Forms ](~/xamarin-forms/user-interface/layouts/bindable-layouts.md).

Значение `RefreshView.IsRefreshing` свойства указывает текущее состояние `RefreshView` . При активации обновления пользователем это свойство автоматически переходит в `true` . После завершения обновления следует сбросить свойство до `false` .

Когда пользователь инициирует обновление, `ICommand` `Command` выполняется свойство, определенное свойством, которое должно обновлять отображаемые элементы. Во время обновления отображается визуализация обновления, которая состоит из анимированного круга хода выполнения:

[![Снимок экрана RefreshView обновления данных в iOS и Android](refreshview-images/default-progress-circle.png "[! Операцион. NO-LOC (Рефрешвиев)] обновление данных")](refreshview-images/default-progress-circle-large.png#lightbox "[! Операцион. NO-LOC (Рефрешвиев)] обновление данных")

> [!NOTE]
> Ручное задание `IsRefreshing` для свойства значения `true` приведет к запуску визуализации обновления и будет выполнять объект, `ICommand` определенный `Command` свойством.

## <a name="refreshview-appearance"></a>RefreshViewвнешний вид

В дополнение к свойствам, `RefreshView` унаследованным от [`VisualElement`](xref:Xamarin.Forms.VisualElement) класса, `RefreshView` также определяет `RefreshColor` свойство. Это свойство можно задать для определения цвета круга хода выполнения, отображаемого во время обновления:

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

На следующем снимке экрана показан объект `RefreshView` с `RefreshColor` набором свойств.

[![Снимок экрана RefreshView с синей окружностью хода выполнения на iOS и Android](refreshview-images/teal-progress-circle.png "[! Операцион. NO-LOC (Рефрешвиев)] с синей окружностью хода выполнения")](refreshview-images/teal-progress-circle-large.png#lightbox "[! Операцион. NO-LOC (Рефрешвиев)] с синей окружностью хода выполнения")

Кроме того, `BackgroundColor` свойству может быть присвоено значение [`Color`](xref:Xamarin.Forms.Color) , представляющее цвет фона круга хода выполнения.

> [!NOTE]
> В iOS `BackgroundColor` свойство задает цвет фона `UIView` , который содержит круг хода выполнения.

## <a name="disable-a-refreshview"></a>ОтключениеRefreshView

Приложение может ввести состояние, при котором операция Pull для обновления не является допустимой операцией. В таких случаях `RefreshView` можно отключить, задав `IsEnabled` свойству значение `false` . Это предотвратит возможность запуска запроса Pull для обновления пользователями.

Кроме того, при определении `Command` свойства `CanExecute` делегат `ICommand` может быть указан для включения или отключения команды.

## <a name="related-links"></a>Связанные ссылки

- [RefreshViewСледующий](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [Связываемые макеты вXamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshViewНаправление опроса — зависит от платформы](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
