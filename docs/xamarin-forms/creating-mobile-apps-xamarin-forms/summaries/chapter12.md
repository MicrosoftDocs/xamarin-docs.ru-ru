---
title: "Сводная информация о главе 12 (Стили)" description: "Создание мобильных приложений с помощью Xamarin.Forms: Сводная информация о главе 12 (Стили)" ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3 author: davidbritch ms.author: dabritch ms.date: 19.07.2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="summary-of-chapter-12-styles"></a>Сводная информация о главе 12. Стили

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

В Xamarin.Forms стили позволяют нескольким представлениям совместно использовать коллекцию параметров свойств. Это сокращает разметку и позволяет поддерживать единообразные визуальные темы.

Стили почти всегда определяются и используются в разметке. Объект типа [`Style`](xref:Xamarin.Forms.Style) создается в словаре ресурсов, а затем устанавливается в свойство визуального элемента [`Style`](xref:Xamarin.Forms.NavigableElement.Style) с помощью расширения разметки `StaticResource` или `DynamicResource`.

## <a name="the-basic-style"></a>Базовый стиль

Для `Style` требуется, чтобы для [`TargetType`](xref:Xamarin.Forms.Style.TargetType) был задан тип визуального объекта, к которому он применяется. При создании экземпляра `Style` в словаре ресурсов (как и обычно) для него также требуется атрибут `x:Key`.

`Style` имеет свойство содержимого типа [`Setters`](xref:Xamarin.Forms.Style.Setters), которое представляет собой коллекцию объектов [`Setter`](xref:Xamarin.Forms.Setter). Каждый `Setter` связывает [`Property`](xref:Xamarin.Forms.Setter.Property) с [`Value`](xref:Xamarin.Forms.Setter.Value).

В XAML параметр `Property` — это имя свойства CLR (например, свойство `Text` объекта `Button`), однако свойство со стилем должно быть включено в свойство, доступное для привязки. Кроме того, свойство должно быть определено в классе, указанном параметром `TargetType`, или унаследовано этим классом.

Параметр `Value` можно указать с помощью элемента свойства `<Setter.Value>`. Это позволяет задать `Value` для объекта, который не может быть выражен в текстовой строке, в объекте `OnPlatform` или в объекте, созданном с помощью `x:Arguments` или `x:FactoryMethod`. Свойство `Value` для другого элемента в словаре также можно задать выражением `StaticResource`.

Программа [**BasicStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) демонстрирует базовый синтаксис и показывает, как ссылаться на `Style` с помощью расширения разметки `StaticResource`:

[![Тройной снимок экрана базового стиля](images/ch12fg01-small.png "Базовые стили")](images/ch12fg01-large.png#lightbox "Базовые стили")

Объект `Style` и любой объект, созданный в объекте `Style` в качестве параметра `Value`, являются общими для всех представлений, ссылающихся на этот `Style`. `Style` не может содержать все, что нельзя использовать совместно, например производный `View`.

Обработчики событий не могут быть заданы в `Style`. Свойство `GestureRecognizers` нельзя задать в `Style`, поскольку оно не поддерживается привязываемым свойством.

## <a name="styles-in-code"></a>Стили в коде

Хотя это и не является распространенным, объекты `Style` можно создавать и инициализировать в коде. Это продемонстрировано в примере [**BasicStyleCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode).

## <a name="style-inheritance"></a>Наследование стилей

`Style` имеет свойство [`BasedOn`](xref:Xamarin.Forms.Style.BasedOn), которое можно задать для расширения разметки `StaticResource`, ссылающейся на другой стиль. Это позволяет стилям наследовать предыдущие стили, а также добавлять или заменять параметры свойств. Это показано в примере [**StyleInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance).

Если `Style2` основан на `Style1`, то `TargetType` из `Style2` должен быть таким же, как `Style1`, или производным от `Style1`. Словарь ресурсов, в котором хранится `Style1`, должен иметь тот же словарь ресурсов, что и `Style2`, или словарь ресурсов, находящийся выше в визуальном дереве.

## <a name="implicit-styles"></a>Неявные стили

Если `Style` в словаре ресурсов не содержит атрибут `x:Key`, ему автоматически присваивается ключ словаря, а объект `Style` становится *неявным стилем*. Представление без параметра `Style`, тип которого соответствует `TargetType`, точно будет находить этот стиль, как показывает пример [**ImplicitStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle).

Неявный стиль может быть производным от `Style` с параметром `x:Key`, но не наоборот. Нельзя явно ссылаться на неявный стиль.

Вы можете реализовать три типа иерархии со стилями и `BasedOn`:

- Из стилей, определенных в `Application` и `Page`, к стилям, определенным в макетах ниже в визуальном дереве.
- Из стилей, определенных для базовых классов, таких как `VisualElement` и `View`, к стилям, определенным для конкретных классов.
- Из стилей с явными ключами словаря к неявным стилям.

Эти иерархии демонстрируются в примере [**StyleHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy).

## <a name="dynamic-styles"></a>Динамические стили

На стиль в ресурсном словаре можно ссылаться не на `StaticResource`, а на `DynamicResource`. Это делает стиль *динамическим стилем*. Если этот стиль заменяется в словаре ресурсов другим стилем с тем же ключом, представления, ссылающиеся на этот стиль `DynamicResource`, автоматически меняются. Кроме того, отсутствие записи словаря с указанным ключом приведет к тому, что `StaticResource` вызовет исключение, но не `DynamicResource`.

Эту методику можно использовать для динамического изменения стилей или тем, как показывает пример [**DynamicStyles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles).

Однако для свойства `BasedOn` нельзя задать расширение составных частей `DynamicResource`, поскольку `BasedOn` не поддерживается связываемыми свойствами. Чтобы сделать стиль динамически производным, не устанавливайте `BasedOn`. Вместо этого присвойте свойству [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) ключ словаря стиля, от которого требуется выполнить наследование. Этот метод демонстрируется в примере [**DynamicStylesInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh).

## <a name="device-styles"></a>Стили устройства

Вложенный класс [`Device.Styles`](xref:Xamarin.Forms.Device.Styles) определяет двенадцать статических полей, предназначенных только для чтения, для шести стилей с `TargetType` из `Label`, которые можно использовать для общих типов использования текста.

Шесть из этих полей имеют тип `Style`, который можно задать непосредственно для свойства `Style` в коде:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Другие шесть полей относятся к типу `string` и могут использоваться в качестве ключей словаря для динамических стилей:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) равно "BodyStyle"
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) равно "TitleStyle"
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) равно "SubtitleStyle"
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) равно "CaptionStyle"
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) равно "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) равно "ListItemDetailTextStyle"

Эти стили иллюстрируются в примере [**DeviceStylesList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList).

## <a name="related-links"></a>Связанные ссылки

- [Полный текст главы 12 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Примеры для главы 12](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
