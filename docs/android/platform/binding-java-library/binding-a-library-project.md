---
title: Привязка проекта библиотеки Eclipse
description: В этом пошаговом руководстве объясняется, как использовать шаблоны проектов Xamarin.Android для привязки проекта библиотеки Android Eclipse.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 54de1cde65e9c1ab9cb758c057a87c6b15c4da54
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853031"
---
# <a name="binding-an-eclipse-library-project"></a>Привязка проекта библиотеки Eclipse

> [!IMPORTANT]
> В настоящее время рассматривается возможность использования настраиваемых привязок на платформе Xamarin. Примите участие в [**этом опросе**](https://www.surveymonkey.com/r/KKBHNLT), чтобы помочь определить дальнейшие направления разработки.

_В этом пошаговом руководстве объясняется, как использовать шаблоны проектов Xamarin.Android для привязки проекта библиотеки Android Eclipse._

## <a name="overview"></a>Обзор

Хотя файлы AAR все чаще используются для распространения библиотек Android, в некоторых случаях необходимо создать привязку для *проекта библиотеки Android*. Проекты библиотеки Android — это специальные проекты Android, содержащие общий код и ресурсы, на которые могут ссылаться проекты приложений Android. Как правило, при создании библиотеки в интегрированной среде разработки Eclipse выполняется привязка к проекту библиотеки Android.
В этом пошаговом руководстве приведены примеры создания ZIP-файла проекта библиотеки Android из структуры каталогов проекта Eclipse.

Проекты библиотеки Android отличаются от обычных проектов Android тем, что они не компилируются в APK и не предусматривают, сами по себе, развертывание на устройстве. Вместо этого проект библиотеки Android предназначен для использования в проекте приложения Android. При создании проекта приложения Android сначала компилируется проект библиотеки Android. Затем проект приложения Android будет внедрен в скомпилированный проект библиотеки Android и добавит код и ресурсы в APK для распространения. Из-за этого различия создание привязки для проекта библиотеки Android немного отличается от создания привязки для JAR- или AAR-файла Java.

## <a name="walkthrough"></a>Пошаговое руководство

Чтобы использовать проект библиотеки Android в проекте привязки Xamarin.Android Java, сначала необходимо создать проект библиотеки Android в Eclipse. На следующем снимке экрана показан пример проекта библиотеки Android после компиляции: 

[![Пример проекта библиотеки в Eclipse](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Обратите внимание, что исходный код из проекта библиотеки Android был скомпилирован во временный JAR-файл с именем **android-mapviewballoons.jar**, а ресурсы были скопированы в папку **bin/res/crunch**. 

После компиляции проекта библиотеки Android в Eclipse его можно будет привязать с помощью проекта привязки Xamarin.Android Java. Сначала следует создать ZIP-файл, содержащий папки **bin** и **res** проекта библиотеки Android. Важно удалить промежуточный подкаталог **crunch**, чтобы ресурсы находились в папке **bin/res**. На снимке экрана ниже показано содержимое одного из таких ZIP-файлов: 

[![Содержимое файла Project.zip библиотеки Android](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Этот ZIP-файл добавляется в проект привязки Xamarin.Android Java, как показано на снимке экрана ниже:

[![ZIP-файл добавлен в проект привязки Java](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Обратите внимание, что для действия сборки ZIP-файла автоматически задается значение **LibraryProjectZip**.

При наличии JAR-файлов, необходимых для проекта библиотеки Android, их нужно добавить в папку **Jars** проекта библиотеки привязки Java и для параметра **Build Action** задать значение **ReferenceJar**. Пример этого можно увидеть на снимке экрана ниже: 

[![Для параметра Build Action задано значение ReferenceJar](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

После выполнения этих действий можно использовать проект привязки Java для Xamarin.Android, как описано выше в этом документе.

> [!NOTE]
> Компиляция проектов библиотеки Android в других IDE в настоящее время не поддерживается. Другие IDE не могут создать ту же структуру каталогов или файлы в папке **bin**, как это делает Eclipse. 

## <a name="summary"></a>Сводка

В этой статье рассмотрен процесс привязки проекта библиотеки Android. Мы создали проект библиотеки Android в Eclipse, а затем создали ZIP-файл из папок **bin** и **res** в проекте библиотеки Android. Затем мы использовали этот ZIP-файл для создания проекта привязки Java для Xamarin.Android. 
