---
title: Установка предварительной версии Xamarin в ОС Windows
description: Этот документ описывает установку предварительной версии Xamarin в Visual Studio 2019 с помощью канала предварительного выпуска.
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c74c132a623388f1d0b3b467b73d948cf2a2022
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91437255"
---
# <a name="installing-xamarin-preview-on-windows"></a>Установка предварительной версии Xamarin в ОС Windows

Visual Studio 2019 и Visual Studio 2017 не поддерживают альфа- и бета-каналы и стабильные каналы так, как предыдущие версии. Вместо этого есть два варианта:

- **выпуск** — эквивалент _стабильного_ канала в Visual Studio для Mac;
- **предварительная версия** — эквивалент _альфа-_ и _бета-_ каналов в Visual Studio для Mac.

> [!TIP]
> Для пробного использования функций предварительной версии следует [скачать установщик предварительной версии Visual Studio](https://visualstudio.microsoft.com/vs/preview/), который предложит установить **предварительные версии** Visual Studio параллельно со стабильной версией (версией выпуска). Дополнительные сведения о новых возможностях Visual Studio 2019 можно найти в [заметках о выпуске](/visualstudio/releases/2019/release-notes).

Предварительная версия Visual Studio может включать соответствующие предварительные версии функциональных возможностей Xamarin, в том числе следующие:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Inspector
- Симулятор iOS удаленной работы для Xamarin

На снимке экрана **установщика предварительной версии** ниже показаны параметры и предварительной версии, и выпуска (обратите внимание на номера версий, указанные серым цветом: версия 15.0 относится к выпуску, а версия 15.1 — к предварительной версии):

![отображение параметров предварительной версии в установщике](windows-images/vs2017-installer.jpg)

В процессе установки, если выполняется параллельная установка, может применяться **псевдоним установки** (чтобы версии можно было различить в меню "Пуск"), как показано ниже:

[![Изменение псевдонима перед установкой](windows-images/vs2017-nickname-sml.png "Изменение псевдонима перед установкой")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2019-preview"></a>Удаление предварительной версии Visual Studio 2019

**Установщик Visual Studio** также следует использовать для удаления установленных предварительных версий Visual Studio 2019. Дополнительные сведения см. в руководстве по [удалению Xamarin](uninstalling-xamarin.md#uninstallvs2017).