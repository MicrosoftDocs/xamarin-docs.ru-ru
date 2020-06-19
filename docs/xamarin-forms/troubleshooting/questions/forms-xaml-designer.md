---
title: Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f9cf570da0d8e078d1d05e74dc0f65994080c6c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135893"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?

Xamarin.FormsСейчас не поддерживает визуальные конструкторы для файлов XAML. Поэтому при попытке открыть файл XAML форм в *конструкторе пользовательского интерфейса XAML* Visual Studio или *конструкторе пользовательского интерфейса XAML с кодировкой*выдается следующее сообщение об ошибке:

> "Не удается открыть файл в выбранном редакторе. Выберите другой редактор ".

Это ограничение описано в разделе Руководство по [ Xamarin.Forms основам XAML](~/xamarin-forms/xaml/xaml-basics/index.md) :

> «Пока нет визуального конструктора для создания XAML в Xamarin.Forms приложениях, поэтому все XAML должны быть написаны вручную».

Однако Xamarin.Forms Предварительный просмотр XAML можно отобразить, выбрав пункт **Просмотр > другие окна Windows > Xamarin.Forms предварительного просмотра** .
