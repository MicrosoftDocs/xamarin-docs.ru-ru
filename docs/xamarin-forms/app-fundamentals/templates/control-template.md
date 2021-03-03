---
title: Шаблоны элементов управления Xamarin.Forms
description: Шаблоны элементов управления Xamarin.Forms определяют визуальную структуру производных пользовательских элементов управления ContentView и производных страниц ContentPage.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ad65c46f216af9a24eb02ab55411f0a89ac61100
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374619"
---
# <a name="xamarinforms-control-templates"></a>Шаблоны элементов управления Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemos)

Шаблоны элементов управления Xamarin.Forms позволяют определить визуальную структуру производных пользовательских элементов управления [`ContentView`](xref:Xamarin.Forms.ContentView) и производных страниц [`ContentPage`](xref:Xamarin.Forms.ContentPage). Шаблоны элементов управления отделяют пользовательский интерфейс элемента управления или страницы от логики, которая их реализует. Вы можете добавить дополнительное содержимое в шаблонный пользовательский элемент управления или шаблонную страницу в заранее определенное расположение.

Например, можно создать шаблон элемента управления, который переопределяет пользовательский интерфейс, предоставленный пользовательским элементом управления. Шаблон элемента управления затем можно использовать в обязательном экземпляре пользовательского элемента управления. Кроме того, вы можете создать шаблон элемента управления, определяющий любой общий пользовательский интерфейс, который будет использоваться на нескольких страницах в приложении. Шаблон элемента управления затем можно использовать на нескольких страницах. Каждая страница будет по-прежнему отображать уникальное содержимое.

## <a name="create-a-controltemplate"></a>Создание шаблона ControlTemplate

В следующем примере показан код для пользовательского элемента управления `CardView`:

```csharp
public class CardView : ContentView
{
    public static readonly BindableProperty CardTitleProperty = BindableProperty.Create(nameof(CardTitle), typeof(string), typeof(CardView), string.Empty);
    public static readonly BindableProperty CardDescriptionProperty = BindableProperty.Create(nameof(CardDescription), typeof(string), typeof(CardView), string.Empty);
    // ...

    public string CardTitle
    {
        get => (string)GetValue(CardTitleProperty);
        set => SetValue(CardTitleProperty, value);
    }

    public string CardDescription
    {
        get => (string)GetValue(CardDescriptionProperty);
        set => SetValue(CardDescriptionProperty, value);
    }
    // ...
}
```

Класс `CardView`, производный от класса [`ContentView`](xref:Xamarin.Forms.ContentView), представляет пользовательский элемент управления, который отображает данные в карточном представлении макета. Класс содержит свойства, поддерживаемые привязываемыми свойствами, для отображаемых данных. Однако класс `CardView` не определяет никаких пользовательских интерфейсов. Вместо этого пользовательский интерфейс будет определен с помощью шаблона элемента управления. Дополнительные сведения о создании производных пользовательских элементов управления `ContentView` см. в статье [Xamarin.Forms ContentView](~/xamarin-forms/user-interface/layouts/contentview.md).

Создаваемый шаблон элемента управления имеет тип [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). При создании `ControlTemplate` вы объединяете объекты [`View`](xref:Xamarin.Forms.View), чтобы создать пользовательский интерфейс для пользовательского элемента управления или страницы. Шаблон `ControlTemplate` должен иметь только один объект `View` в качестве корневого элемента. Однако корневой элемент обычно содержит другие объекты `View`. Комбинация объектов составляет визуальную структуру элемента управления.

Хотя [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) можно определить в строке кода, обычно объект [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) объявляют как ресурс в словаре ресурсов. Так как шаблоны элементов управления являются ресурсами, для них действуют те же правила определения области, которые применяются ко всем ресурсам. Например, если объявить шаблон элемента управления в корневом элементе XAML-файла определения приложения, шаблон можно использовать в любом месте приложения. Если вы определяете шаблон на странице, шаблон элемента управления можно будет использовать только на ней. Дополнительные сведения о ресурсах см. в статье [Словари ресурсов Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md).

