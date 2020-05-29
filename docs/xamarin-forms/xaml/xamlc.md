---
title: Компиляция XAML вXamarin.Forms
description: В этой статье объясняется, как при необходимости можно скомпилировать код XAML непосредственно в промежуточный язык (IL) с помощью Xamarin.Forms компилятора XAML (XAMLC).
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eebbb3040175118320639bcb4482ec77b5c16ac7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137297"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Компиляция XAML вXamarin.Forms

_При необходимости можно воспользоваться компилятором XAML (XAMLC) и скомпилировать XAML напрямую в промежуточный язык (IL)._

Компиляция XAML предоставляет ряд преимуществ:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

Компиляция XAML по умолчанию отключена для обеспечения обратной совместимости. Его можно включить на уровне сборки и класса, добавив [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) атрибут.

В следующем примере кода показано включение компиляции XAML на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

В этом примере выполняется проверка во время компиляции всех XAML-кода, содержащихся в сборке, при этом ошибки XAML сообщаются во время компиляции, а не во время выполнения. Таким образом, `assembly` префикс [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) атрибута указывает, что атрибут применяется ко всей сборке.

> [!NOTE]
> [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)Атрибут и [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) перечисление находятся в `Xamarin.Forms.Xaml` пространстве имен, которое необходимо импортировать, чтобы использовать их.

В следующем примере кода показано включение компиляции XAML на уровне класса:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

В этом примере выполняется проверка XAML для `HomePage` класса и ошибок, передаваемых в процессе компиляции.

> [!NOTE]
> Скомпилированные привязки можно включить для повышения производительности привязки данных в Xamarin.Forms приложениях. Дополнительные сведения см. в статье [Скомпилированные привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="related-links"></a>Связанные ссылки

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
