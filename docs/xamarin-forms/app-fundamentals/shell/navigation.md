---
title: Навигация по оболочке Xamarin.Forms
description: Приложения оболочки Xamarin.Forms предоставляют улучшенные возможности навигации по интерфейсу, позволяя переходить на любую страницу в приложении без необходимости навигации по заданной иерархии.
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/23/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d714b38dd838ef0779802dc7ca8c050dbd4464b
ms.sourcegitcommit: 1b542afc0f6f2f6adbced527ae47b9ac90eaa1de
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/03/2021
ms.locfileid: "101757539"
---
# <a name="xamarinforms-shell-navigation"></a>Навигация по оболочке Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Оболочка Xamarin.Forms предоставляет улучшенные возможности навигации по интерфейсу на основе URI, позволяя переходить на любую страницу в приложении без соблюдения строгой иерархии. Кроме того, они также дают возможность перехода назад без необходимости прохода всех страниц в стеке навигации.

Класс [`Shell`](xref:Xamarin.Forms.Shell) определяет следующие свойства, связанные с навигацией:

- Присоединенное свойство [`BackButtonBehavior`](xref:Xamarin.Forms.Shell.BackButtonBehaviorProperty) с типом [`BackButtonBehavior`](xref:Xamarin.Forms.BackButtonBehavior) определяет поведение кнопки "Назад".
- [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) с типом [`ShellItem`](xref:Xamarin.Forms.ShellItem) обозначает выбранный элемент.
- [`CurrentPage`](xref:Xamarin.Forms.Shell.CurrentPage) с типом [`Page`](xref:Xamarin.Forms.Page) обозначает представленную сейчас страницу.
- [`CurrentState`](xref:Xamarin.Forms.Shell.CurrentState) с типом [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) обозначает текущее состояние навигации для [`Shell`](xref:Xamarin.Forms.Shell).
- [`Current`](xref:Xamarin.Forms.Shell.Current) с типом [`Shell`](xref:Xamarin.Forms.Shell) является приведенным по типу псевдонимом для `Application.Current.MainPage`.

Свойства [`BackButtonBehavior`](xref:Xamarin.Forms.Shell.BackButtonBehaviorProperty), [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) и [`CurrentState`](xref:Xamarin.Forms.Shell.CurrentState) поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки данных.

Навигация выполняется путем вызова метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*) из класса [`Shell`](xref:Xamarin.Forms.Shell). Когда планируется действие навигации, срабатывает событие [`Navigating`](xref:Xamarin.Forms.Shell.Navigating), а после завершения навигации — событие [`Navigated`](xref:Xamarin.Forms.Shell.Navigated).

