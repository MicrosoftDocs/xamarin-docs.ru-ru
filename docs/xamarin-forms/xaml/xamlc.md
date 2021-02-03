---
title: Компиляция XAML в Xamarin.Forms
description: В этой статье объясняется, как при необходимости можно скомпилировать код XAML непосредственно в промежуточный язык (IL) с помощью Xamarin.Forms компилятора XAML (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/03/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8d53f80372062f4830d92213110f01d005f92018
ms.sourcegitcommit: 4f274920d1fe906cda0bf83b8e928b3b50147d40
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2021
ms.locfileid: "99509888"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Компиляция XAML в Xamarin.Forms

_При необходимости можно воспользоваться компилятором XAML (XAMLC) и скомпилировать XAML напрямую в промежуточный язык (IL)._

Компиляция XAML предоставляет ряд преимуществ:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

Компиляция XAML по умолчанию отключена в платформе. Однако он включен в шаблонах для новых проектов. Можно явно включить или отключить ( `XamlCompilationOptions.Skip` ) на уровне сборки и класса, добавив [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) атрибут.

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

Хотя атрибут можно поместить в любом месте, поместите его в **AssemblyInfo.CS**.

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
