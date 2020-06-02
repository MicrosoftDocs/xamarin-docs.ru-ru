---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 11. The Bindable infrastructure''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: edc3dfd97457fe93a04edd82574f6ed419f5fdc1
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136803"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>Сводная информация о Главе 11. Инфраструктура c возможностью привязки

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

Каждый программист, работавший с C#, знает концепцию *свойств*. Свойства в C# содержат методы доступа для *присвоения* и (или) *получения* значения. Они часто называются *свойствами среды CLR* (общеязыковой среды выполнения).

Xamarin.Forms использует расширенное определение свойства, именуемое *привязываемым свойством*, которое инкапсулировано классом [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) и поддерживается классом [`BindableObject`](xref:Xamarin.Forms.BindableObject). Эти классы связаны, но во многом отличаются. `BindableProperty` используется для определения самого свойства, а `BindableObject` похож на `object`, так как является базовым классом для классов, определяющих привязываемые свойства.

## <a name="the-xamarinforms-class-hierarchy"></a>Иерархия классов Xamarin.Forms

В примере [**ClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) с помощью отражения демонстрируются иерархия классов Xamarin.Forms и решающая роль, которую в этой иерархии играет `BindableObject`. `BindableObject` является производным от `Object` и родительским классом для [`Element`](xref:Xamarin.Forms.Element), от которого, в свою очередь, наследуется [`VisualElement`](xref:Xamarin.Forms.VisualElement). Это родительский класс для [`Page`](xref:Xamarin.Forms.Page) и [`View`](xref:Xamarin.Forms.View), который является родительским классом для [`Layout`](xref:Xamarin.Forms.Layout):

[![Три снимка экрана с описанием иерархии классов](images/ch11fg01-small.png "Совместная иерархия классов")](images/ch11fg01-large.png#lightbox "Совместная иерархия классов")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>Коротко о BindableObject и BindableProperty

В классах, производных от `BindableObject`, многие свойства CLR "подкрепляются" аналогичными привязываемыми свойствами. Например, свойство [`Text`](xref:Xamarin.Forms.Label.Text) класса `Label` является свойством CLR, но класс `Label` определяет также открытое статическое поле только для чтения с именем [`TextProperty`](xref:Xamarin.Forms.Label.TextProperty) и типом `BindableProperty`.

Приложение может задать или получить свойство `Text` из `Label` обычным образом или задать `Text` вызовом метода [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)), который определен в `BindableObject` с аргументом `Label.TextProperty`. Аналогичным образом, приложение может получить значение свойства `Text` вызовом метода [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) с аргументом `Label.TextProperty`. Это демонстрируется в примере [**PropertySettings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings).

Действительно, свойство CLR `Text` полностью реализуется с помощью методов `SetValue` и `GetValue`, определенных `BindableObject` в сочетании со статическим свойством `Label.TextProperty`.

`BindableObject` и `BindableProperty` предоставляют поддержку следующих возможностей.

- Присвоение свойствам значений по умолчанию.
- Сохранение текущих значений.
- Предоставление механизмов для проверки значений свойств.
- Поддержание согласованности между связанными свойствами в одном классе.
- Реагирование на изменения свойств.
- Активация уведомлений перед изменением или после изменения свойства.
- Поддержка привязки данных.
- Поддержка стилей.
- Поддержка динамических ресурсов.

При изменении свойства, которое подкреплено привязываемым свойством, `BindableObject` запускает событие [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged), идентифицирующее измененное свойство. Если свойству присваивается такое же значение, как и прежнее, это событие не запускается.

Некоторые свойства не подкрепляются привязываемыми свойствами, а некоторые классы Xamarin.Forms в &mdash;, например `Span` &mdash;, не являются производными от `BindableObject`. Привязываемые свойства может поддерживать только класс, производный от `BindableObject`, так как методы `SetValue` и `GetValue` определяются в `BindableObject`.

Так как `Span` не является производным от `BindableObject`, ни одно из его свойств &mdash;, например `Text` &mdash;, не подкрепляется привязываемым свойством. Именно поэтому параметр `DynamicResource` в свойстве `Text` класса `Span` вызывает исключение в примере [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) из предыдущей главы. Пример [**DynamicVsStaticCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) демонстрирует, как задать динамические ресурсы в коде с помощью метода [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)), который определен в `Element`. Первый аргумент является объектом типа `BindableProperty`.

Аналогичным образом, у определенного в `BindableObject` метода [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) первый аргумент имеет тип `BindableProperty`.

## <a name="defining-bindable-properties"></a>Определение привязываемых свойств

Чтобы определить собственные привязываемые свойства, вызовите статический метод [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) для создания статического поля только для чтения с типом `BindableProperty`.

Это действие демонстрируется в классе [`AltLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) из библиотеки [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit). Класс является производным от `Label` и позволяет указать размер шрифта в пунктах. Применение этого класса демонстрируется в примере [**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText).

Для метода `BindableProperty.Create` обязательными являются четыре аргумента:

- `propertyName`: текстовое имя свойства (то же, что имя свойства CLR);
- `returnType`: тип свойства CLR;
- `declaringType`: тип класса, в котором объявлено свойство;
- `defaultValue`: значение свойства по умолчанию.

Поскольку `defaultValue` имеет тип `object`, компилятор должен уметь определять тип значения по умолчанию. Например, если `returnType` является `double`, для `defaultValue` нужно задать значение 0,0, а не просто 0, иначе во время выполнения будет вызвано исключение несоответствия типа.

Также достаточно часто привязываемое свойство включает следующее:

- `propertyChanged`: статический метод, который вызывается при изменении значения свойства. Первым аргументом здесь является экземпляр класса, у которого изменилось свойство.

Другие аргументы `BindableProperty.Create` менее широко распространены:

- `defaultBindingMode`: используется в сочетании с привязкой данных (как описано в Главе 16 [ **Привязка данных**](chapter16.md));
- `validateValue`: обратный вызов для проверки допустимого значения;
- `propertyChanging`: обратный вызов, который обозначает, что свойство должно быть изменено;
- `coerceValue`: обратный вызов, который изменяет заданное значение на другое значение;
- `defaultValueCreate`: обратный вызов для создания значения по умолчанию, которое нельзя использовать совместно для нескольких экземпляров класса (например, в коллекции).

### <a name="the-read-only-bindable-property"></a>Привязываемое свойство только для чтения

Привязываемое свойство может быть доступно только для чтения. Чтобы создать привязываемое свойство только для чтения, вызовите статический метод [`BindableProperty.CreateReadOnly`](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) для определения частного статического поля только для чтения с типом [`BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey).

Затем определите метод доступа `set` для свойства CLR как `private` для вызова перегрузки [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) с объектом `BindablePropertyKey`. Это не позволит задавать значение свойства за пределами самого класса.

Пример такого подхода демонстрируется в классе [`CountedLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs), который используется в примере [**BaskervillesCount**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount).

## <a name="related-links"></a>Связанные ссылки

- [Глава 11, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [Примеры для Главы 11](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md)