> [!NOTE]
> Навигацию по страницам в приложении оболочки можно по-прежнему выполнять с помощью свойства [Navigation](xref:Xamarin.Forms.NavigableElement.Navigation). Дополнительные сведения см. в статье [об иерархической навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="routes"></a>Маршруты

Навигация в приложении оболочки выполняется путем указания URI для перехода. URI навигации могут иметь три компонента.

- *Маршрут* определяет путь к содержимому, которое существует в визуальной иерархии оболочки.
- *Страница*. Страницы не отражены в визуальной иерархии оболочки и могут добавляться в стек навигации из любого места в приложении оболочки. Например, страница сведений не будет определяться в визуальной иерархии оболочки, но может быть добавлена в стек навигации при необходимости.
- Один или несколько *параметров запроса*. Параметры запроса могут передаваться странице назначения при переходе на нее.

Если URI навигации содержит всех три компонента, он имеет такую структуру: //маршрут/страница?параметры_запроса

### <a name="register-routes"></a>Регистрация маршрутов

Маршруты можно определить для объектов [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem), [`TabBar`](xref:Xamarin.Forms.TabBar), [`Tab`](xref:Xamarin.Forms.Tab) и [`ShellContent`](xref:Xamarin.Forms.ShellContent) с помощью свойств `Route`:

```xaml
<Shell ...>
    <FlyoutItem ...
                Route="animals">
        <Tab ...
             Route="domestic">
            <ShellContent ...
                          Route="cats" />
            <ShellContent ...
                          Route="dogs" />
        </Tab>
        <ShellContent ...
                      Route="monkeys" />
        <ShellContent ...
                      Route="elephants" />  
        <ShellContent ...
                      Route="bears" />
    </FlyoutItem>
    <ShellContent ...
                  Route="about" />                  
    ...
</Shell>
```

> [!NOTE]
> С каждым элементом в иерархии оболочки сопоставляется маршрут. Если маршрут не задан, он создается во время выполнения. Но для автоматических маршрутов не гарантируется согласованность между сеансами приложения.

Этот пример создает описанную ниже иерархию маршрутов, которую можно использовать для программной навигации:

```
animals
  domestic
    cats
    dogs
  monkeys
  elephants
  bears
about
```

Чтобы перейти к объекту [`ShellContent`](xref:Xamarin.Forms.ShellContent) по маршруту `dogs`, абсолютный URI должен выглядеть так: `//animals/domestic/dogs`. Аналогичным образом для перехода к объекта `ShellContent` по маршруту `about` абсолютный URI должен выглядеть так: `//about`.

> [!WARNING]
> Если будет обнаружен дубликат маршрута, при запуске приложения создается исключение `ArgumentException`. Это исключение также будет создаваться, если два или более маршрута на одном уровне иерархии совместно используют имя маршрута.

### <a name="register-detail-page-routes"></a>Регистрация маршрутов страниц сведений

В конструкторе подкласса [`Shell`](xref:Xamarin.Forms.Shell) или в любой другой части кода, которая выполняется перед вызовом маршрута, можно явно зарегистрировать дополнительные маршруты для любых страниц сведений, которые не представлены в визуальной иерархии оболочки. Это выполняется с помощью метода [`Routing.RegisterRoute`](xref:Xamarin.Forms.Routing.RegisterRoute*):

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

Этот пример регистрирует страницы сведений, для которых не определены маршруты в подклассе [`Shell`](xref:Xamarin.Forms.Shell). К страницам сведений можно переходить по URI из любой точки в приложении. Маршруты для таких страниц называются *глобальными маршрутами*.

> [!WARNING]
> Если метод [`Routing.RegisterRoute`](xref:Xamarin.Forms.Routing.RegisterRoute*) пытается зарегистрировать один и тот же маршрут к двум или более разным типам, будет создано исключение `ArgumentException`.

Также вы можете регистрировать страницы в другие иерархии маршрутов, если потребуется:

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

Этот пример настраивает контекстную навигацию по страницам, где переход по маршруту `details` со страницы для маршрута `monkeys` отображает `MonkeyDetailPage`. Аналогичным образом переход по маршруту `details` со страницы для маршрута `elephants` отображает `ElephantDetailPage`. Дополнительные сведения см. в разделе [Контекстная навигация](#contextual-navigation).

> [!NOTE]
> Если для страницы зарегистрированы маршруты с помощью метода [`Routing.RegisterRoute`](xref:Xamarin.Forms.Routing.RegisterRoute*), вы можете отменить такую регистрацию с помощью метода [`Routing.UnRegisterRoute`](xref:Xamarin.Forms.Routing.UnRegisterRoute*), если потребуется.

## <a name="perform-navigation"></a>Выполнение перемещения

Чтобы выполнить перемещение, нужно получить ссылку на подкласс [`Shell`](xref:Xamarin.Forms.Shell). Чтобы получить эту ссылку, можно привести свойство `App.Current.MainPage` к объекту `Shell` или получить значение свойства [`Shell.Current`](xref:Xamarin.Forms.Shell.Current). После этого выполняется перемещение путем вызова метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*) из объекта `Shell`. Этот метод переходит к [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) и возвращает `Task`, который завершается после завершения анимации для этого перехода. Объект `ShellNavigationState` создается с помощью метода `GoToAsync` из `string` или `Uri` и для его свойства `Location` задается значение аргумента `string` или `Uri`.

> [!IMPORTANT]
> При переходе по маршруту, входящему в визуальную иерархию оболочки, стек навигации не создается. Но при переходе к странице, которая не входит в визуальную иерархию оболочки, создается стек навигации.

Текущее состояние навигации для объекта [`Shell`](xref:Xamarin.Forms.Shell) можно извлечь через свойство [`Shell.Current.CurrentState`](xref:Xamarin.Forms.Shell.CurrentState), которое содержит URI отображаемого маршрута в свойстве `Location`.

### <a name="absolute-routes"></a>Абсолютные маршруты

Для навигации можно передать допустимый абсолютный URI в качестве аргумента в метод [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*):

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

Этот пример выполняет переход на страницу с маршрутом `monkeys`, который определен в объекте [`ShellContent`](xref:Xamarin.Forms.ShellContent). Объект `ShellContent`, который представляет маршрут `monkeys`, является дочерним элементом объекта [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) с маршрутом `animals`.

### <a name="relative-routes"></a>Относительные маршруты

Для навигации можно также передать допустимый относительный URI в качестве аргумента в метод [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*). Система маршрутизации попытается сопоставить URI с объектом [`ShellContent`](xref:Xamarin.Forms.ShellContent). Таким образом, если все маршруты в приложении уникальны, для навигации достаточно указать уникальное имя маршрута в качестве относительного URI.

Поддерживаются следующие форматы относительных маршрутов.

| Формат | Описание |
| --- | --- |
| *route* | Поиск указанного маршрута выполняется в иерархии маршрутов вверх, начиная от текущей позиции. Страница совпадения будет отправлена в стек навигации. |
| /*маршрут* | Поиск указанного маршрута выполняется в иерархии маршрутов вниз, начиная от текущей позиции. Страница совпадения будет отправлена в стек навигации. |
| //*маршрут* | Поиск указанного маршрута выполняется в иерархии маршрутов вверх, начиная от текущей позиции. Страница совпадения заменит стек навигации. |
| ///*маршрут* | Поиск указанного маршрута выполняется в иерархии маршрутов вниз, начиная от текущей позиции. Страница совпадения заменит стек навигации. |

В приведенном ниже примере выполняется переход на страницу с маршрутом `monkeydetails`.

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

В этом примере поиск маршрута в иерархии `monkeyDetails` выполняется вверх до тех пор, пока не будет найдена станица совпадения. Найденная страница отправляется в стек навигации.

#### <a name="contextual-navigation"></a>Контекстная навигация

Относительные маршруты позволяют организовать контекстную навигацию. Для примера рассмотрим следующую иерархию маршрутов:

```
monkeys
  details
bears
  details
```

Когда отображается страница с зарегистрированным маршрутом `monkeys`, переход по маршруту `details` отображает страницу с зарегистрированным маршрутом `monkeys/details`. Аналогичным образом, когда отображается страница с зарегистрированным маршрутом `bears`, переход по маршруту `details` отображает страницу с зарегистрированным маршрутом `bears/details`. Сведения о том, как зарегистрировать маршруты для этого примера, см. в [этой статье](#register-detail-page-routes).

### <a name="backwards-navigation"></a>Обратная навигация

Для выполнения обратной навигации можно указать ".." в качестве аргумента для метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*):

```csharp
await Shell.Current.GoToAsync("..");
```

Обратную навигацию с ".." можно также объединить с маршрутом:

```csharp
await Shell.Current.GoToAsync("../route");
```

В этом примере выполняется обратная навигация, а затем осуществляется переход к указанному маршруту.

> [!IMPORTANT]
> Переход назад и по указанному маршруту возможен, только если при обратной навигации вы будете находиться в текущем местоположении в иерархии маршрутов для перехода по указанному маршруту.

Аналогичным образом можно перемещаться назад несколько раз, а затем переходить по указанному маршруту:

```csharp
await Shell.Current.GoToAsync("../../route");
```

В этом примере обратная навигация выполняется дважды, а затем осуществляется переход к указанному маршруту.

Кроме того, при обратной навигации можно передавать данные в свойствах запроса:

```csharp
await Shell.Current.GoToAsync($"..?parameterToPassBack={parameterValueToPassBack}");
```

В этом примере выполняется обратная навигация, и значение параметра запроса передается параметру запроса на предыдущей странице.

> [!NOTE]
> Параметры запроса можно добавить к любому запросу обратной навигации.

Дополнительные сведения о передаче данных при навигации см. в разделе [Передача данных](#pass-data).

### <a name="invalid-routes"></a>Недопустимые маршруты

Следующие форматы маршрутов считаются недопустимыми.

| Формат | Объяснение |
| --- | --- |
| //*страница* или / / /*страница* | В настоящее время глобальные маршруты не могут быть единственной страницей в стеке навигации. Таким образом, абсолютная маршрутизация для глобальных маршрутов не поддерживается. |

Использование этих форматов маршрутов приводит к созданию исключения `Exception`.

> [!WARNING]
> Попытка перехода на несуществующий маршрут приводит к созданию исключения `ArgumentException`.

### <a name="debugging-navigation"></a>Отладка навигации

Некоторые классы оболочки дополняются `DebuggerDisplayAttribute`, который определяет отображение класса или поля в отладчике. Это помогает в отладке запросов навигации, отображая актуальные данные для запроса перехода. Например, на следующем снимке экрана показаны свойства [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) и [`CurrentState`](xref:Xamarin.Forms.Shell.CurrentState) объекта [`Shell.Current`](xref:Xamarin.Forms.Shell.Current):

![Снимок экрана отладчика](navigation-images/debugger.png "Отладчик")

В этом примере свойство [`CurrentItem`](xref:Xamarin.Forms.Shell.CurrentItem) с типом [`FlyoutItem`](xref:Xamarin.Forms.FlyoutItem) отображает название и маршрут объекта `FlyoutItem`. Аналогичным образом свойство [`CurrentState`](xref:Xamarin.Forms.Shell.CurrentState) с типом [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) отображает URI отображаемого маршрута в приложении оболочки.

### <a name="navigation-stack"></a>Стек навигации

Класс [`Tab`](xref:Xamarin.Forms.Tab) определяет свойство [`Stack`](xref:Xamarin.Forms.ShellSection.Stack) с типом `IReadOnlyList<Page>`, которое представляет текущий стек навигации в пределах `Tab`. Этот класс также предоставляет следующие переопределяемые методы навигации:

- [`GetNavigationStack`](xref:Xamarin.Forms.ShellSection.GetNavigationStack*), который возвращает текущий стек навигации `IReadOnlyList<Page`>.
- [`OnInsertPageBefore`](xref:Xamarin.Forms.ShellSection.OnInsertPageBefore*), который вызывается при вызове метода `INavigation.InsertPageBefore`.
- [`OnPopAsync`](xref:Xamarin.Forms.ShellSection.OnPopAsync*), который возвращает `Task<Page>` и вызывается при вызове `INavigation.PopAsync`.
- [`OnPopToRootAsync`](xref:Xamarin.Forms.ShellSection.OnPopToRootAsync*), который возвращает `Task` и вызывается при вызове `INavigation.OnPopToRootAsync`.
- [`OnPushAsync`](xref:Xamarin.Forms.ShellSection.OnPushAsync*), который возвращает `Task` и вызывается при вызове `INavigation.PushAsync`.
- [`OnRemovePage`](xref:Xamarin.Forms.ShellSection.OnRemovePage*), который вызывается при вызове метода `INavigation.RemovePage`.

В следующем примере показано переопределение метода [`OnRemovePage`](xref:Xamarin.Forms.ShellSection.OnRemovePage*):

```csharp
public class MyTab : Tab
{
    protected override void OnRemovePage(Page page)
    {
        base.OnRemovePage(page);

        // Custom logic
    }
}
```

В этом примере объекты `MyTab` должны использоваться в визуальной иерархии оболочки, а не в объектах [`Tab`](xref:Xamarin.Forms.Tab).

## <a name="navigation-events"></a>События навигации

Класс [`Shell`](xref:Xamarin.Forms.Shell) определяет событие [`Navigating`](xref:Xamarin.Forms.Shell.Navigating), которое возникает перед выполнением любого перехода — программного или вызванного действием пользователя. Объект [`ShellNavigatingEventArgs`](xref:Xamarin.Forms.ShellNavigatingEventArgs), который прилагается к событию `Navigating`, содержит следующие свойства:

| Свойство | Тип | Описание |
|---|---|---|
| [`Current`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Current) | [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) | URI текущей страницы. |
| [`Source`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Source) | [`ShellNavigationSource`](xref:Xamarin.Forms.ShellNavigationSource) | Тип выполненного перехода. |
| [`Target`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Target) | [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) | URI, представляющий целевой объект навигации. |
| [`CanCancel`](xref:Xamarin.Forms.ShellNavigatingEventArgs.CanCancel)  | `bool` | Значение, указывающее, возможна ли отмена перехода. |
| [`Cancelled`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Cancelled)  | `bool` | Значение, указывающее, была ли навигация отменена. |

Кроме того, класс [`ShellNavigatingEventArgs`](xref:Xamarin.Forms.ShellNavigatingEventArgs) предоставляет метод [`Cancel`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Cancel*), позволяющий отменить переход, и метод [`GetDeferral`](xref:Xamarin.Forms.ShellNavigatingEventArgs.GetDeferral*), возвращающий токен [`ShellNavigatingDeferral`](xref:Xamarin.Forms.ShellNavigatingDeferral), который можно использовать для завершения перехода. Дополнительные сведения: [Отложенный переход](#navigation-deferral).

Класс [`Shell`](xref:Xamarin.Forms.Shell) также определяет событие [`Navigated`](xref:Xamarin.Forms.Shell.Navigated), которое возникает при завершении навигации. Объект [`ShellNavigatedEventArgs`](xref:Xamarin.Forms.ShellNavigatedEventArgs), который прилагается к событию `Navigated`, содержит следующие свойства:

| Свойство | Тип | Описание |
|---|---|---|
| [`Current`](xref:Xamarin.Forms.ShellNavigatedEventArgs.Current)  | [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) | URI текущей страницы. |
| [`Previous`](xref:Xamarin.Forms.ShellNavigatedEventArgs.Previous) | [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) | URI предыдущей страницы. |
| [`Source`](xref:Xamarin.Forms.ShellNavigatedEventArgs.Source) | [`ShellNavigationState`](xref:Xamarin.Forms.ShellNavigationState) | Тип выполненного перехода. |

> [!IMPORTANT]
> Метод `OnNavigating` вызывается, когда генерируется событие [`Navigating`](xref:Xamarin.Forms.Shell.Navigating). Аналогично, метод `OnNavigated` вызывается, когда генерируется событие [`Navigated`](xref:Xamarin.Forms.Shell.Navigated). Оба метода можно переопределить в подклассе [`Shell`](xref:Xamarin.Forms.Shell) для перехвата запросов навигации.

Классы [`ShellNavigatedEventArgs`](xref:Xamarin.Forms.ShellNavigatedEventArgs) и [`ShellNavigatingEventArgs`](xref:Xamarin.Forms.ShellNavigatingEventArgs) имеют свойства `Source` с типом [`ShellNavigationSource`](xref:Xamarin.Forms.ShellNavigationSource). Значения перечисления указаны ниже:

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

Таким образом, переходы могут перехватываться в переопределении `OnNavigating`, а действия могут выполняться на основе источника навигации. Например, следующий код позволяет отменить переход назад, если на странице есть несохраненные данные:

```csharp
protected override void OnNavigating(ShellNavigatingEventArgs args)
{
    base.OnNavigating(args);

    // Cancel any back navigation.
    if (args.Source == ShellNavigationSource.Pop)
    {
        args.Cancel();
    }
// }
```

## <a name="navigation-deferral"></a>Отложенный переход

Переход в рамках оболочки можно перехватить, а затем завершить или отменить в зависимости от пользовательского выбора. Для этого можно переопределить метод `OnNavigating` в подклассе [`Shell`](xref:Xamarin.Forms.Shell) и вызвать метод [`GetDeferral`](xref:Xamarin.Forms.ShellNavigatingEventArgs.GetDeferral*) для объекта [`ShellNavigatingEventArgs`](xref:Xamarin.Forms.ShellNavigatingEventArgs). Этот метод возвращает токен [`ShellNavigatingDeferral`](xref:Xamarin.Forms.ShellNavigatingDeferral) с методом [`Complete`](xref:Xamarin.Forms.ShellNavigatingDeferral.Complete*), который можно использовать для завершения запроса о переходе:

```csharp
public MyShell : Shell
{
    // ...
    protected override async void OnNavigating(ShellNavigatingEventArgs args)
    {
        base.OnNavigating(args);

        ShellNavigatingDeferral token = args.GetDeferral();

        var result = await DisplayActionSheet("Navigate?", "Cancel", "Yes", "No");
        if (result != "Yes")
        {
            args.Cancel();
        }
        token.Complete();
    }    
}
```

В этом примере отображается лист действий, приглашающий пользователя завершить запрос навигации или отменить его. Переход отменяется вызовом метода [`Cancel`](xref:Xamarin.Forms.ShellNavigatingEventArgs.Cancel*) для объекта [`ShellNavigatingEventArgs`](xref:Xamarin.Forms.ShellNavigatingEventArgs). Переход завершается вызовом метода [`Complete`](xref:Xamarin.Forms.ShellNavigatingDeferral.Complete*) для токена [`ShellNavigatingDeferral`](xref:Xamarin.Forms.ShellNavigatingDeferral), полученного методом `GetDeferral`, для объекта `ShellNavigatingEventArgs`.

> [!WARNING]
> Если пользователь пытается выполнить переход, когда имеется ожидающий отложенный переход, метод [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*) выдает исключение `InvalidOperationException`.

## <a name="pass-data"></a>Передача данных

Данные можно передавать в параметрах запроса при выполнении программной навигации на основе URI. Это достигается путем добавления `?` после маршрута, за которым следует идентификатор параметра запроса, `=` и значение. К примеру, следующий код выполняется в этом примере приложения, когда пользователь выбирает слона на странице `ElephantsPage`:

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"elephantdetails?name={elephantName}");
}
```

Этот пример кода получает значения выбранного слона в параметре [`CollectionView`](xref:Xamarin.Forms.CollectionView) и переходит по маршруту `elephantdetails`, передавая `elephantName` как параметр запроса.

Чтобы получать данные, класс той страницы, на которую осуществляется переход, или класс [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) этой страницы должен иметь атрибут [`QueryPropertyAttribute`](xref:Xamarin.Forms.QueryPropertyAttribute) для каждого параметра запроса:

```csharp
[QueryProperty(nameof(Name), "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            LoadAnimal(value);
        }
    }
    ...

    void LoadAnimal(string name)
    {
        try
        {
            Animal animal = ElephantData.Elephants.FirstOrDefault(a => a.Name == name);
            BindingContext = animal;
        }
        catch (Exception)
        {
            Console.WriteLine("Failed to load animal.");
        }
    }    
}
```

Первый аргумент в [`QueryPropertyAttribute`](xref:Xamarin.Forms.QueryPropertyAttribute) указывает имя свойства, которое будет получать данные, а второй аргумент задает идентификатор параметра запроса. Таким образом, атрибут `QueryPropertyAttribute` в примере выше указывает, что свойство `Name` будет получать данные из параметра запроса `name`, переданного в URI, через вызов метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*). Метод задания свойств `Name` вызывает метод `LoadAnimal` для получения объекта `Animal` для `name` и задает его в качестве [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы.

> [!IMPORTANT]
> Значения параметров запросов автоматически декодируются в URL-адресе.

### <a name="pass-multiple-query-parameters"></a>Передача нескольких параметров запроса

Можно передавать несколько параметров запросов, соединяя их с помощью `&`. Например, следующий код передает два элемента данных:

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    string elephantLocation = (e.CurrentSelection.FirstOrDefault() as Animal).Location;
    await Shell.Current.GoToAsync($"elephantdetails?name={elephantName}&location={elephantLocation}");
}
```

Этот пример кода получает значения выбранного слона в параметре [`CollectionView`](xref:Xamarin.Forms.CollectionView) и переходит по маршруту `elephantdetails`, передавая `elephantName` и `elephantLocation` как параметры запроса.

Чтобы получать данные, класс той страницы, на которую осуществляется переход, или класс [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) этой страницы должен иметь атрибут [`QueryPropertyAttribute`](xref:Xamarin.Forms.QueryPropertyAttribute) для каждого параметра запроса:

```csharp
[QueryProperty(nameof(Name), "name")]
[QueryProperty(nameof(Location), "location")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            // Custom logic
        }
    }

    public string Location
    {
        set
        {
            // Custom logic
        }
    }
    ...    
}
```

В этом примере класс имеет атрибут [`QueryPropertyAttribute`](xref:Xamarin.Forms.QueryPropertyAttribute) для каждого параметра запроса. Первый атрибут `QueryPropertyAttribute` указывает, что свойство `Name` получит данные, переданные в параметре запроса `name`, а второй атрибут `QueryPropertyAttribute` указывает, что свойство `Location` получит данные, переданные в параметре запроса `location`. В обоих случаях значения параметров запросов указываются в URI в вызове метода [`GoToAsync`](xref:Xamarin.Forms.Shell.GoToAsync*).

## <a name="back-button-behavior"></a>Действие кнопки "Назад"

Внешний вид и поведение кнопки "Назад" можно переопределить, присвоив присоединенное свойство [`BackButtonBehavior`](xref:Xamarin.Forms.Shell.BackButtonBehaviorProperty) объекту [`BackButtonBehavior`](xref:Xamarin.Forms.BackButtonBehavior). Класс [`BackButtonBehavior`](xref:Xamarin.Forms.BackButtonBehavior) определяет следующие свойства:

- [`Command`](xref:Xamarin.Forms.BackButtonBehavior.Command) с типом `ICommand`, который выполняется при нажатии кнопки "Назад".
- [`CommandParameter`](xref:Xamarin.Forms.BackButtonBehavior.CommandParameter) с типом `object`, который передается как параметр в `Command`.
- [`IconOverride`](xref:Xamarin.Forms.BackButtonBehavior.IconOverride) с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource), который содержит значок для кнопки "Назад".
- [`IsEnabled`](xref:Xamarin.Forms.BackButtonBehavior.IsEnabled) с типом `boolean`, который указывает, доступна ли кнопка перехода назад. Значение по умолчанию — `true`.
- [`TextOverride`](xref:Xamarin.Forms.BackButtonBehavior.TextOverride) с типом `string`, который содержит текст для кнопки "Назад".

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

В следующем коде показан пример переопределения внешнего вида и поведения кнопки "Назад":

```xaml
<ContentPage ...>    
    <Shell.BackButtonBehavior>
        <BackButtonBehavior Command="{Binding BackCommand}"
                            IconOverride="back.png" />   
    </Shell.BackButtonBehavior>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
Shell.SetBackButtonBehavior(this, new BackButtonBehavior
{
    Command = new Command(() =>
    {
        ...
    }),
    IconOverride = "back.png"
});
```

Свойство [`Command`](xref:Xamarin.Forms.BackButtonBehavior.Command) получает значение `ICommand` для выполнения при нажатии кнопки "Назад", а в свойстве `IconOverride` сохраняется значок, используемый для кнопки "Назад":

[![Снимок экрана с переопределением значка для кнопки "Назад" в оболочке для iOS и Android](navigation-images/back-button.png)](navigation-images/back-button-large.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