В следующем примере кода XAML показан шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) для объектов `CardView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
      <ControlTemplate x:Key="CardViewControlTemplate">
          <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                 BackgroundColor="{Binding CardColor}"
                 BorderColor="{Binding BorderColor}"
                 CornerRadius="5"
                 HasShadow="True"
                 Padding="8"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">
              <Grid>
                  <Grid.RowDefinitions>
                      <RowDefinition Height="75" />
                      <RowDefinition Height="4" />
                      <RowDefinition Height="Auto" />
                  </Grid.RowDefinitions>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="75" />
                      <ColumnDefinition Width="200" />
                  </Grid.ColumnDefinitions>
                  <Frame IsClippedToBounds="True"
                         BorderColor="{Binding BorderColor}"
                         BackgroundColor="{Binding IconBackgroundColor}"
                         CornerRadius="38"
                         HeightRequest="60"
                         WidthRequest="60"
                         HorizontalOptions="Center"
                         VerticalOptions="Center">
                      <Image Source="{Binding IconImageSource}"
                             Margin="-20"
                             WidthRequest="100"
                             HeightRequest="100"
                             Aspect="AspectFill" />
                  </Frame>
                  <Label Grid.Column="1"
                         Text="{Binding CardTitle}"
                         FontAttributes="Bold"
                         FontSize="Large"
                         VerticalTextAlignment="Center"
                         HorizontalTextAlignment="Start" />
                  <BoxView Grid.Row="1"
                           Grid.ColumnSpan="2"
                           BackgroundColor="{Binding BorderColor}"
                           HeightRequest="2"
                           HorizontalOptions="Fill" />
                  <Label Grid.Row="2"
                         Grid.ColumnSpan="2"
                         Text="{Binding CardDescription}"
                         VerticalTextAlignment="Start"
                         VerticalOptions="Fill"
                         HorizontalOptions="Fill" />
              </Grid>
          </Frame>
      </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Если шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) объявлен как ресурс, он должен иметь ключ, заданный с помощью атрибута `x:Key`, чтобы его можно было определить в словаре ресурсов. В этом примере корневым элементом шаблона `CardViewControlTemplate` является объект [`Frame`](xref:Xamarin.Forms.Frame). Объект `Frame` использует расширение разметки `RelativeSource`, чтобы установить для `BindingContext` значение экземпляра объекта среды выполнения, к которому применяется *шаблонный родительский элемент*. В объекте `Frame` используется сочетание объектов [`Grid`](xref:Xamarin.Forms.Grid), `Frame`, [`Image`](xref:Xamarin.Forms.Image), [`Label`](xref:Xamarin.Forms.Label) и [`BoxView`](xref:Xamarin.Forms.BoxView), чтобы определить визуальную структуру объекта `CardView`. Выражения привязки этих объектов разрешаются с учетом свойств `CardView` из-за наследования `BindingContext` от корневого элемента `Frame`. Дополнительные сведения о расширении разметки `RelativeSource` см. в статье [Относительные привязки Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

## <a name="consume-a-controltemplate"></a>Использование шаблона ControlTemplate

Шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) можно применить к производному пользовательскому элементу управления [`ContentView`](xref:Xamarin.Forms.ContentView), присвоив свойству [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) значение объекта шаблона элемента управления. Аналогичным образом, шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) можно применить к производной странице [`ContentPage`](xref:Xamarin.Forms.ContentPage), присвоив свойству [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate) значение объекта шаблона элемента управления. Когда во время выполнения применяется шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate), все элементы управления, определенные в `ControlTemplate`, добавляются к визуальному дереву шаблонного пользовательского элемента управления или шаблонной страницы.

В приведенном ниже примере показан шаблон `CardViewControlTemplate`, присваиваемый свойству [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) каждого объекта `CardView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"
                           ControlTemplate="{StaticResource CardViewControlTemplate}" />
    </StackLayout>
