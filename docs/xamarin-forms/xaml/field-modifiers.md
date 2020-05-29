---
title: Модификаторы полей XAML вXamarin.Forms
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
ms.openlocfilehash: db00f522b71a8993ef0f7f6cf5070813ce07396a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138129"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>Модификаторы полей XAML вXamarin.Forms

`x:FieldModifier`Атрибут Namespace задает уровень доступа для созданных полей именованных элементов XAML. Допустимые значения атрибута:

- `private`— Указывает, что созданное поле для элемента XAML доступно только в теле класса, в котором он объявлен.
- `public`— Указывает, что созданное поле для элемента XAML не имеет ограничений доступа.
- `protected`— Указывает, что созданное поле для элемента XAML доступно в своем классе и экземплярах производного класса.
- `internal`— Указывает, что созданное поле для элемента XAML доступно только в типах в той же сборке.
- `notpublic`— Указывает, что созданное поле для элемента XAML доступно только в типах в той же сборке.

По умолчанию, если значение атрибута не задано, то созданным полем для элемента будет `private` .

> [!NOTE]
> Значение атрибута может использовать любой регистр, так как он будет преобразован в нижний регистр Xamarin.Forms .

Для обработки атрибута должны выполняться следующие условия `x:FieldModifier` .

- Элемент XAML верхнего уровня должен быть допустимым `x:Class` .
- Текущий элемент XAML имеет указанный объект `x:Name` .

В следующем коде XAML показаны примеры установки атрибута:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier`Атрибут нельзя использовать для указания уровня доступа класса XAML.
