---
title: Сводная информация о Главе 9. Вызовы API конкретных платформ
description: Создание мобильных приложений с помощью Xamarin.Forms. Сводная информация о Главе 9. Вызовы API конкретных платформ
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6dec0c3e3fc4d25aecfe4e4141c4cc285fd7f8d8
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93366195"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>Сводная информация о Главе 9. Вызовы API конкретных платформ

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

> [!NOTE]
> Эта книга была опубликована весной 2016 года и с тех пор не обновлялась. Многое в этой книге остается ценным, но некоторые материалы устарели, а некоторые разделы перестали быть полностью верными или полными.

Иногда требуется выполнить код, меняющийся в зависимости от платформы. В этой главе рассматриваются методы.

## <a name="preprocessing-in-the-shared-asset-project"></a>Предварительная обработка в проекте общих ресурсов

Проект общих ресурсов Xamarin.Forms может выполнять разные коды для каждой платформы с помощью директив препроцессора C# `#if`, `#elif` и `endif`. Это продемонстрировано в [**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Тройной снимок экрана с переменным форматированием абзаца](images/ch09fg01-small.png "Модель устройства и операционная система")](images/ch09fg01-large.png#lightbox "Модель устройства и операционная система")

Однако результирующий код может быть трудным для чтения.

## <a name="parallel-classes-in-the-shared-asset-project"></a>Параллельные классы в проекте общих ресурсов

Более структурированный подход к выполнению кода, зависящего от платформы, продемонстрирован в SAP в примере [**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2). Каждый из проектов платформы содержит класс и методы с одинаковыми названиями, однако они реализованы для отдельной платформы. Затем SAP просто создает экземпляр класса и вызывает метод.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService и Переносимая библиотека классов

> [!NOTE]
> Переносимые библиотеки классов заменены библиотеками .NET Standard. Все примеры кода в этой книге преобразованы для использования библиотек .NET Standard.

Обычно библиотека не может получить доступ к классам в проектах приложений. По-видимому, это ограничение не позволяет использовать в библиотеке метод, показанный в **PlatInfoSap2**. Однако Xamarin.Forms содержит класс с именем [`DependencyService`](xref:Xamarin.Forms.DependencyService), который использует отражение .NET для доступа к открытым классам в проекте приложения из библиотеки.

Библиотека должна определять `interface` с элементами, которые она должна использовать на каждой платформе. Затем каждая из платформ содержит реализацию этого интерфейса. Класс, реализующий интерфейс, должен быть идентифицирован на уровне сборки с помощью [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute).

Затем, чтобы получить экземпляр класса платформы, реализующего интерфейс, библиотека использует универсальный метод [`Get`](xref:Xamarin.Forms.DependencyService.Get*) из `DependencyService`.

Это показано в примере [**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo).

## <a name="platform-specific-sound-generation"></a>Создание звука для конкретной платформы

Пример [**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) добавляет звуковые сигналы в программу **MonkeyTap**, осуществляя доступ к средствам создания звука на каждой платформе.

## <a name="related-links"></a>Связанные ссылки

- [Глава 9, полный текст в формате PDF](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Примеры для Главы 9](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Служба зависимостей](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
