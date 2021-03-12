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
ms.openlocfilehash: dfc17ecabbf0dc079c3a8096d33bc1a6cfb3eed7
ms.sourcegitcommit: 4bbf54d2bc1df96af69814e2e5dae47be12e0474
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102602727"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Почему конструктор XAML Visual Studio не работает с Xamarin.Forms файлами XAML?

Xamarin.Forms Сейчас не поддерживает визуальные конструкторы для файлов XAML. Поэтому при попытке открыть файл XAML форм в *конструкторе пользовательского интерфейса XAML* Visual Studio или *конструкторе пользовательского интерфейса XAML с кодировкой* выдается следующее сообщение об ошибке:

> "Не удается открыть файл в выбранном редакторе. Выберите другой редактор ".

Это ограничение описано в разделе Руководство по [ Xamarin.Forms основам XAML](~/xamarin-forms/xaml/xaml-basics/index.md) :

> "Нет визуального конструктора для создания XAML в Xamarin.Forms приложениях, поэтому все XAML должны быть написаны вручную".

Однако Xamarin.Forms редактор XAML можно отобразить, выбрав пункт меню **вид > другой Xamarin.Forms Редактор > Windows** .
