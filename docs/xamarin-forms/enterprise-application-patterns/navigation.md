---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9b2ee9fb02a8fd18d69e93424dc76bfd54fafc86
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130901"
---
# <a name="enterprise-app-navigation"></a>Навигация по корпоративному приложению

Xamarin.Formsвключает поддержку навигации по страницам, что обычно приводит к взаимодействию пользователя с пользовательским ИНТЕРФЕЙСом или из самого приложения в результате внутренних изменений состояния на основе логики. Однако Навигация может быть сложной для реализации в приложениях, использующих шаблон Model-View-ViewModel (MVVM), так как должны выполняться следующие задачи:

- Определение представления, к которому осуществляется переход, с использованием подхода, который не обеспечивает тесное связывание и зависимости между представлениями.
- Способ координации процесса, с которым будет осуществляться переход к представлению, — это экземпляр и инициализирован. При использовании MVVM модель представления и представления необходимо создать и связать друг с другом через контекст привязки представления. Если приложение использует контейнер внедрения зависимостей, то для создания экземпляров представлений и моделей представлений может потребоваться специальный механизм построения.
- Следует ли выполнять навигацию или Просмотр первой модели. С помощью навигации «Просмотр — первый» страница, к которой выполняется переход, ссылается на имя типа представления. Во время навигации создается экземпляр указанного представления вместе с соответствующей моделью представления и другими зависимыми службами. Альтернативный подход заключается в использовании навигации «Просмотр модели — первый», где страница, к которой осуществляется переход, ссылается на имя типа модели представления.
- Как четко разделить поведение навигации приложения по представлениям и моделям представления. Шаблон MVVM обеспечивает разделение между пользовательским интерфейсом приложения и его представлением и бизнес-логикой. Однако поведение навигации приложения часто будет охватывать части пользовательского интерфейса и презентации приложения. Пользователь часто инициирует навигацию из представления, и представление будет заменено в результате навигации. Однако при этом часто бывает необходимо инициировать или координировать навигацию в модели представления.
- Передача параметров во время навигации для целей инициализации. Например, если пользователь переходит к представлению для обновления сведений о заказе, данные заказа должны быть переданы в представление, чтобы они могли отображать правильные данные.
- Принцип совместной навигации для обеспечения соблюдения определенных бизнес-правил. Например, пользователи могут получить запрос перед переходом от представления, чтобы они могли исправлять любые недопустимые данные, или выдать запрос на отправку или отмену изменений данных, внесенных в представление.

Эта глава решает эти проблемы, предоставляя `NavigationService` класс, который используется для выполнения просмотра модели. Первая страница навигации.

> [!NOTE]
> Объект, `NavigationService` используемый приложением, предназначен только для выполнения иерархической навигации между экземплярами ContentPage. Использование службы для перехода между другими типами страниц может привести к непредвиденному поведению.

## <a name="navigating-between-pages"></a>Переход между страницами

Логика навигации может находиться в коде программной части представления или в модели представления с привязкой к данным. Хотя логика навигации в представлении может быть самым простым подходом, ее легко тестировать с помощью модульных тестов. Размещение логики навигации в классах модели представления означает, что логику можно использовать в модульных тестах. Кроме того, модель представления может реализовать логику для управления навигацией, чтобы обеспечить применение определенных бизнес-правил. Например, приложение может не позволить пользователю перейти с страницы на другую страницу, не убедившись в том, что входные данные являются допустимыми.

`NavigationService`Класс обычно вызывается из моделей представления для повышения пригодности для тестирования. Однако переход к представлениям из моделей представлений потребует, чтобы модели представления ссылались на представления, а в частности представления, которые не связаны с моделью активного представления, что не рекомендуется. Таким образом, `NavigationService` представленный здесь тип модели представления указывается как целевой объект для перехода.

Мобильное приложение eShopOnContainers использует `NavigationService` класс для предоставления навигации представления модели. Этот класс реализует `INavigationService` интерфейс, который показан в следующем примере кода:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Этот интерфейс указывает, что реализующий класс должен предоставлять следующие методы:

|Метод|Назначение|
|--- |--- |
|`InitializeAsync`|Выполняет переход на одну из двух страниц при запуске приложения.|
|`NavigateToAsync`|Выполняет иерархическую навигацию на указанной странице.|
|`NavigateToAsync(parameter)`|Выполняет иерархическую навигацию на указанной странице, передавая параметр.|
|`RemoveLastFromBackStackAsync`|Удаляет предыдущую страницу из стека навигации.|
|`RemoveBackStackAsync`|Удаляет все предыдущие страницы из стека навигации.|

Кроме того, `INavigationService` интерфейс указывает, что реализующий класс должен предоставлять `PreviousPageViewModel` свойство. Это свойство возвращает тип модели представления, связанный с предыдущей страницей в стеке навигации.

