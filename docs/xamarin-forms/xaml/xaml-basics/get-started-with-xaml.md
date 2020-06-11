---
Title: "часть 1. Начало работы с XAML "Description:" в Xamarin.Forms приложении XAML в основном используется для определения визуального содержимого страницы и работает вместе с файлом кода программной части ".
MS. произв. Xamarin MS. AssetID: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 09/30/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-1-getting-started-with-xaml"></a>Часть 1. Начало работы с XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_В Xamarin.Forms приложении XAML в основном используется для определения визуального содержимого страницы и работает вместе с файлом кода программной части C#._

Файл кода программной части обеспечивает поддержку кода для разметки. Вместе эти два файла вносят вклад в новое определение класса, которое включает дочерние представления и инициализацию свойств. В файле XAML классы и свойства указываются с помощью XML-элементов и атрибутов, а также устанавливаются ссылки между разметкой и кодом.

## <a name="creating-the-solution"></a>Создание решения

Чтобы начать редактирование первого файла XAML, используйте Visual Studio или Visual Studio для Mac для создания нового Xamarin.Forms решения. (Выберите вкладку, соответствующую вашей среде.)

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

В Windows запустите Visual Studio 2019 и в окне "Пуск" щелкните **создать новый проект** , чтобы создать новый проект:

![Новое окно решения](get-started-with-xaml-images/win/new-solution-2019.png)

В окне **Создать проект** в раскрывающемся списке **Тип проекта** выберите **Мобильное приложение**, а затем выберите шаблон **Мобильное приложение (Xamarin.Forms)** и нажмите кнопку **Далее**:

![Окно нового проекта](get-started-with-xaml-images/win/new-project-2019.png)

В окне **Настройка нового проекта** задайте для **имени проекта имя** **ксамлсамплес** (или другое значение) и нажмите кнопку **создать** .

В диалоговом окне **Создание межплатформенного приложения** щелкните **пусто**и нажмите кнопку **ОК** :

![Диалоговое окно создания приложения](get-started-with-xaml-images/win/new-cross-platform-app.png)

В решении создаются четыре проекта: Библиотека **ксамлсамплес** .NET Standard, **ксамлсамплес. Android**, **ксамлсамплес. iOS**и универсальная платформа Windows решение **ксамлсамплес. UWP**.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac выберите в меню **файл > создать решение** . В диалоговом окне **Новый проект** выберите **многоплатформенное > приложение** в левой части и **пустое приложение форм** (*не* **приложение форм**) из списка шаблонов:

![Диалоговое окно создания проекта 1](get-started-with-xaml-images/mac/newprojectdialog1.png)

Нажмите кнопку **Далее**.

В следующем диалоговом окне Присвойте проекту имя **ксамлсамплес** (или другое значение). Убедитесь, что выбран переключатель **использовать .NET Standard** :

![Диалоговое окно создания проекта 2](get-started-with-xaml-images/mac/newprojectdialog2.png)

Нажмите кнопку **Далее**.

В следующем диалоговом окне можно выбрать расположение для проекта:

![Диалоговое окно создания проекта 3](get-started-with-xaml-images/mac/newprojectdialog3.png)

Нажмите кнопку **создать** .

В решении создаются три проекта: **ксамлсамплес** .NET Standard Library, **ксамлсамплес. Android**и **ксамлсамплес. iOS**.

-----

После создания решения **ксамлсамплес** может потребоваться протестировать среду разработки, выбрав различные проекты платформы в качестве запускаемого проекта решения, а также создав и развернув простое приложение, созданное с помощью шаблона проекта, на эмуляторах телефонов или на реальных устройствах.

Если вам не нужно писать код для конкретной платформы, общий проект библиотеки **ксамлсамплес** .NET Standard — это то, что вы будете тратить практически на все время программирования. Эти статьи не будут сонаходиться за пределами этого проекта.

### <a name="anatomy-of-a-xaml-file"></a>Анатомия файла XAML

В библиотеке .NET Standard **ксамлсамплес** — это пара файлов со следующими именами:

- Файл **app. XAML**, XAML; перетаскивани
- **App.XAML.CS**— файл *кода программной части* C#, связанный с файлом XAML.

Чтобы увидеть файл кода программной части, необходимо щелкнуть стрелку рядом с файлом **app. XAML** .

