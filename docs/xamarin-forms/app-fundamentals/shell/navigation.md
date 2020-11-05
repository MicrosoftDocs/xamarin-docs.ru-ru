---
title: Навигация по оболочке Xamarin.Forms
description: Приложения оболочки Xamarin.Forms предоставляют улучшенные возможности навигации по интерфейсу, позволяя переходить на любую страницу в приложении без необходимости навигации по заданной иерархии.
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ee027399aec51bb7ae4fac15e9c706c65d3af235
ms.sourcegitcommit: d1980b2251999224e71c1289e4b4097595b7e261
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2020
ms.locfileid: "92928637"
---
# <a name="no-locxamarinforms-shell-navigation"></a>Навигация по оболочке Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Оболочка Xamarin.Forms предоставляет улучшенные возможности навигации по интерфейсу на основе URI, позволяя переходить на любую страницу в приложении без соблюдения строгой иерархии. Кроме того, они также дают возможность перехода назад без необходимости прохода всех страниц в стеке навигации.

`Shell` определяет следующие свойства, связанные с навигацией.

- Присоединенное свойство `BackButtonBehavior` с типом `BackButtonBehavior` определяет поведение кнопки "Назад".
- `CurrentItem` с типом `FlyoutItem` обозначает выбранный `FlyoutItem`.
- `CurrentState` с типом `ShellNavigationState` обозначает текущее состояние навигации для `Shell`.
- `Current` с типом `Shell`, является приведенным по типу псевдонимом для `Application.Current.MainPage`.

Свойства `BackButtonBehavior`, `CurrentItem` и `CurrentState` поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть эти свойства можно указывать в качестве целевых для привязки данных.

Навигация выполняется путем вызова метода `GoToAsync` из класса `Shell`. Когда планируется действие навигации, срабатывает событие `Navigating`, а после завершения навигации — событие `Navigated`.