</ContentPage>
```

В этом примере элементы управления шаблона `CardViewControlTemplate` становятся частью визуального дерева каждого объекта `CardView`. Так как корневой объект [`Frame`](xref:Xamarin.Forms.Frame) шаблона элемента управления присваивает значение `BindingContext` шаблонному родительскому элементу, `Frame` и его дочерние элементы разрешают свои выражения привязки с учетом свойств каждого объекта `CardView`.

На приведенных ниже снимках экрана показан шаблон `CardViewControlTemplate`, применяемый к трем объектам `CardView`:

[![Снимки экрана шаблонных объектов CardView в iOS и Android](control-template-images/relativesource-controltemplate.png "Шаблонные объекты CardView")](control-template-images/relativesource-controltemplate-large.png#lightbox "Шаблонные объекты CardView")

> [!IMPORTANT]
> Момент времени, когда шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) применяется к экземпляру элемента управления, можно определить путем переопределения метода `OnApplyTemplate` в шаблонном пользовательском элементе управления или шаблонной странице. Дополнительные сведения см. в разделе [Извлечение именованного элемента из шаблона](#get-a-named-element-from-a-template).

## <a name="pass-parameters-with-templatebinding"></a>Передача параметров с помощью TemplateBinding

Расширение разметки `TemplateBinding` привязывает свойство элемента из шаблона [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) к общему свойству, которое определено шаблонным пользовательским элементом управления или шаблонной страницей. Благодаря использованию расширения `TemplateBinding` свойства элемента управления могут действовать в качестве параметров шаблона. Таким образом, если присвоено свойство шаблонного пользовательского элемента управления или шаблонной страницы, значение передается в элемент с расширением разметки `TemplateBinding`.

> [!IMPORTANT]
> Выражение разметки `TemplateBinding` позволяет удалить привязку `RelativeSource` из предыдущего шаблона элемента управления и заменяет выражения `Binding`.

Расширение разметки `TemplateBinding` определяет следующие свойства:

- `Path` с типом `string` — путь к свойству.
- `Mode` с типом `BindingMode` — направление распространения изменений между *источником* и *целью*.
- `Converter` с типом `IValueConverter` — преобразователь значений привязки.
- `ConverterParameter` с типом `object` — параметр для преобразователя значений привязки.
- `StringFormat` с типом `string` — формат строки для привязки.

`Path` — это свойство `ContentProperty` для расширения разметки `TemplateBinding`. Если путь является первым элементом в выражении `TemplateBinding`, часть Path= расширения разметки можно опустить. Дополнительные сведения об использовании этих свойств в выражении привязки см. в статье [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

> [!WARNING]
> Расширение разметки `TemplateBinding` следует использовать только в шаблоне [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Однако попытка использовать выражение `TemplateBinding` за пределами шаблона `ControlTemplate` не приведет к ошибке сборки или возникновению исключения.

В следующем примере кода XAML показан шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) для объектов `CardView`, использующих расширение разметки `TemplateBinding`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            <Frame BackgroundColor="{TemplateBinding CardColor}"
                   BorderColor="{TemplateBinding BorderColor}"
                   CornerRadius="5"
                   HasShadow="True"
                   Padding="8"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="75" />
                        <RowDefinition Height="4" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="75" />
                        <ColumnDefinition Width="200" />
                    </Grid.ColumnDefinitions>
                    <Frame IsClippedToBounds="True"
                           BorderColor="{TemplateBinding BorderColor}"
                           BackgroundColor="{TemplateBinding IconBackgroundColor}"
                           CornerRadius="38"
                           HeightRequest="60"
                           WidthRequest="60"
                           HorizontalOptions="Center"
                           VerticalOptions="Center">
                        <Image Source="{TemplateBinding IconImageSource}"
                               Margin="-20"
                               WidthRequest="100"
                               HeightRequest="100"
                               Aspect="AspectFill" />
                    </Frame>
                    <Label Grid.Column="1"
                           Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold"
                           FontSize="Large"
                           VerticalTextAlignment="Center"
                           HorizontalTextAlignment="Start" />
                    <BoxView Grid.Row="1"
                             Grid.ColumnSpan="2"
                             BackgroundColor="{TemplateBinding BorderColor}"
                             HeightRequest="2"
                             HorizontalOptions="Fill" />
                    <Label Grid.Row="2"
                           Grid.ColumnSpan="2"
                           Text="{TemplateBinding CardDescription}"
                           VerticalTextAlignment="Start"
                           VerticalOptions="Fill"
                           HorizontalOptions="Fill" />
                </Grid>
            </Frame>
        </ControlTemplate>
    </ContentPage.Resources>
    ...
</ContentPage>
```