> [!NOTE]
> `INavigationService`Интерфейс, как правило, также указывает `GoBackAsync` метод, который используется для программного возврата на предыдущую страницу стека навигации. Однако этот метод отсутствует в мобильном приложении eShopOnContainers, так как он не является обязательным.

### <a name="creating-the-navigationservice-instance"></a>Создание экземпляра NavigationService

`NavigationService`Класс, который реализует `INavigationService` интерфейс, регистрируется как одноэлементный с помощью контейнера внедрения зависимостей Autofac, как показано в следующем примере кода:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService`Интерфейс разрешается в `ViewModelBase` конструкторе класса, как показано в следующем примере кода:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Он возвращает ссылку на `NavigationService` объект, хранящийся в контейнере внедрения зависимостей Autofac, который создается `InitNavigation` методом в `App` классе. Дополнительные сведения см. [в разделе переходы при запуске приложения](#navigating_when_the_app_is_launched).

`ViewModelBase`Класс сохраняет `NavigationService` экземпляр в `NavigationService` свойстве типа `INavigationService` . Таким образом, все классы модели представления, производные от `ViewModelBase` класса, могут использовать `NavigationService` свойство для доступа к методам, заданным в `INavigationService` интерфейсе. Это позволяет избежать издержек при внедрении `NavigationService` объекта из контейнера внедрения зависимостей Autofac в каждый класс модели представления.

### <a name="handling-navigation-requests"></a>Обработка запросов навигации

Xamarin.Formsпредоставляет [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) класс, реализующий иерархическую навигацию, в которой пользователь может перемещаться по страницам, перемещая и обратнее по мере необходимости. Дополнительные сведения об иерархической навигации см. в статье [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Вместо [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) непосредственного использования класса приложение eShopOnContainers заключает `NavigationPage` класс в `CustomNavigationView` класс, как показано в следующем примере кода:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Цель этого переноса заключается в простоте стилизации [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) экземпляра внутри XAML-файла для класса.

Навигация выполняется внутри классов модели представления путем вызова одного из `NavigateToAsync` методов, указывающих тип модели представления для страницы, к которой осуществляется переход, как показано в следующем примере кода:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

В следующем примере кода показаны `NavigateToAsync` методы, предоставляемые `NavigationService` классом:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Каждый метод позволяет любому классу модели представления, производному от `ViewModelBase` класса, выполнять иерархическую навигацию, вызывая `InternalNavigateToAsync` метод. Кроме того, второй `NavigateToAsync` метод позволяет указать данные навигации в качестве аргумента, который передается в модель представления, к которой осуществляется переход, где обычно используется для выполнения инициализации. Дополнительные сведения см. в разделе [Передача параметров во время навигации](#passing_parameters_during_navigation).

`InternalNavigateToAsync`Метод выполняет запрос навигации и показан в следующем примере кода:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync`Метод выполняет переход к модели представления при первом вызове `CreatePage` метода. Этот метод находит представление, соответствующее указанному типу модели представления, и создает и возвращает экземпляр этого типа представления. Поиск представления, соответствующего типу модели представления, использует подход на основе соглашения, который предполагает, что:

- Представления находятся в той же сборке, что и типы моделей представления.
- Представления находятся в. Дочернее пространство имен views.
- Модели представления находятся в. Дочернее пространство имен ViewModel.
- Имена представлений соответствуют именам моделей представления с удаленным "моделью".