> [!NOTE]
> Навигацию в приложении оболочки Xamarin.Forms можно по-прежнему выполнять с помощью свойства [Navigation](xref:Xamarin.Forms.NavigableElement.Navigation). Дополнительные сведения см. в статье [об иерархической навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## <a name="routes"></a>Маршруты

Навигация в приложении оболочки выполняется путем указания URI для перехода. URI навигации могут иметь три компонента.

- *Маршрут* определяет путь к содержимому, которое существует в визуальной иерархии оболочки.
- *Страница*. Страницы не отражены в визуальной иерархии оболочки и могут добавляться в стек навигации из любого места в приложении оболочки. Например, страница подробностей об элементе не будет определяться в визуальной иерархии оболочки, но может быть добавлена в стек навигации при необходимости.
- Один или несколько *параметров запроса*. Параметры запроса могут передаваться странице назначения при переходе на нее.

Если URI навигации содержит всех три компонента, он имеет такую структуру: //маршрут/страница?параметры_запроса

### <a name="register-routes"></a>Регистрация маршрутов

Маршруты можно определить для объектов `FlyoutItem`, `Tab` и `ShellContent` с помощью свойств `Route`:

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
> С каждым элементом в иерархии оболочки сопоставляется маршрут. Если маршрут не задан разработчиком, он создается во время выполнения. Но для автоматических маршрутов не гарантируется согласованность между сеансами приложения.

Этот фрагмент из примера приложения создает описанную ниже иерархию маршрутов, которую можно использовать для программной навигации:

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

Чтобы перейти к объекту `ShellContent` по маршруту `dogs`, абсолютный URI должен выглядеть так: `//animals/domestic/dogs`. Аналогичным образом для перехода к объекта `ShellContent` по маршруту `about` абсолютный URI должен выглядеть так: `//about`.

> [!IMPORTANT]
> Если будет обнаружен дубликат маршрута, при запуске приложения создается исключение `ArgumentException`. Это исключение также будет создаваться, если два или более маршрута на одном уровне иерархии совместно используют имя маршрута.

#### <a name="register-page-routes"></a>Регистрация страниц маршрутов

В конструкторе подкласса оболочки или в любой другой части кода, которая выполняется перед вызовом маршрута, можно явно зарегистрировать дополнительные маршруты для любых страниц, которые не представлены в визуальной иерархии оболочки:

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

Этот пример регистрирует страницу сведений об элементе, для которой не определены маршруты в подклассе оболочки. К таким страницам можно переходить по URI из любой точки в приложении. Маршруты для таких страниц называются *глобальными маршрутами*.

> [!NOTE]
> Если для страницы зарегистрированы маршруты с помощью метода `Routing.RegisterRoute`, вы можете отменить такую регистрацию с помощью метода `Routing.UnRegisterRoute`, если потребуется.

Также вы можете регистрировать страницы в другие иерархии маршрутов, если потребуется:

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

Этот пример настраивает контекстную навигацию по страницам, где переход по маршруту `details` со страницы для маршрута `monkeys` отображает `MonkeyDetailPage`. Аналогичным образом переход по маршруту `details` со страницы для маршрута `elephants` отображает `ElephantDetailPage`.

> [!IMPORTANT]
> Если метод `Routing.RegisterRoute` пытается зарегистрировать один и тот же маршрут к двум или более разным типам, будет создано исключение `ArgumentException`.

## <a name="perform-navigation"></a>Выполнение перемещения

Чтобы выполнить перемещение, нужно получить ссылку на подкласс `Shell`. Чтобы получить эту ссылку, можно привести свойство `App.Current.MainPage` к объекту `Shell` или получить значение свойства `Shell.Current`. После этого выполняется перемещение путем вызова метода `GoToAsync` из объекта `Shell`. Этот метод переходит к `ShellNavigationState` и возвращает `Task`, который завершается после завершения анимации для этого перехода. Объект `ShellNavigationState` создается с помощью метода `GoToAsync` из `string` или `Uri` и для его свойства `Location` задается значение аргумента `string` или `Uri`.

При переходе по маршруту, входящему в визуальную иерархию оболочки, стек навигации не создается. Но при переходе к странице, которая не входит в визуальную иерархию оболочки, создается стек навигации.

> [!NOTE]
> Текущее состояние навигации для `Shell` можно извлечь через свойство `Shell.Current.CurrentState`, которое содержит URI отображаемого маршрута в свойстве `Location`.

### <a name="absolute-routes"></a>Абсолютные маршруты

Для навигации можно передать допустимый абсолютный URI в качестве аргумента в метод `GoToAsync`:

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

Этот пример выполняет переход на страницу с маршрутом `monkeys`, который определен в объекте `ShellContent`. Объект `ShellContent`, который представляет маршрут `monkeys`, является дочерним элементом объекта `FlyoutItem` с маршрутом `animals`.

### <a name="relative-routes"></a>Относительные маршруты

Для навигации можно также передать допустимый относительный URI в качестве аргумента в метод `GoToAsync`: Система маршрутизации попытается сопоставить URI с объектом `ShellContent`. Таким образом, если все маршруты в приложении уникальны, для навигации достаточно указать уникальное имя маршрута в качестве относительного URI.

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

В этом примере выполняется переход на страницу с маршрутом `monkeydetails`.

Кроме того, поддерживаются следующие форматы относительных маршрутов.

| Формат | Описание |
| --- | --- |
| //*маршрут* | Поиск указанного маршрута выполняется в иерархии маршрутов вверх, начиная от текущего отображаемого маршрута. |
| ///*маршрут* | Поиск указанного маршрута выполняется в иерархии маршрутов вниз, начиная от текущего отображаемого маршрута. |

#### <a name="contextual-navigation"></a>Контекстная навигация

Относительные маршруты позволяют организовать контекстную навигацию. Для примера рассмотрим следующую иерархию маршрутов:

```
monkeys
  details
bears
  details
```

Когда отображается страница с зарегистрированным маршрутом `monkeys`, переход по маршруту `details` отображает страницу с зарегистрированным маршрутом `monkeys/details`. Аналогичным образом, когда отображается страница с зарегистрированным маршрутом `bears`, переход по маршруту `details` отображает страницу с зарегистрированным маршрутом `bears/details`. Сведения о том, как зарегистрировать маршруты для этого примера, см. в [этой статье](#register-page-routes).

### <a name="backwards-navigation"></a>Обратная навигация

Для выполнения обратной навигации можно указать ".." в качестве аргумента для метода `GotoAsync`:

```csharp
await Shell.Current.GoToAsync("..");
```

Обратную навигацию с ".." можно также объединить с маршрутом, как показано далее:

```csharp
await Shell.Current.GoToAsync("../route");
```

В этом примере общим результатом является переход назад, а затем переход по указанному маршруту.

> [!IMPORTANT]
> Переход назад и по указанному маршруту возможен, только если при обратной навигации вы будете находиться в текущем местоположении в иерархии маршрутов для перехода по указанному маршруту.

Аналогичным образом можно перемещаться назад несколько раз, а затем переходить по указанному маршруту:

```csharp
await Shell.Current.GoToAsync("../../route");
```

В этом примере общим результатом является переход назад два раза, а затем переход по указанному маршруту.

> [!NOTE]
> При переходе с ".." можно также передавать данные. Дополнительные сведения см. в разделе [Передача данных](#pass-data).

### <a name="invalid-routes"></a>Недопустимые маршруты

Следующие форматы маршрутов считаются недопустимыми.

| Формат | Объяснение |
| --- | --- |
| *маршрут* или / *маршрут* | Маршруты в визуальной иерархии нельзя принудительно передать в стек навигации. |
| //*страница* или / / / *страница* | В настоящее время глобальные маршруты не могут быть единственной страницей в стеке навигации. Таким образом, абсолютная маршрутизация для глобальных маршрутов не поддерживается. |

Использование любого из этих форматов маршрутов приводит к созданию исключения `Exception`.

> [!IMPORTANT]
> Попытка перехода на несуществующий маршрут приводит к созданию исключения `ArgumentException`.

### <a name="debugging-navigation"></a>Отладка навигации

Некоторые классы оболочки дополняются `DebuggerDisplayAttribute`, который определяет отображение класса или поля в отладчике. Это помогает в отладке запросов навигации, отображая актуальные данные для запроса перехода. Например, на следующем снимке экрана показаны свойства `CurrentItem` и `CurrentState` объекта `Shell.Current`:

![Снимок экрана отладчика](navigation-images/debugger.png "Отладчик")

В этом примере свойство `CurrentItem` с типом `FlyoutItem` отображает название и маршрут объекта `FlyoutItem`. Аналогичным образом свойство `CurrentState` с типом `ShellNavigationState` отображает URI отображаемого маршрута в приложении оболочки.

### <a name="tab-class"></a>Класс Tab

Класс `Tab` определяет свойство `Stack` с типом `IReadOnlyList<Page>`, которое представляет текущий стек навигации в пределах `Tab`. Этот класс также предоставляет следующие переопределяемые методы навигации:

- `GetNavigationStack`, который возвращает текущий стек навигации `IReadOnlyList<Page`>.
- `OnInsertPageBefore`, который вызывается при вызове метода `INavigation.InsertPageBefore`.
- `OnPopAsync`, который возвращает `Task<Page>` и вызывается при вызове `INavigation.PopAsync`.
- `OnPopToRootAsync`, который возвращает `Task` и вызывается при вызове `INavigation.OnPopToRootAsync`.
- `OnPushAsync`, который возвращает `Task` и вызывается при вызове `INavigation.PushAsync`.
- `OnRemovePage`, который вызывается при вызове метода `INavigation.RemovePage`.

## <a name="navigation-events"></a>События навигации

Класс `Shell` определяет событие `Navigating`, которое возникает перед выполнением любого перехода — программного или вызванного действием пользователя. Объект `ShellNavigatingEventArgs`, который прилагается к событию `Navigating`, содержит следующие свойства:

| Свойство. | type | Описание |
|---|---|---|
| `Current` | `ShellNavigationState` | URI текущей страницы. |
| `Source` | `ShellNavigationSource` | Тип выполненного перехода. |
| `Target` | `ShellNavigationState`  | URI, представляющий целевой объект навигации. |
| `CanCancel`  | `bool` | Значение, указывающее, возможна ли отмена перехода. |
| `Cancelled`  | `bool` | Значение, указывающее, была ли навигация отменена. |

Кроме того, класс `ShellNavigatingEventArgs` предоставляет метод `Cancel` для отмены навигации.

Класс `Shell` также определяет событие `Navigated`, которое возникает при завершении навигации. Объект `ShellNavigatedEventArgs`, который прилагается к событию `Navigating`, содержит следующие свойства:

| Свойство. | Тип | Описание |
|---|---|---|
| `Current` | `ShellNavigationState` | URI текущей страницы. |
| `Previous`| `ShellNavigationState` | URI предыдущей страницы. |
| `Source`  | `ShellNavigationSource` | Тип выполненного перехода. |

Классы `ShellNavigatedEventArgs` и `ShellNavigatingEventArgs` имеют свойства `Source` с типом `ShellNavigationSource`. Значения перечисления указаны ниже:

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

Таким образом, в обработчике события `Navigating` можно перехватить навигацию и выполнить действия в зависимости от источника навигации. Например, следующий код позволяет отменить переход назад, если на странице есть несохраненные данные:

```csharp
void OnNavigating(object sender, ShellNavigatingEventArgs e)
{
    // Cancel back navigation if data is unsaved
    if (e.Source == ShellNavigationSource.Pop && !dataSaved)
    {
        e.Cancel();
    }
}
```

## <a name="pass-data"></a>Передача данных

Данные можно передавать в параметрах запроса при выполнении программной навигации. К примеру, следующий код выполняется в этом примере приложения, когда пользователь выбирает слона на странице `ElephantsPage`:

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"//animals/elephants/elephantdetails?name={elephantName}");
}
```

Этот пример кода получает значения выбранного слона в параметре [`CollectionView`](xref:Xamarin.Forms.CollectionView) и переходит по маршруту `elephantdetails`, передавая `elephantName` как параметр запроса. Обратите внимание, что к параметрам запроса навигации применяется URL-кодирование, поэтому "Indian Elephant" передается как "Indian%20Elephant".

Чтобы получать данные, класс той страницы, на которую осуществляется переход, или класс [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) этой страницы должен иметь атрибут `QueryPropertyAttribute` для каждого параметра запроса:

```csharp
[QueryProperty("Name", "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            BindingContext = ElephantData.Elephants.FirstOrDefault(m => m.Name == Uri.UnescapeDataString(value));
        }
    }
    ...
}
```

Первый аргумент в `QueryPropertyAttribute` указывает имя свойства, которое будет получать данные, а второй аргумент задает идентификатор параметра запроса. Таким образом, атрибут `QueryPropertyAttribute` в примере выше указывает, что свойство `Name` будет получать данные из параметра запроса `name`, переданного в URI, через вызов метода `GoToAsync`. Затем свойство `Name` расшифровывает URL-кодирование значения параметра запроса и присваивает свойству [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) страницы значение отображаемого объекта.

> [!NOTE]
> Класс можно дополнить несколькими объектами `QueryPropertyAttribute`.

## <a name="back-button-behavior"></a>Действие кнопки "Назад"

Класс `BackButtonBehavior` определяет следующие свойства, которые определяют внешний вид и поведение кнопки "Назад".

- `Command` с типом `ICommand`, который выполняется при нажатии кнопки "Назад".
- `CommandParameter` с типом `object`, который передается как параметр в `Command`.
- `IconOverride` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource), который содержит значок для кнопки "Назад".
- `IsEnabled` с типом `boolean`, который указывает, доступна ли кнопка перехода назад. Значение по умолчанию — `true`.
- `TextOverride` с типом `string`, который содержит текст для кнопки "Назад".

Все эти свойства поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

Класс `BackButtonBehavior` можно использовать, присвоив вложенное свойство `Shell.BackButtonBehavior` объекту `BackButtonBehavior`:

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

Свойство `Command` получает значение `ICommand` для выполнения при нажатии кнопки "Назад", а в свойстве `IconOverride` сохраняется значок, используемый для кнопки "Назад":

[![Снимок экрана с переопределением значка для кнопки "Назад" в оболочке для iOS и Android](navigation-images/back-button.png "Переопределение значка кнопки \"Назад\" в оболочке")](navigation-images/back-button-large.png#lightbox "Переопределение значка кнопки \"Назад\" в оболочке")

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
