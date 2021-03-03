---
title: Резервные значения привязки в Xamarin.Forms
description: В этой статье описывается, как повысить надежность привязок путем определения резервных значений, которые будут использоваться при сбое привязки.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d288d43bcee5719c1ab3667812e5bac4891bb188
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366325"
---
# <a name="xamarinforms-binding-fallbacks"></a>Резервные значения привязки в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](/samples/xamarin/xamarin-forms-samples/databindingdemos)

Иногда может происходить сбой привязки данных из-за того, что не удалось разрешить источник привязки, или из-за того, что успешно выполненная привязка возвращает значение `null`. Хотя в таких ситуациях можно применять преобразователи величин или другой дополнительный код, повысить надежность привязок можно путем определения резервных значений, которые используются в случае сбоя привязки. Для этого можно определить свойства [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) и [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) в выражении привязки. Так как эти свойства находятся в классе [`BindingBase`](xref:Xamarin.Forms.BindingBase), их можно использовать с обычными привязками, множественными привязками, скомпилированными привязками и расширением разметки `Binding`.

> [!NOTE]
> Использование свойств [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) и [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) в выражении привязки не является обязательным.

## <a name="defining-a-fallback-value"></a>Определение резервного значения

Свойство [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) позволяет определить резервное значение, которое будет использоваться, если невозможно разрешить *источник* привязки. Это свойство задается, например, при привязке к исходным свойствам, которые могут иметься не у всех объектов однородных типов в связанной коллекции.

На странице **MonkeyDetail** иллюстрируется задание свойства [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue):

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

Для привязки к [`Label`](xref:Xamarin.Forms.Label) определено значение [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue), которое будет присвоено целевому объекту, если не удастся разрешить источник привязки. Поэтому, если у привязанного объекта нет свойства `Population`, отображается значение, определенное в свойстве `FallbackValue`. Обратите внимание на то, что здесь значение свойства `FallbackValue` отделяется одинарными кавычками (апострофами).

Значения свойств [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) рекомендуется определять не в коде, а в качестве ресурсов а [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Преимущество такого подхода состоит в том, что такие значения определяются однократно в одном месте, где их легко найти. Ресурсы можно извлечь с помощью расширения разметки `StaticResource`:

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> Свойство `FallbackValue` невозможно задать с помощью выражения привязки.

Вот работающая программа:

![Привязка FallbackValue](binding-fallbacks-images/bindingunavailable-detail-cropped.png "Привязка FallbackValue")

Если свойство `FallbackValue` не задано в выражении привязки и не удалось разрешить путь привязки или его часть, для целевого объекта задается [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue). Однако если свойство `FallbackValue` задано, но не удается разрешить путь привязки или его часть, для целевого объекта задается значение свойства `FallbackValue`. Поэтому на странице **MonkeyDetail** в элементе [`Label`](xref:Xamarin.Forms.Label) отображается текст "Population size unknown" (Размер популяции неизвестен), так как у привязанного объекта нет свойства `Population`.

> [!IMPORTANT]
> Если задано свойство [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue), определенный преобразователь величин не выполняется в выражении привязки.

## <a name="defining-a-null-replacement-value"></a>Определение значения, заменяющего NULL

Свойство [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) позволяет определить заменяющее значение, которое будет использоваться, если *источник* привязки разрешается, но возвращается значение `null`. Это свойство задается, например, при привязке к исходным свойствам, которые могут иметь значение `null` в связанной коллекции.

На странице **Monkeys** иллюстрируется задание свойства [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue):

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Для привязок к [`Image`](xref:Xamarin.Forms.Image) и [`Label`](xref:Xamarin.Forms.Label) определены значения [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue), которые применяются, если путь привязки возвращает значение `null`. Поэтому значения свойств `TargetNullValue` отображаются для всех объектов в коллекции, для которых свойства `ImageUrl` и `Location` не определены. Обратите внимание на то, что здесь значения свойства `TargetNullValue` отделяются одинарными кавычками (апострофами).

Значения свойств [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) рекомендуется определять не в коде, а в качестве ресурсов а [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). Преимущество такого подхода состоит в том, что такие значения определяются однократно в одном месте, где их легко найти. Ресурсы можно извлечь с помощью расширения разметки `StaticResource`:

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> Свойство `TargetNullValue` невозможно задать с помощью выражения привязки.

Вот работающая программа:

[![Привязка TargetNullValue](binding-fallbacks-images/bindingunavailable-small.png "Привязка TargetNullValue")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "Привязка TargetNullValue")

Когда свойство `TargetNullValue` не указано в выражении привязки, исходное значение `null` преобразуется, если определен преобразователь величин, форматируется, если определен формат `StringFormat`, а затем результат задается для целевого объекта. Однако когда свойство `TargetNullValue` указано, исходное значение `null` преобразуется, если определен преобразователь величин, а затем, если оно по-прежнему равно `null` после преобразования, значение свойства `TargetNullValue` задается для целевого объекта.

> [!IMPORTANT]
> Если свойство `TargetNullValue` задано, форматирование строк в выражении привязки не применяется.

## <a name="related-links"></a>Связанные ссылки

- [Демоверсии привязок данных (пример)](/samples/xamarin/xamarin-forms-samples/databindingdemos)