В этом примере расширение разметки `TemplateBinding` разрешает выражения привязки для свойств каждого объекта `CardView`. На приведенных ниже снимках экрана показан шаблон `CardViewControlTemplate`, применяемый к трем объектам `CardView`:

[![Снимки экрана шаблонных объектов CardView](control-template-images/templatebinding-controltemplate.png "Шаблонные объекты CardView")](control-template-images/templatebinding-controltemplate-large.png#lightbox "Шаблонные объекты CardView")

> [!IMPORTANT]
> Использование расширения разметки `TemplateBinding` эквивалентно присвоению для свойства `BindingContext` корневого элемента шаблона значения шаблонного родительского элемента с помощью расширения разметки `RelativeSource` и разрешению привязки дочерних объектов с помощью расширения разметки `Binding`. На самом деле расширение разметки `TemplateBinding` создает `Binding` с `Source` в качестве `RelativeBindingSource.TemplatedParent`.

## <a name="apply-a-controltemplate-with-a-style"></a>Применение шаблона ControlTemplate со стилем

Шаблоны элементов управления также можно применить со стилями. Для этого необходимо создать *неявный* или *явный* стиль, в котором используется [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate).

В следующем примере кода XAML показан *неявный* стиль, в котором используется `CardViewControlTemplate`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewControlTemplate">
            ...
        </ControlTemplate>

        <Style TargetType="controls:CardView">
            <Setter Property="ControlTemplate"
                    Value="{StaticResource CardViewControlTemplate}" />
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="John Doe"
                           CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Jane Doe"
                           CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png"/>
        <controls:CardView BorderColor="DarkGray"
                           CardTitle="Xamarin Monkey"
                           CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                           IconBackgroundColor="SlateGray"
                           IconImageSource="user.png" />
    </StackLayout>
</ContentPage>
```

В этом примере *неявный* [`Style`](xref:Xamarin.Forms.Style) автоматически применяется к каждому объекту `CardView` и устанавливает для свойства [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) каждого объекта `CardView` значение `CardViewControlTemplate`.

Дополнительные сведения о стилях см. в статье [Стили Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md).

## <a name="redefine-a-controls-ui"></a>Переопределение пользовательского интерфейса элемента управления

Когда шаблон [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) создается и присваивается свойству `ControlTemplate` производного пользовательского элемента управления [`ContentView`](xref:Xamarin.Forms.ContentView) или производной страницы [`ContentPage`](xref:Xamarin.Forms.ContentPage), визуальная структура, определенная для пользовательского элемента управления или страницы, заменяется визуальной структурой, определенной в `ControlTemplate`.

Например, пользовательский элемент управления `CardViewUI` определяет пользовательский интерфейс, используя следующий код XAML:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ControlTemplateDemos.Controls.CardViewUI"
             x:Name="this">
    <Frame BindingContext="{x:Reference this}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="75" />
                <ColumnDefinition Width="200" />
            </Grid.ColumnDefinitions>
            <Frame IsClippedToBounds="True"
                   BorderColor="{Binding BorderColor, FallbackValue='Black'}"
                   BackgroundColor="{Binding IconBackgroundColor, FallbackValue='Gray'}"
                   CornerRadius="38"
                   HeightRequest="60"
                   WidthRequest="60"
                   HorizontalOptions="Center"
                   VerticalOptions="Center">
                <Image Source="{Binding IconImageSource}"
                       Margin="-20"
                       WidthRequest="100"
                       HeightRequest="100"
                       Aspect="AspectFill" />
            </Frame>
            <Label Grid.Column="1"
                   Text="{Binding CardTitle, FallbackValue='Card title'}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     Grid.ColumnSpan="2"
                     BackgroundColor="{Binding BorderColor, FallbackValue='Black'}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Grid.ColumnSpan="2"
                   Text="{Binding CardDescription, FallbackValue='Card description'}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
        </Grid>
    </Frame>
</ContentView>
```

Однако элементы управления, которые входят в пользовательский интерфейс, можно заменить. Для этого необходимо определить новую визуальную структуру в [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) и присвоить ее свойству [`ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) объекта `CardViewUI`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="CardViewCompressed">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="100" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="100" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="{TemplateBinding IconImageSource}"
                        BackgroundColor="{TemplateBinding IconBackgroundColor}"
                        WidthRequest="100"
                        HeightRequest="100"
                        Aspect="AspectFill"
                        HorizontalOptions="Center"
                        VerticalOptions="Center" />
                <StackLayout Grid.Column="1">
                    <Label Text="{TemplateBinding CardTitle}"
                           FontAttributes="Bold" />
                    <Label Text="{TemplateBinding CardDescription}" />
                </StackLayout>
            </Grid>
        </ControlTemplate>
    </ContentPage.Resources>
    <StackLayout Margin="30">
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="John Doe"
                             CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Jane Doe"
                             CardDescription="Phasellus eu convallis mi. In tempus augue eu dignissim fermentum. Morbi ut lacus vitae eros lacinia."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
        <controls:CardViewUI BorderColor="DarkGray"
                             CardTitle="Xamarin Monkey"
                             CardDescription="Aliquam sagittis, odio lacinia fermentum dictum, mi erat scelerisque erat, quis aliquet arcu."
                             IconBackgroundColor="SlateGray"
                             IconImageSource="user.png"
                             ControlTemplate="{StaticResource CardViewCompressed}" />
    </StackLayout>
</ContentPage>
```

В этом примере визуальная структура объекта `CardViewUI` переопределяется в шаблоне [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate), который предоставляет более компактную визуальную структуру, пригодную для сокращенного списка:

[![Снимки экрана шаблонных объектов CardViewUI в iOS и Android](control-template-images/redefine-controltemplate.png "Шаблонные объекты CardViewUI")](control-template-images/redefine-controltemplate-large.png#lightbox "Шаблонные объекты CardViewUI")

## <a name="substitute-content-into-a-contentpresenter"></a>Преобразование содержимого в ContentPresenter

[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) можно поместить в шаблон элемента управления, чтобы указать, где будет отображаться содержимое шаблонного пользовательского элемента управления или шаблонной страницы. Затем пользовательский элемент управления или пользовательская страница, которые используют шаблон элемента управления, определяют содержимое, отображаемое объектом `ContentPresenter`. На следующей схеме показан шаблон `ControlTemplate` для страницы, содержащий несколько элементов управления, в том числе элемент `ContentPresenter`, обозначенный синим прямоугольником:

![Шаблон элемента управления для ContentPage](control-template-images/control-template.png "Шаблон элемента управления для ContentPage")

В следующем коде XAML показан шаблон элемента управления `TealTemplate`, содержащий [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) в своей визуальной структуре:

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="0.1*" />
            <RowDefinition Height="0.8*" />
            <RowDefinition Height="0.1*" />
        </Grid.RowDefinitions>
        <BoxView Color="Teal" />
        <Label Margin="20,0,0,0"
               Text="{TemplateBinding HeaderText}"
               TextColor="White"
               FontSize="Title"
               VerticalOptions="Center" />
        <ContentPresenter Grid.Row="1" />
        <BoxView Grid.Row="2"
                 Color="Teal" />
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        <controls:HyperlinkLabel Grid.Row="2"
                                 Margin="0,0,20,0"
                                 Text="Help"
                                 TextColor="White"
                                 Url="https://docs.microsoft.com/xamarin/xamarin-forms/"
                                 HorizontalOptions="End"
                                 VerticalOptions="Center" />
    </Grid>
</ControlTemplate>
```

В следующем примере показан шаблон `TealTemplate`, назначенный свойству [`ControlTemplate`](xref:Xamarin.Forms.TemplatedPage.ControlTemplate) производной страницы [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<controls:HeaderFooterPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"                           
                           ControlTemplate="{StaticResource TealTemplate}"
                           HeaderText="MyApp"
                           ...>
    <StackLayout Margin="10">
        <Entry Placeholder="Enter username" />
        <Entry Placeholder="Enter password"
               IsPassword="True" />
        <Button Text="Login" />
    </StackLayout>
</controls:HeaderFooterPage>
```

Когда во время выполнения шаблон `TealTemplate` применяется к странице, ее содержимое подставляется в класс [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter), определенный в шаблоне элемента управления:

[![Снимки экрана шаблонного объекта страницы в iOS и Android](control-template-images/tealtemplate-contentpage.png "Шаблонная страница ContentPage")](control-template-images/tealtemplate-contentpage-large.png#lightbox "Шаблонная страница ContentPage")

## <a name="get-a-named-element-from-a-template"></a>Извлечение именованного элемента из шаблона

Именованные элементы в шаблоне элемента управления можно извлечь из шаблонного пользовательского элемента управления или шаблонной страницы. Это можно сделать с помощью метода `GetTemplateChild`, который возвращает именованный элемент в созданном экземпляре визуального дерева [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate), если он найден. В противном случае она возвращает `null`.

Когда экземпляр шаблона элемента управления будет создан, вызовется метод шаблона `OnApplyTemplate`. Поэтому метод `GetTemplateChild` следует вызывать из переопределения `OnApplyTemplate` в шаблонном элементе управления или шаблонной странице.

> [!IMPORTANT]
> Метод `GetTemplateChild` должен вызываться только после вызова метода `OnApplyTemplate`.

В следующем коде XAML показан шаблон элемента управления `TealTemplate`, который можно применить к производным страницам [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<ControlTemplate x:Key="TealTemplate">
    <Grid>
        ...
        <Label x:Name="changeThemeLabel"
               Grid.Row="2"
               Margin="20,0,0,0"
               Text="Change Theme"
               TextColor="White"
               HorizontalOptions="Start"
               VerticalOptions="Center">
            <Label.GestureRecognizers>
                <TapGestureRecognizer Tapped="OnChangeThemeLabelTapped" />
            </Label.GestureRecognizers>
        </Label>
        ...
    </Grid>
</ControlTemplate>
```

В этом примере элемент [`Label`](xref:Xamarin.Forms.Label) именованный, поэтому его можно извлечь из кода шаблонной страницы. Для этого нужно вызвать метод `GetTemplateChild` из переопределения `OnApplyTemplate` для шаблонной страницы:

```csharp
public partial class AccessTemplateElementPage : HeaderFooterPage
{
    Label themeLabel;

    public AccessTemplateElementPage()
    {
        InitializeComponent();
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        themeLabel = (Label)GetTemplateChild("changeThemeLabel");
        themeLabel.Text = OriginalTemplate ? "Aqua Theme" : "Teal Theme";
    }
}
```

В этом примере объект [`Label`](xref:Xamarin.Forms.Label) с именем `changeThemeLabel` извлекается после создания экземпляра `ControlTemplate`. После этого объект `changeThemeLabel` станет доступным для использования классом `AccessTemplateElementPage`. На приведенных ниже снимках экрана показано, что текст, отображаемый объектом `Label`, изменен:

[![Снимки экрана шаблонного объекта страницы](control-template-images/get-named-element.png "Шаблонная страница ContentPage")](control-template-images/get-named-element-large.png#lightbox "Шаблонная страница ContentPage")

## <a name="bind-to-a-viewmodel"></a>Привязка к ViewModel

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) может выполнять привязку данных к ViewModel, даже если шаблон `ControlTemplate` привязывается к шаблонному родительскому элементу (экземпляру объекта среды выполнения, к которому применяется шаблон).

В следующем примере кода XAML показана страница, которая использует ViewModel с именем `PeopleViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ControlTemplateDemos"
             xmlns:controls="clr-namespace:ControlTemplateDemos.Controls"
             ...>
    <ContentPage.BindingContext>
        <local:PeopleViewModel />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <DataTemplate x:Key="PersonTemplate">
            <controls:CardView BorderColor="DarkGray"
                               CardTitle="{Binding Name}"
                               CardDescription="{Binding Description}"
                               ControlTemplate="{StaticResource CardViewControlTemplate}" />
        </DataTemplate>
    </ContentPage.Resources>

    <StackLayout Margin="10"
                 BindableLayout.ItemsSource="{Binding People}"
                 BindableLayout.ItemTemplate="{StaticResource PersonTemplate}" />
</ContentPage>
```

В этом примере элемент `BindingContext` страницы содержит экземпляр `PeopleViewModel`. ViewModel предоставляет коллекцию `People` и `ICommand` с именем `DeletePersonCommand`. Класс [`StackLayout`](xref:Xamarin.Forms.StackLayout) на странице использует привязываемый макет, чтобы привязать данные к коллекции `People`, а для ресурса `PersonTemplate` шаблон `ItemTemplate` привязываемого макета. Этот шаблон [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) указывает, что каждый элемент коллекции `People` будет отображаться с помощью объекта `CardView`. Визуальная структура объекта `CardView` определяется с помощью шаблона [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) с именем `CardViewControlTemplate`:

```xaml
<ControlTemplate x:Key="CardViewControlTemplate">
    <Frame BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
           BackgroundColor="{Binding CardColor}"
           BorderColor="{Binding BorderColor}"
           CornerRadius="5"
           HasShadow="True"
           Padding="8"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="75" />
                <RowDefinition Height="4" />
                <RowDefinition Height="Auto" />
            </Grid.RowDefinitions>
            <Label Text="{Binding CardTitle}"
                   FontAttributes="Bold"
                   FontSize="Large"
                   VerticalTextAlignment="Center"
                   HorizontalTextAlignment="Start" />
            <BoxView Grid.Row="1"
                     BackgroundColor="{Binding BorderColor}"
                     HeightRequest="2"
                     HorizontalOptions="Fill" />
            <Label Grid.Row="2"
                   Text="{Binding CardDescription}"
                   VerticalTextAlignment="Start"
                   VerticalOptions="Fill"
                   HorizontalOptions="Fill" />
            <Button Text="Delete"
                    Command="{Binding Source={RelativeSource AncestorType={x:Type local:PeopleViewModel}}, Path=DeletePersonCommand}"
                    CommandParameter="{Binding CardTitle}"
                    HorizontalOptions="End" />
        </Grid>
    </Frame>
</ControlTemplate>
```

В этом примере корневым элементом шаблона [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) является объект [`Frame`](xref:Xamarin.Forms.Frame). Объект `Frame` использует расширение разметки `RelativeSource`, чтобы задать для `BindingContext` значение шаблонного родительского элемента. Выражения привязки объекта `Frame` и его дочерних элементов разрешаются для свойств `CardView` из-за наследования `BindingContext` от корневого элемента `Frame`. На следующих снимках экрана показана страница с коллекцией `People`, состоящей из трех элементов:

[![Снимки экрана трех шаблонных объектов CardView](control-template-images/viewmodel-controltemplate.png "Шаблонные объекты CardView")](control-template-images/viewmodel-controltemplate-large.png#lightbox "Шаблонные объекты CardView")

Хотя объекты в шаблоне [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) привязываются к свойствам родительского элемента, [`Button`](xref:Xamarin.Forms.Button) в шаблоне элемента управления привязывается к шаблонному родительскому элементу и к `DeletePersonCommand` в объекте ViewModel. Это связано с тем, что свойство `Button.Command` переопределяет источник привязки как контекст привязки предка с типом `PeopleViewModel`, то есть [`StackLayout`](xref:Xamarin.Forms.StackLayout). Затем часть `Path` выражений привязки может разрешить свойство `DeletePersonCommand`. Однако свойство `Button.CommandParameter` не изменяет источник привязки, а наследует его от родительского элемента в [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). Таким образом, свойство `CommandParameter` привязывается к свойству `CardTitle` `CardView`.

Общим эффектом привязок [`Button`](xref:Xamarin.Forms.Button) является то, что при нажатии `Button` в классе `PeopleViewModel` выполняется `DeletePersonCommand` со значением свойства `CardName`, которое передается в `DeletePersonCommand`. В результате заданный объект `CardView` удаляется из привязываемого макета:

[![Снимки экрана двух шаблонных объектов CardView](control-template-images/viewmodel-itemdeleted.png "Шаблонные объекты CardView")](control-template-images/viewmodel-itemdeleted-large.png#lightbox "Шаблонные объекты CardView")

Дополнительные сведения об относительных привязках см. в статье [Относительные привязки Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md).

## <a name="related-links"></a>Связанные ссылки

- [ControlTemplateDemos (пример)](/samples/xamarin/xamarin-forms-samples/templates-controltemplatedemos)
- [Xamarin.Forms ContentView](~/xamarin-forms/user-interface/layouts/contentview.md)
- [Относительные привязки Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)
- [Словари ресурсов Xamarin.Forms](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Привязка данных Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Стили Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md)