При создании экземпляра представления он связывается с соответствующей моделью представления. Дополнительные сведения о том, как это происходит, см. в разделе [Автоматическое создание модели представления с локатором модели представления](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Если создаваемое представление имеет значение `LoginView` , оно упаковывается внутри нового экземпляра `CustomNavigationView` класса и назначается [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) свойству. В противном случае `CustomNavigationView` извлекается экземпляр и при условии, что он не равен null, [`PushAsync`](xref:Xamarin.Forms.NavigationPage) вызывается метод для принудительной отправки представления, создаваемого в стеке навигации. Однако если полученный `CustomNavigationView` экземпляр имеет значение `null` , создаваемое представление упаковывается внутри нового экземпляра `CustomNavigationView` класса и назначается `Application.Current.MainPage` свойству. Этот механизм гарантирует, что при переходе страницы будут правильно добавляться в стек навигации, когда он пуст и когда он содержит данные.

> [!TIP]
> Рассмотрите возможность кэширования страниц. Кэширование страниц приводит к потреблению памяти для представлений, которые в настоящее время не отображаются. Однако без кэширования страницы это означает, что синтаксический анализ XAML и создание страницы и ее модели представления будут выполняться каждый раз при переходе к новой странице, что может оказать влияние на производительность сложной страницы. Для хорошо спроектированной страницы, которая не использует слишком много элементов управления, производительность должна быть достаточной. Однако кэширование страниц может помочь в случае возникновения длительной загрузки страниц.

После создания представления и перехода к нему `InitializeAsync` выполняется метод связанной модели представления представления. Дополнительные сведения см. в разделе [Передача параметров во время навигации](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Навигация при запуске приложения

При запуске приложения `InitNavigation` `App` вызывается метод в классе. Этот метод показан в следующем примере кода:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Метод создает новый `NavigationService` объект в контейнере внедрения зависимостей Autofac и возвращает ссылку на него перед вызовом `InitializeAsync` метода.

> [!NOTE]
> Когда `INavigationService` интерфейс разрешается `ViewModelBase` классом, контейнер возвращает ссылку на `NavigationService` объект, который был создан при вызове метода инитнавигатион.

Метод `NavigationService` `InitializeAsync` показан в следующем примере кода:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView`Переходит к, если приложение имеет кэшированный маркер доступа, который используется для проверки подлинности. В противном случае `LoginView` переходит к.

Дополнительные сведения о контейнере внедрения зависимостей Autofac см. [в разделе Введение в внедрение зависимостей](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Передача параметров во время навигации

Один из `NavigateToAsync` методов, `INavigationService` заданных интерфейсом, позволяет указать данные навигации в качестве аргумента, который передается в модель представления, к которой осуществляется переход, где обычно используется для выполнения инициализации.

Например, `ProfileViewModel` класс содержит объект `OrderDetailCommand` , который выполняется, когда пользователь выбирает заказ на `ProfileView` странице. В свою очередь, выполняет `OrderDetailAsync` метод, который показан в следующем примере кода:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Этот метод вызывает навигацию в `OrderDetailViewModel` , передавая `Order` экземпляр, который представляет порядок, выбранный пользователем на `ProfileView` странице. Когда `NavigationService` класс создает `OrderDetailView` , `OrderDetailViewModel` экземпляр класса создается и назначается для представления [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . После перехода к службам `OrderDetailView` `InternalNavigateToAsync` метод выполняет `InitializeAsync` метод модели представления, связанной с представлением.

`InitializeAsync`Метод определяется в `ViewModelBase` классе как метод, который можно переопределить. Этот метод задает `object` аргумент, представляющий данные, передаваемые в модель представления во время операции навигации. Поэтому Просмотрите классы модели, которые хотят получить данные из операции перехода, и предоставьте собственную реализацию `InitializeAsync` метода для выполнения необходимой инициализации. В следующем примере кода показан `InitializeAsync` метод из `OrderDetailViewModel` класса:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Этот метод извлекает `Order` экземпляр, который был передан в модель представления во время операции навигации, и использует его для получения полных сведений о заказе из `OrderService` экземпляра.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Вызов навигации с помощью поведений

Навигация обычно запускается из представления с помощью взаимодействия с пользователем. Например, `LoginView` выполняет навигацию после успешной проверки подлинности. В следующем примере кода показано, как вызывается Навигация с помощью поведения:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Во время выполнения `EventToCommandBehavior` будет реагировать на взаимодействие с [`WebView`](xref:Xamarin.Forms.WebView) . При `WebView` переходе на веб-страницу [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) будет срабатывать событие, которое будет выполняться `NavigateCommand` в `LoginViewModel` . По умолчанию аргументы события для события передаются в команду. Эти данные преобразуются при передаче между источником и целевым объектом преобразователем, указанным в `EventArgsConverter` свойстве, которое возвращает [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) из [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs) . Таким образом, при `NavigationCommand` выполнении объект URL веб-страницы передается в качестве параметра зарегистрированному `Action` .

В свою очередь, `NavigationCommand` выполняет `NavigateAsync` метод, который показан в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Этот метод вызывает навигацию на `MainViewModel` , а также при переходе удаляет `LoginView` страницу из стека навигации.

### <a name="confirming-or-cancelling-navigation"></a>Подтверждение или отмена навигации

Приложению может потребоваться взаимодействовать с пользователем во время операции навигации, чтобы пользователь мог подтвердить или отменить навигацию. Это может быть необходимо, например, когда пользователь пытается выполнить переход до полного заполнения страницы ввода данных. В этом случае приложение должно предоставить уведомление, позволяющее пользователю перейти с страницы или отменить операцию перехода до ее возникновения. Это можно сделать в классе модели представления с помощью ответа от уведомления, чтобы определить, вызывается ли переход.

## <a name="summary"></a>Сводка

Xamarin.Formsвключает поддержку навигации по страницам, что обычно приводит к взаимодействию пользователя с пользовательским ИНТЕРФЕЙСом или из самого приложения в результате внутренних изменений состояния, управляемых логическими операциями. Однако Навигация может быть сложной для реализации в приложениях, использующих шаблон MVVM.

В этой главе представлен `NavigationService` класс, который используется для выполнения навигации по модели представления при первом просмотре моделей. Размещение логики навигации в классах представления позволяет выполнять логику с помощью автоматических тестов. Кроме того, модель представления может реализовать логику для управления навигацией, чтобы обеспечить применение определенных бизнес-правил.

## <a name="related-links"></a>Связанные ссылки

- [Скачать электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
