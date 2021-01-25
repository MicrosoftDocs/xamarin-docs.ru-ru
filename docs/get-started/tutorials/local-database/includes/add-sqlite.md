---
ms.openlocfilehash: e4c0e1500e6e445580a971c6af0af0fd267ee0f7
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98690194"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **LocalDatabaseTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **LocalDatabaseTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** выберите проект **LocalDatabaseTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...** .

    ![Снимок экрана: выбранный пункт меню "Управление пакетами NuGet"](../images/vs/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект.

    ![Снимок экрана: пакет NuGet для SQLite.NET в диспетчере пакетов NuGet](../images/vs/add-package.png "Пакет NuGet для SQLite.NET")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Авторы:** SQLite-net
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **LocalDatabaseTutorial**.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **LocalDatabaseTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **панели решения** выберите проект **LocalDatabaseTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet**:

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vsmac/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В окне **Управление пакетами NuGet** выполните поиск пакета NuGet **sqlite-net-pcl**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet для SQLite.NET в диспетчере пакетов NuGet](../images/vsmac/add-package.png "Пакет NuGet для SQLite.NET")

    > [!NOTE]
    > Существует ряд пакетов NuGet с похожими названиями. Правильный пакет имеет следующие атрибуты:
    > - **Идентификатор:** sqlite-net-pcl
    > - **Автор**: SQLite-net
    > - **Ссылка NuGet:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Несмотря на название, этот пакет NuGet можно использовать в проектах .NET Standard.

    Этот пакет будет использоваться для включения операций базы данных в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
