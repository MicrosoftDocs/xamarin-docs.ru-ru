---
title: Привязка источников видео к проигрывателю
description: В этой статье объясняется, как привязать источники видео к видеопроигрывателю с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c02764d6160bb8ce2481a3cf67e357933d25934f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84133735"
---
# <a name="binding-video-sources-to-the-player"></a>Привязка источников видео к проигрывателю

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

Когда свойству `Source` представления `VideoPlayer` присваивается новый видеофайл, воспроизведение текущего видео останавливается и начинает воспроизводиться новое видео. Это демонстрируется на странице **Select Web Video** (Выбор видео из Интернета) в примере [**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos). На ней есть представление `ListView` с названиями трех видеороликов, на которые указывают ссылки в файле **App.xaml**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

При выборе видео выполняется обработчик событий `ItemSelected` в файле кода программной части. Обработчик удаляет все пробелы и апострофы из названия и использует итоговую строку как ключ для получения одного из ресурсов, определенных в файле **App.xaml**. После этого объект `UriVideoSource` присваивается свойству `Source` представления `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

При первой загрузке страницы элемент в `ListView` не выбран. Чтобы видео начало воспроизводиться, необходимо выбрать один из элементов:

[![Выбор видео из Интернета](source-bindings-images/selectwebvideo-small.png "Выбор видео из Интернета")](source-bindings-images/selectwebvideo-large.png#lightbox "Выбор видео из Интернета")

Свойство `Source` представления `VideoPlayer` поддерживается привязываемым свойством, то есть может быть целью привязки данных. Это демонстрируется на странице **Bind to VideoPlayer** (Привязка к VideoPlayer). Разметка в файле **BindToVideoPlayer.xaml** поддерживается следующим классом, который инкапсулирует название видео и соответствующий объект `VideoSource`:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

Элемент `ListView` в файле **BindToVideoPlayer.xaml** содержит массив объектов `VideoInfo`, каждый из которых инициализируется с использованием названия видео и объекта `UriVideoSource` из словаря ресурсов в файле **App.xaml**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

Свойство `Source` представления `VideoPlayer` привязано к `ListView`. Свойство `Path` привязки задается как `SelectedItem.VideoSource`. Это составная строка, включающая два свойства: `SelectedItem` — это свойства элемента `ListView`. Выбранный элемент имеет тип `VideoInfo`, у которого есть свойство `VideoSource`.

Так же как и в случае с первой страницей **Select Web Video**, элемент в `ListView` изначально не выбран. Поэтому, чтобы видео начало воспроизводиться, нужно выбрать один из видеороликов.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии видеопроигрывателя (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