Как **app. XAML** , так и **app.XAML.CS** вносят вклад в класс с именем `App` , производным от `Application` . Большинство других классов с файлами XAML вносят вклад в класс, производный от класса `ContentPage` ; эти файлы используют XAML для определения визуального содержимого целой страницы. Это справедливо для двух других файлов в проекте **ксамлсамплес** :

- **MainPage. XAML**, файл XAML; перетаскивани
- **MainPage.XAML.CS**, файл кода программной части C#.

Файл **MainPage. XAML** выглядит следующим образом (хотя форматирование может быть немного другим):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <StackLayout>
        <!-- Place new controls here -->
        <Label Text="Welcome to Xamarin Forms!"
               VerticalOptions="Center"
               HorizontalOptions="Center" />
    </StackLayout>

</ContentPage>
```

Два объявления пространства имен XML ( `xmlns` ) ссылаются на URI, первый — на веб-сайте Xamarin, а второй — в корпорации Майкрософт. Не пытайтесь проверить, что эти URI указывают. Нет ничего. Они представляют собой универсальные коды ресурсов (URI), принадлежащие Xamarin и Microsoft, и, по сути, работают как идентификаторы версий.

Первое объявление пространства имен XML означает, что теги, определенные в файле XAML без префикса, ссылаются на классы в Xamarin.Forms , например `ContentPage` . Второе объявление пространства имен определяет префикс `x` . Используется для нескольких элементов и атрибутов, которые являются встроенными для самого XAML и поддерживаются другими реализациями XAML. Однако эти элементы и атрибуты немного отличаются в зависимости от года, внедренного в URI. Xamarin.Formsподдерживает спецификацию XAML 2009, но не все.

`local`Объявление пространства имен позволяет получить доступ к другим классам из проекта библиотеки .NET Standard.

В конце первого тега `x` префикс используется для атрибута с именем `Class` . Поскольку использование этого `x` префикса является практически универсальным для пространства имен XAML, АТРИБУТЫ XAML, такие как `Class` , почти всегда называются `x:Class` .

`x:Class`Атрибут указывает полное имя класса .NET: `MainPage` класс в `XamlSamples` пространстве имен. Это означает, что этот XAML-файл определяет новый класс с именем `MainPage` в `XamlSamples` пространстве имен, производном от, `ContentPage` — тег, в котором `x:Class` отображается атрибут.

`x:Class`Атрибут может присутствовать только в корневом элементе файла XAML для определения производного класса C#. Это единственный новый класс, определенный в файле XAML. Все остальное, которые отображаются в XAML-файле, просто создаются из существующих классов и инициализируются.

Файл **MainPage.XAML.CS** выглядит следующим образом (помимо неиспользуемых `using` директив):

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage`Класс является производным от `ContentPage` , но обратите внимание на `partial` Определение класса. Это предполагает, что для существует еще одно определение разделяемого класса `MainPage` , но где это так? И что такое `InitializeComponent` метод?

Когда Visual Studio выполняет сборку проекта, он анализирует XAML-файл для создания файла кода C#. Если взглянуть на каталог **ксамлсамплес\ксамлсамплес\обж\дебуг** , вы найдете файл с именем **XamlSamples.MainPage.XAML.g.CS**. "G" означает, что создан. Это другое определение разделяемого класса `MainPage` , содержащее определение `InitializeComponent` метода, вызываемого из `MainPage` конструктора. Затем эти два `MainPage` определения разделяемых классов могут быть скомпилированы вместе. В зависимости от того, скомпилирован ли XAML-файл или нет, в исполняемый объект внедряется XAML-или двоичная форма файла XAML.

Во время выполнения код в конкретном проекте платформы вызывает `LoadApplication` метод, передавая ему новый экземпляр `App` класса в библиотеке .NET Standard. `App`Экземпляр конструктора класса `MainPage` . Конструктор этого класса вызывает `InitializeComponent` , который затем вызывает `LoadFromXaml` метод, извлекающий XAML-файл (или его скомпилированный двоичный файл) из библиотеки .NET Standard. `LoadFromXaml`Инициализирует все объекты, определенные в файле XAML, соединяет их вместе в связях типа «родители-потомки», прикрепляет обработчики событий, определенные в коде к событиям, заданным в файле XAML, и устанавливает результирующее дерево объектов в качестве содержимого страницы.

