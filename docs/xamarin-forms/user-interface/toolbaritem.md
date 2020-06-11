---
Title: " Xamarin.Forms тулбаритем" Описание: "класс тулбаритем является специальной разновидностью кнопки, используемой на панели навигации приложения".
MS. произв. Xamarin MS. assetId: CC737D54-0280-46BD-A2BC-A0FB67DDD6A1 MS. Technology: Xamarin-Forms author: профексоржеек MS. author: жусжохнс МС. Дата: 07/29/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-toolbaritem"></a>Xamarin.Formsтулбаритем

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)

Xamarin.Forms [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Класс является специальной разновидностью кнопки, которую можно добавить в `Page` `ToolbarItems` коллекцию объекта. Каждый `ToolbarItem` объект будет отображаться в виде кнопки на панели навигации приложения. `ToolbarItem`Экземпляр может иметь значок и отображаться как первичный или вторичный элемент меню. `ToolbarItem`Класс наследует от [`MenuItem`](xref:Xamarin.Forms.MenuItem) .

На следующих снимках экрана показаны `ToolbarItem` объекты на панели навигации в iOS и Android:

!["Демонстрационный снимок экрана Тулбаритем на Android и iOS"](toolbaritem-images/toolbaritem-device-screenshot.png "Демонстрационный снимок экрана Тулбаритем на устройствах Android и iOS")

`ToolbarItem`Класс определяет следующие свойства:

* [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)`ToolbarItemOrder`значение перечисления, которое определяет, отображается ли `ToolbarItem` экземпляр в основном или дополнительном меню.
* [`Priority`](xref:Xamarin.Forms.ToolbarItem.Priority)`integer`значение, определяющее порядок вывода элементов в `Page` `ToolbarItems` коллекции объекта.

`ToolbarItem`Класс наследует следующие свойства, обычно используемые из `MenuItem` класса:

* [`Command`](xref:Xamarin.Forms.MenuItem.Command)— Это `ICommand` , которое позволяет привязать пользовательские действия, такие как касания пальца или щелчки, к командам, определенным в ViewModel.
* [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)Аргумент `object` , указывающий параметр, который должен быть передан в `Command` .
* [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)`ImageSource`значение, определяющее значок вывода `ToolbarItem` объекта.
* [`Text`](xref:Xamarin.Forms.MenuItem.Text)значение типа `string` , определяющее отображаемый текст `ToolbarItem` объекта.

Эти свойства поддерживаются объектами, [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) поэтому `ToolbarItem` экземпляр может быть целевым объектом привязок данных.

> [!NOTE]
> Альтернативой созданию панели инструментов из [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) объектов является присвоение [`NavigationPage.TitleView`](xref:Xamarin.Forms.NavigationPage.TitleViewProperty) присоединенному свойству класса макета, содержащего несколько представлений. Дополнительные сведения см. [в разделе Отображение представлений на панели навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md#displaying-views-in-the-navigation-bar).

## <a name="create-a-toolbaritem"></a>Создание Тулбаритем

`ToolbarItem`Экземпляр объекта можно создать на языке XAML. `Text`Свойства и `IconImageSource` можно задать, чтобы определить способ отображения кнопки на панели навигации. В следующем примере показано, как создать экземпляр `ToolbarItem` с некоторыми наборами общих свойств и добавить его в `ContentPage` `ToolbarItems` коллекцию.

```xaml
<ContentPage.ToolbarItems>
    <ToolbarItem Text="Example Item"
                 IconImageSource="example_icon.png"
                 Order="Primary"
                 Priority="0" />
</ContentPage.ToolbarItems>
```

В этом примере создается объект, `ToolbarItem` который содержит текст, значок и отображается первым в области основной панели навигации. `ToolbarItem`Можно также создать в коде и добавить в `ToolbarItems` коллекцию:

```csharp
ToolbarItem item = new ToolbarItem
{
    Text = "Example Item",
    IconImageSource = ImageSource.FromFile("example_icon.png"),
    Order = ToolbarItemOrder.Primary,
    Priority = 0
};

// "this" refers to a Page object
this.ToolbarItems.Add(item);
```

Файл, представленный `string` , предоставленный в качестве `IconImageSource` свойства, должен существовать в каждом проекте платформы.

> [!NOTE]
> Ресурсы изображений обрабатываются по-разному на каждой платформе. `ImageSource`Может поступать из источников, включая локальный файл или внедренный ресурс, универсальный код ресурса (URI) или поток. Дополнительные сведения о настройке `IconImageSource` свойств и изображений в Xamarin.Forms см. в разделе [изображения в Xamarin.Forms ](~/xamarin-forms/user-interface/images.md).

## <a name="define-button-behavior"></a>Определение поведения кнопки

`ToolbarItem`Класс наследует `Clicked` событие от `MenuItem` класса. Обработчик событий может быть присоединен к `Clicked` событию, чтобы реагировать на касания или на `ToolbarItem` экземпляры в XAML:

```xaml
<ToolbarItem ...
             Clicked="OnItemClicked" />
```

Обработчик событий можно также присоединить в коде:

```csharp
ToolbarItem item = new ToolbarItem { ... }
item.Clicked += OnItemClicked;
```

В предыдущих примерах указан `OnItemClicked` обработчик событий. В следующем коде показан пример реализации:

```csharp
void OnItemClicked(object sender, EventArgs e)
{
    ToolbarItem item = (ToolbarItem)sender;
    messageLabel.Text = $"You clicked the \"{item.Text}\" toolbar item.";
}
```

`ToolbarItem`объекты также могут использовать `Command` Свойства и `CommandParameter` для реагирования на ввод пользователя без обработчиков событий. Дополнительные сведения об `ICommand` интерфейсе и привязке данных MVVM см. в разделе [ Xamarin.Forms поведение MenuItem MVVM](~/xamarin-forms/user-interface/menuitem.md#define-menuitem-behavior-with-mvvm).

## <a name="enable-or-disable-a-toolbaritem-at-runtime"></a>Включение или отключение Тулбаритем во время выполнения

Чтобы включить отключение `ToolbarItem` во время выполнения, привяжите его `Command` свойство к `ICommand` реализации и убедитесь, что `canExecute` делегат включает и отключается `ICommand` соответствующим образом.

Дополнительные сведения см. в разделе [Включение или отключение элемента MenuItem во время выполнения](menuitem.md#enable-or-disable-a-menuitem-at-runtime).

## <a name="primary-and-secondary-menus"></a>Главное и дополнительное меню

`ToolbarItemOrder`Перечисление `Default` имеет `Primary` значения, и `Secondary` .

Если `Order` свойство имеет значение `Primary` , `ToolbarItem` объект будет отображаться на главной панели навигации на всех платформах. `ToolbarItem`объекты имеют приоритет над заголовком страницы, который будет обрезан, чтобы освободить место для элементов. На следующих снимках экрана показаны `ToolbarItem` объекты в основном меню iOS и Android:

![Снимок экрана "первичное меню Тулбаритем для Android и iOS"](toolbaritem-images/toolbaritem-primary-menu.png "Снимок экрана основного меню Тулбаритем на устройствах Android и iOS")

Если `Order` свойство имеет значение `Secondary` , поведение зависит от разных платформ. В UWP и Android `Secondary` меню Items (элементы) отображается в виде трех точек, которые можно коснуться или щелкнуть для отображения элементов в вертикальном списке. В iOS `Secondary` меню Items (элементы) отображается под панелью навигации в виде горизонтального списка. На следующих снимках экрана показано дополнительное меню в iOS и Android:

![Снимок экрана "Тулбаритем-дополнительное меню" для Android и iOS "](toolbaritem-images/toolbaritem-secondary-menu.png "Снимок экрана дополнительного меню Тулбаритем в Android и iOS")

> [!WARNING]
> Поведение значка в `ToolbarItem` объектах, `Order` для которых задано свойство, `Secondary` не согласовано на разных платформах. Не устанавливайте `IconImageSource` свойство для элементов, отображаемых во вторичном меню.

## <a name="related-links"></a>Связанные ссылки

* [Демонстрации Тулбаритем](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-toolbaritem/)
* [Изображения вXamarin.Forms](~/xamarin-forms/user-interface/images.md)
* [Xamarin.FormsMenuItem](~/xamarin-forms/user-interface/menuitem.md)
