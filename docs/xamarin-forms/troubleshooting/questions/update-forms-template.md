---
title: Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bdead80671a1ae6539de6614441df7e86863a5a6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137479"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?

В этом руководством в Xamarin.Forms качестве примера используется шаблон библиотеки .NET Standard, но один и тот же общий метод также будет работать для Xamarin.Forms шаблона общего проекта. Это руководство написано с примером обновления с Xamarin.Forms 1.5.1.6471 на 2.1.0.6529, но для этого можно задать другие версии по умолчанию.

1. Скопируйте исходный шаблон `.zip` из:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. Распакуйте во `.zip` временное расположение.

3. Измените все вхождения старой версии Xamarin.Forms пакета на новую версию, которую вы хотите использовать.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Пример: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`.

4. Измените элемент Name основного [файла многопроектного шаблона](https://msdn.microsoft.com/library/ms185308.aspx) ( `Xamarin.Forms.PCL.vstemplate` ), чтобы сделать его уникальным. Пример.

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Повторно заархивировать всю папку шаблона. Обязательно сопоставьте исходную файловую структуру `.zip` файла. `Xamarin.Forms.PCL.vstemplate`Файл должен находиться в верхней части `.zip` файла, а не в папках.

6. Создайте подкаталог "Мобильные приложения" в папке шаблонов Visual Studio для каждого пользователя:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Скопируйте новую ZIP-папку шаблона в новый каталог "Мобильные приложения".

8. Скачайте пакет NuGet, соответствующий версии, из шага 3. Например, [ https://nuget.org/api/v2/package/ Xamarin.Forms /2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (см. также [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file) ) и скопируйте его в соответствующую вложенную папку в папке расширения Xamarin Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