Хотя обычно не требуется тратить много времени на создание файлов кода, иногда исключения среды выполнения создаются для кода в созданных файлах, поэтому вы должны быть знакомы с ними.

При компиляции и запуске этой программы в `Label` центре страницы отображается элемент, как показано в XAML:

[![Отображение по умолчанию Xamarin.Forms](get-started-with-xaml-images/xamlsamples.png)](get-started-with-xaml-images/xamlsamples-large.png#lightbox)

Для более интересных визуальных элементов требуется только более интересный код XAML.

## <a name="adding-new-xaml-pages"></a>Добавление новых страниц XAML

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Чтобы добавить в проект другие классы на основе XAML `ContentPage` , выберите проект библиотеки **ксамлсамплес** .NET Standard, щелкните правой кнопкой мыши и выберите **Добавить > новый элемент...**. В диалоговом окне **Добавление нового элемента** выберите **элемент Visual C# элементы > Xamarin.Forms > страница содержимого** (не **Страница содержимого (C#)**, которая создает страницу только с кодом или **представление содержимого**, которое не является страницей). Присвойте странице имя, например **хеллоксамлпаже**:

![Диалоговое окно "Добавление нового элемента"](get-started-with-xaml-images/win/add-new-item-dialog-2019.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Чтобы добавить в проект другие классы на основе XAML `ContentPage` , выберите проект библиотеки **ксамлсамплес** .NET Standard и вызовите **файл > новый** элемент меню "файл". В левой части диалогового окна **новый файл** выберите **формы** слева и **Forms ContentPage XAML** (не **Forms ContentPage**, который создает страницу только кода или **представление содержимого**, которое не является страницей). Присвойте странице имя, например **хеллоксамлпаже**:

![Диалоговое окно "Новый файл"](get-started-with-xaml-images/mac/newfiledialog.png)

-----

В проект добавляются два файла: **хеллоксамлпаже. XAML** и файл кода программной части **HelloXamlPage.XAML.CS**.

## <a name="setting-page-content"></a>Задание содержимого страницы

Измените файл **хеллоксамлпаже. XAML** , чтобы только теги были доступны для `ContentPage` и `ContentPage.Content` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>

    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content`Теги являются частью уникального синтаксиса XAML. Сначала они могут показаться недействительными XML, но они являются допустимыми. Этот период не является специальным символом в XML.

`ContentPage.Content`Теги называются тегами *элементов свойств* . `Content`является свойством `ContentPage` , и обычно для него задано отдельное представление или макет с дочерними представлениями. Обычно свойства становятся атрибутами в XAML, но было бы трудно установить `Content` атрибут для сложного объекта. По этой причине свойство выражается в виде элемента XML, состоящего из имени класса и имени свойства, разделенного точкой. Теперь `Content` свойство можно задать между `ContentPage.Content` тегами следующим образом:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

Также обратите внимание, что `Title` для корневого тега задан атрибут.

В настоящее время связь между классами, свойствами и XML должна быть очевидной: Xamarin.Forms класс (например, `ContentPage` или `Label` ) отображается в файле XAML как элемент XML. Свойства этого класса, включая `Title` `ContentPage` и семь свойств, `Label` обычно отображаются как XML-атрибуты.

Для задания значений этих свойств существует множество сочетаний клавиш. Некоторые свойства являются базовыми типами данных: например, `Title` Свойства и `Text` имеют тип, `String` `Rotation` имеет тип `Double` и `IsVisible` (который `true` по умолчанию задан только для иллюстрации) имеет тип `Boolean` .

`HorizontalTextAlignment`Свойство имеет тип `TextAlignment` , который является перечислением. Для свойства любого типа перечисления необходимо указать только имя члена.

Однако для свойств более сложных типов используются преобразователи для синтаксического анализа XAML. Это классы в Xamarin.Forms , которые являются производными от `TypeConverter` . Многие являются открытыми классами, но некоторые — нет. Для этого конкретного XAML-файла некоторые из этих классов играют роль в фоновом режиме:

- `LayoutOptionsConverter`для `VerticalOptions` Свойства
- `FontSizeConverter`для `FontSize` Свойства
- `ColorTypeConverter`для `TextColor` Свойства

Эти преобразователи управляют допустимым синтаксисом параметров свойств.

`ThicknessTypeConverter`Может обработано одно, два или четыре числа, разделенные запятыми. Если указано одно число, оно применяется ко всем четырем сторонам. С двумя числами первым является левое и правое заполнение, а вторая — сверху и снизу. Четыре числа находятся в порядке слева, сверху, справа и снизу.

Объект `LayoutOptionsConverter` может преобразовать имена открытых статических полей `LayoutOptions` структуры в значения типа `LayoutOptions` .

`FontSizeConverter`Может выполнять обработку `NamedSize` элемента или размера числового шрифта.

`ColorTypeConverter`Компонент принимает имена открытых статических полей `Color` структуры или шестнадцатеричных значений RGB с альфа-каналом или без него, перед которым стоит знак решетки (#). Вот синтаксис без альфа-канала:

 `TextColor="#rrggbb"`

Каждая из маленьких букв представляет собой шестнадцатеричную цифру. Вот как включается альфа-канал:

 `TextColor="#aarrggbb">`

При альфа-канале следует помнить, что FF полностью непрозрачны, а 00 — полностью прозрачно.

Два других формата позволяют указать только одну шестнадцатеричную цифру для каждого канала:

 `TextColor="#rgb"` `TextColor="#argb"`

В этих случаях цифра повторяется для формирования значения. Например, #CF3 — цвет RGB CC-FF-33.

## <a name="page-navigation"></a>Переход по страницам

При запуске программы **ксамлсамплес** `MainPage` отображается. Чтобы увидеть новое значение `HelloXamlPage` , можно задать новую стартовую страницу в файле **app.XAML.CS** или переходить на новую страницу из `MainPage` .

Чтобы реализовать навигацию, сначала измените код в конструкторе **app.XAML.CS** , чтобы `NavigationPage` был создан объект:

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

В конструкторе **MainPage.XAML.CS** можно создать простой `Button` и использовать обработчик событий для перехода к `HelloXamlPage` :

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

Установка `Content` Свойства страницы заменяет значение `Content` свойства в файле XAML. При компиляции и развертывании новой версии этой программы на экране появляется кнопка. При нажатии клавиши выполняется переход к `HelloXamlPage` . Ниже приведена результирующая страница для iPhone, Android и UWP:

[![Текст повернутой метки](get-started-with-xaml-images/helloxaml1.png)](get-started-with-xaml-images/helloxaml1-large.png#lightbox)

Вернуться к `MainPage` использованию кнопки **< назад** в iOS можно с помощью стрелки влево в верхней части страницы или в нижней части телефона на устройстве Android или с помощью стрелки влево в верхней части страницы Windows 10.

Вы можете экспериментировать с XAML для различных способов визуализации `Label` . Если в текст необходимо встроить символы Юникода, можно использовать стандартный синтаксис XML. Например, чтобы вставить приветствие в парные кавычки, используйте:

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

Вот как это выглядит:

[![Повернутый текст метки с символами Юникода](get-started-with-xaml-images/helloxaml2.png)](get-started-with-xaml-images/helloxaml2-large.png#lightbox)

## <a name="xaml-and-code-interactions"></a>Взаимодействие XAML и кода

Пример **хеллоксамлпаже** содержит только один элемент `Label` на странице, но это очень нередкое. Большинство `ContentPage` производных типов задают `Content` свойство в макете некоторой сортировки, например `StackLayout` . `Children`Свойство объекта `StackLayout` определено как `IList<View>` имеющее тип, но фактически является объектом типа `ElementCollection<View>` , и эта коллекция может быть заполнена несколькими представлениями или другими макетами. В XAML эти связи типа «родители-потомки» устанавливаются с обычной XML-иерархией. Ниже приведен XAML-файл для новой страницы с именем **ксамлплускодепаже**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Этот XAML-файл является синтаксически завершенным и выглядит следующим образом:

[![Несколько элементов управления на странице](get-started-with-xaml-images/xamlpluscode1.png)](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox)

Однако, скорее всего, эта программа будет работать проблемных. Возможно, ожидается, что в `Slider` `Label` программе отображается текущее значение, и, вероятно, `Button` предполагалось что-то делать в рамках программы.

Как можно увидеть в [части 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). Задание отображения `Slider` значения с помощью `Label` можно полностью обработать в XAML с помощью привязки данных. Но рекомендуется сначала увидеть решение Code. Несмотря на то, что для обработки `Button` щелчка определенно требуется код. Это означает, что файл кода программной части для `XamlPlusCodePage` должен содержать обработчики для `ValueChanged` события `Slider` и `Clicked` события `Button` . Добавим их:

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

Эти обработчики событий не обязательно должны быть открытыми.

В файле XAML `Slider` `Button` теги и должны включать атрибуты для `ValueChanged` `Clicked` событий и, ссылающихся на эти обработчики:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что назначение обработчика для события имеет тот же синтаксис, что и присваивание значения свойству.

Если обработчик для `ValueChanged` события `Slider` будет использовать `Label` для вывода текущего значения, обработчику необходимо сослаться на этот объект из кода. Для `Label` требуется имя, указанное с помощью `x:Name` атрибута.

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x`Префикс `x:Name` атрибута указывает, что этот атрибут является встроенным для XAML.

Имя, присваиваемое `x:Name` атрибуту, имеет те же правила, что и имена переменных C#. Например, имя должно начинаться с буквы или знака подчеркивания и не должно содержать пробелов.

Теперь `ValueChanged` обработчик событий может установить, `Label` чтобы отобразить новое `Slider` значение. Новое значение доступно из аргументов события:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

Или обработчик может получить `Slider` объект, создающий это событие из `sender` аргумента, и получить `Value` свойство из этого:

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

При первом запуске программы `Label` не отображает это `Slider` значение, так как `ValueChanged` событие еще не запущено. Но при любой манипуляции со `Slider` значением отображается значение:

[![Отображаемое значение ползунка](get-started-with-xaml-images/xamlpluscode2.png)](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox)

Теперь для `Button` . Давайте имитируем ответ на `Clicked` событие, отображая предупреждение с помощью `Text` кнопки. Обработчик событий может безопасно привести `sender` аргумент к типу, `Button` а затем получить доступ к его свойствам:

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

Метод определяется так, как `async` `DisplayAlert` метод является асинхронным и должен начинаться с `await` оператора, который возвращает, когда метод завершается. Так как этот метод получает событие, которое запускается `Button` из `sender` аргумента, один и тот же обработчик можно использовать для нескольких кнопок.

Вы увидели, что объект, определенный в XAML, может инициировать событие, которое обрабатывается в файле кода программной части, и что файл кода программной части может обращаться к объекту, определенному в XAML, используя имя, присвоенное ему с `x:Name` атрибутом. Это два фундаментальных способа взаимодействия кода и XAML.

Некоторые дополнительные сведения о том, как работает XAML, можно получить, изучив только что созданный **файл XamlPlusCode.XAML.g.CS**, который теперь включает любое имя, присвоенное любому `x:Name` атрибуту в качестве частного поля. Ниже приведена упрощенная версия этого файла:

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

Объявление этого поля позволяет свободно использовать переменную в любом месте внутри `XamlPlusCodePage` файла разделяемого класса в вашей юрисдикции. Во время выполнения поле назначается после синтаксического анализа XAML. Это означает, что `valueLabel` `null` когда `XamlPlusCodePage` конструктор начинается, но является допустимым после вызова метода, это поле становится обязательным `InitializeComponent` .

После `InitializeComponent` возвращения управления обратно в конструктор визуальные элементы страницы были созданы так же, как если бы они были созданы и инициализированы в коде. Файл XAML больше не играет роли в классе. Вы можете манипулировать этими объектами на странице любым нужным образом, например, путем добавления представлений в `StackLayout` или `Content` для свойства страницы на что-то другое. Вы можете «пройти по дереву», изучив `Content` свойство страницы и элементы в `Children` коллекциях макетов. Можно задать свойства для представлений, доступ к которым осуществляется таким способом, или назначить обработчики событий динамически.

Можете бесплатно. Это страница, и XAML — это только средство для создания содержимого.

## <a name="summary"></a>Сводка

С помощью этого введения вы узнали, как файл XAML и файл кода вносят вклад в определение класса и как взаимодействуют файлы XAML и Code. Но XAML также имеет собственные уникальные синтаксические функции, позволяющие использовать его очень гибким способом. Вы можете приступить к изучению этих данных в [части 2. Важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md).

## <a name="related-links"></a>Связанные ссылки

- [ксамлсамплес](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Часть 2. Важный синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Часть 3. Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Часть 5. Из привязки данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
