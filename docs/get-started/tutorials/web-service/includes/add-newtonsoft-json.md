---
ms.openlocfilehash: 4517415e2431193e56728e3bb1a24e3c7d119ca4
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634969"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...**:

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vs/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vs/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **панели решения** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet**:

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vsmac/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В окне **Управление пакетами NuGet** выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vsmac/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
