---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 10. XAML markup extensions''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8f23034df684e778677e4f2e480e1c41807536fb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136816"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Сводка по главе 10. Расширения разметки XAML

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

Как правило, средство синтаксического анализа XAML преобразует любую строку, заданную в качестве значения атрибута, в тип свойства на основе стандартных преобразований для базовых типов данных .NET или производной класса [`TypeConverter`](xref:Xamarin.Forms.TypeConverter), присоединенной к свойству или типу этого свойства с помощью [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Но иногда бывает удобно задать атрибут из другого источника, например элемента в словаре, значения статического свойства или поля или какого-нибудь вычисления.

Для этих целей и существует *расширение разметки XAML*. Несмотря на название, расширения разметки XAML *не являются* расширением языка XML. XAML — это всегда обычный XML.

## <a name="the-code-infrastructure"></a>Инфраструктура кода

Расширение разметки XAML — это класс, реализующий интерфейс [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension). В конце имени такого класса часто содержится слово `Extension`, но обычно имя отображается в XAML без этого суффикса.

Следующие расширения разметки XAML поддерживаются во всех реализациях XAML:

- `x:Static` поддерживается в [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension);
- `x:Reference` поддерживается в [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension);
- `x:Type` поддерживается в [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension);
- `x:Null` поддерживается в [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension);
- `x:Array` поддерживается в [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension);

Следующие четыре расширения разметки XAML поддерживаются во многих реализациях XAML, включая Xamarin.Forms:

- `StaticResource` поддерживается в [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension);
- `DynamicResource` поддерживается в [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension);
- `Binding` поддерживается в [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash; см. здесь: [Глава 16. Привязка данных](chapter16.md)
- `TemplateBinding` поддерживается в [`TemplateBindingExtension`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension)&mdash; (не рассматривается в этой книге).

Дополнительное расширение разметки XAML включено в Xamarin.Forms в связи с [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash; (не рассматривается в этой книге).

## <a name="accessing-static-members"></a>Доступ к статическим элементам

Используйте элемент [`x:Static`](xref:Xamarin.Forms.Xaml.StaticExtension), чтобы задать для атрибута значение общего статического свойства, поля или элемента перечисления. Задайте для свойства [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) статический элемент. Обычно проще указывать `x:Static` и имя элемента в фигурных скобках. Не обязательно включать имя свойства `Member`, достаточно указать сам элемент. Этот типичный синтаксис показан в примере [**SharedStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics). Сами статические поля определяются в классе [`AppConstants`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs). Этот метод позволяет устанавливать константы, используемые в программе.

С помощью дополнительного объявления пространства имен XML можно ссылаться на общие статические свойства, поля или элементы перечисления, определенные в .NET Framework, как показано в примере [**SystemStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics).

## <a name="resource-dictionaries"></a>Словари ресурсов

Класс `VisualElement` определяет свойство с именем [`Resources`](xref:Xamarin.Forms.VisualElement.Resources), которое можно задать для объекта типа [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). В XAML можно хранить элементы в этом словаре и указывать их с помощью атрибута `x:Key`. Элементы, хранящиеся в словаре ресурсов, являются общими для всех ссылок на этот элемент.

### <a name="staticresource-for-most-purposes"></a>StaticResource для большинства целей

В большинстве случаев вы будете использовать расширение разметки [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension), чтобы сослаться на элемент из словаря ресурсов, как показано в примере [**ResourceSharing**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing). Можно использовать элемент `StaticResourceExtension` или `StaticResource` в фигурных скобках.

[![Тройной снимок экрана общего доступа к ресурсам](images/ch10fg03-small.png "Общий доступ к ресурсам")](images/ch10fg03-large.png#lightbox "Общий доступ к ресурсам")

Не путайте расширение разметки `x:Static` и расширение разметки `StaticResource`.

### <a name="a-tree-of-dictionaries"></a>Дерево словарей

Когда средство синтаксического анализа XAML встречает `StaticResource`, оно начинает поиск соответствующего ключа в визуальном дереве, а затем — поиск в `ResourceDictionary` в классе `App` приложения. Это позволяет элементам словаря ресурсов, расположенным более глубоко в визуальном дереве, переопределять словарь ресурсов, находящийся выше в визуальном дереве. Это показано в примере [**ResourceTrees**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees).

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource для особых целей

Расширение разметки `StaticResource` извлекает элемент из словаря при построении визуального дерева во время вызова `InitializeComponent`. Альтернативой `StaticResource` является расширение [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), которое поддерживает ссылку на ключ словаря и обновляет целевой объект при изменении элемента, на который ссылается ключ.

Разница между `StaticResource` и `DynamicResource` показана в примере [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic).

Свойство, заданное `DynamicResource`, должно поддерживаться привязываемым свойством, как описано в [главе 11 "Инфраструктура c возможностью привязки"](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Менее часто используемые расширения разметки

Используйте расширение разметки [`x:Null`](xref:Xamarin.Forms.Xaml.NullExtension), чтобы задать для свойства значение `null`.

Используйте расширение разметки [`x:Type`](xref:Xamarin.Forms.Xaml.TypeExtension), чтобы задать свойство для объекта .NET `Type`.

Для определения массива используйте [`x:Array`](xref:Xamarin.Forms.Xaml.ArrayExtension). Чтобы указать тип элементов массива, задайте для свойства [`Type`] расширение разметки `x:Type`.

## <a name="a-custom-markup-extension"></a>Пользовательское расширение разметки

Вы можете создавать собственные расширения разметки XAML, создав класс, реализующий интерфейс [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension), с помощью метода [`ProvideValue`](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)).

Класс [`HslColorExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) соответствует этому требованию. Он создает значение типа `Color` на основе значений свойств с именами `H`, `S`, `L` и `A`. Этот класс является первым элементом в библиотеке Xamarin.Forms под названием [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit), который создается и используется в этой книге.

В примере [**CustomExtensionDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) показано, как ссылаться на эту библиотеку и применять пользовательское расширение разметки.

## <a name="related-links"></a>Связанные ссылки

- [Глава 10, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Примеры для главы 10](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [Расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
