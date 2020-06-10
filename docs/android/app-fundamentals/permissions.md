---
title: Разрешения в Xamarin. Android
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 3204180ed28e98e52c4f0772ecb37a5353736acb
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570365"
---
# <a name="permissions-in-xamarinandroid"></a>Разрешения в Xamarin. Android

## <a name="overview"></a>Обзор

Приложения Android выполняются в собственной песочнице, и по соображениям безопасности не имеют доступа к определенным системным ресурсам или оборудованию на устройстве. Пользователь должен явно предоставить разрешение на доступ к приложению, прежде чем он сможет использовать эти ресурсы. Например, приложение не может получить доступ к GPS на устройстве без явного разрешения у пользователя. Android выдаст исключение, `Java.Lang.SecurityException` Если приложение пытается получить доступ к защищенному ресурсу без разрешения.

Разрешения объявляются в **файле AndroidManifest. XML** разработчиком приложения при разработке приложения. У Android есть два разных рабочих процесса для получения согласия пользователя для этих разрешений:

- Для приложений, предназначенных для Android 5,1 (уровень API 22) или ниже, при установке приложения был получен запрос на разрешение. Если пользователь не предоставил разрешения, приложение не будет установлено. После установки приложения невозможно отозвать разрешения, за исключением удаления приложения.
- Начиная с Android 6,0 (API уровня 23) пользователям было предоставлено больше возможностей для управления разрешениями; они могут предоставлять или отзывать разрешения при условии, что приложение установлено на устройстве. На этом снимке экрана показаны параметры разрешений для приложения Google Contacts. В нем перечислены различные разрешения и пользователи могут включать и отключать разрешения.

![Экран образцов разрешений](permissions-images/01-permissions-check.png) 

Приложения Android должны проверяться во время выполнения, чтобы узнать, есть ли у них разрешение на доступ к защищенному ресурсу. Если приложение не имеет разрешений, оно должно выполнять запросы с помощью новых API, предоставляемых пакет SDK для Android, чтобы предоставить разрешения пользователю. Разрешения делятся на две категории:

- **Обычные разрешения** &ndash; Это разрешения, которые приводят к небольшому риску безопасности при обеспечении безопасности или конфиденциальности пользователя. Android 6,0 автоматически предоставит обычные разрешения во время установки. [Полный список обычных разрешений](https://developer.android.com/guide/topics/permissions/normal-permissions.html)см. в документации по Android.
- **Опасные разрешения** &ndash; В отличие от обычных разрешений, опасными разрешениями являются те, которые защищают безопасность или конфиденциальность пользователя. Они должны быть явно предоставлены пользователем. Отправка или получение SMS-сообщения является примером действия, которое требует небезопасного разрешения.

> [!IMPORTANT]
> Категория, к которой относится разрешение, может меняться со временем.  Возможно, что разрешение, которое было отнесено к категории "обычные", может быть повышено на будущее уровне API до опасного разрешения.

Небезопасные разрешения доделятся на [_группы разрешений_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Группа разрешений будет хранить разрешения, которые логически связаны. Когда пользователь предоставляет разрешение на одного члена группы разрешений, Android автоматически предоставляет разрешение всем членам этой группы. Например, [`STORAGE`](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) Группа разрешений содержит как разрешения, так `WRITE_EXTERNAL_STORAGE` и `READ_EXTERNAL_STORAGE` . Если пользователь предоставляет разрешение на `READ_EXTERNAL_STORAGE` , то разрешение предоставляется `WRITE_EXTERNAL_STORAGE` автоматически в то же время.

Перед запросом одного или нескольких разрешений рекомендуется указать причину, по которой приложению требуется разрешение, прежде чем запрашивать разрешение. Когда пользователь понимает смысл, приложение может запросить разрешение у пользователя. Зная смысл, пользователь может принять обоснованное решение, если ему нужно предоставить разрешение и разобраться с последствиями, если это не так. 

Весь рабочий процесс проверки и запроса разрешений известен как проверка разрешений во _время выполнения_ , и его можно суммировать на следующей схеме: 

[![Блок-схема проверки разрешений во время выполнения](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

В библиотеке поддержки Android есть некоторые новые API для разрешений на более старые версии Android. Эти интерфейсы API с необязательными портами автоматически проверяют версию Android на устройстве, поэтому не требуется выполнять проверку на уровне API каждый раз.  

В этом документе описывается, как добавить разрешения в приложение Xamarin. Android и как приложения, предназначенные для Android 6,0 (уровень API 23) или выше, должны выполнять проверку разрешений во время выполнения.

> [!NOTE]
> Возможно, разрешения на оборудование могут повлиять на фильтрацию приложения по Google Play. Например, если приложению требуется разрешение для камеры, то Google Play не будет показывать приложение в Google Play Маркет на устройстве, на котором не установлена камера.

<a name="requirements"></a>

## <a name="requirements"></a>Требования

Настоятельно рекомендуется, чтобы проекты Xamarin. Android включали пакет NuGet [Xamarin. Android. support. с совместимостью](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) . Этот пакет будет выполнять неограниченный доступ к более старым версиям Android для интерфейсов API, предоставляя один общий интерфейс без необходимости постоянно проверять версию Android, на которой выполняется приложение.

## <a name="requesting-system-permissions"></a>Запрос разрешений системы

Первым шагом в работе с разрешениями Android является объявление разрешений в файле манифеста Android. Это необходимо сделать независимо от уровня API, предназначенных приложением.

Приложения, предназначенные для Android 6,0 или более поздней версии, не могут предположить, что, поскольку пользователь предоставил разрешение в некоторый момент времени в прошлом, это разрешение будет действительным в следующий раз. Приложение, предназначенное для Android 6,0, должно всегда выполнять проверку разрешений среды выполнения. Приложениям, предназначенным для Android 5,1 или более низкого уровня, не требуется выполнять проверку разрешений во время выполнения.

> [!NOTE]
> Приложения должны запрашивать только те разрешения, которые им необходимы.

### <a name="declaring-permissions-in-the-manifest"></a>Объявление разрешений в манифесте

Разрешения добавляются в **AndroidManifest. XML** с `uses-permission` элементом. Например, если приложение должно определять положение устройства, для него требуются точные разрешения и расположение курса. В манифест добавляются следующие два элемента: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Разрешения можно объявить с помощью встроенной поддержки средств Visual Studio:

1. Дважды щелкните **Свойства** в **Обозреватель решений** и выберите вкладку **Манифест Android** в окно свойств:

    [![Необходимые разрешения на вкладке манифеста Android](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Если в приложении еще нет AndroidManifest. XML, нажмите кнопку **No AndroidManifest. XML. Щелкните, чтобы добавить его** , как показано ниже.

    [![Нет сообщения AndroidManifest. XML](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Выберите разрешения, необходимые приложению, в списке **требуемые разрешения** и сохраните:

    [![Пример выбранных разрешений камеры](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio для Mac](#tab/macos)

Разрешения можно объявить с помощью встроенной поддержки средств в Visual Studio для Mac:

1. Дважды щелкните проект в **панель решения** и выберите **параметры > сборка > приложение Android**:

    [![Раздел необходимых разрешений показан](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Нажмите кнопку **добавить манифест Android** , если проект еще не содержит файл **AndroidManifest. XML**:

    [![Манифест Android проекта отсутствует](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Выберите необходимые для приложения разрешения в списке **требуемые разрешения** и нажмите кнопку **ОК**.

    [![Пример выбранных разрешений камеры](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin. Android будет автоматически добавлять некоторые разрешения во время сборки для отладки сборок. Это упростит отладку приложения. В частности, два важных разрешения: `INTERNET` и `READ_EXTERNAL_STORAGE` . Эти автоматически заданные разрешения не будут включены в списке **требуемые разрешения** . Однако сборки выпуска используют только те разрешения, которые явно заданы в списке **необходимые разрешения** . 

Для приложений, предназначенных для Android 5.1 (уровень API 22) или ниже, больше ничего делать не нужно. Приложения, которые будут работать на Android 6,0 (API 23 уровня 23) или выше, должны перейти к следующему разделу о выполнении проверок разрешений времени выполнения. 

### <a name="runtime-permission-checks-in-android-60"></a>Проверки разрешений среды выполнения в Android 6,0

`ContextCompat.CheckSelfPermission`Метод (доступный в библиотеке поддержки Android) используется для проверки предоставления определенного разрешения. Этот метод возвращает [`Android.Content.PM.Permission`](xref:Android.Content.PM.Permission) перечисление, которое имеет одно из двух значений:

- **`Permission.Granted`**&ndash;Указанное разрешение предоставлено.
- **`Permission.Denied`**&ndash;Указанное разрешение не предоставлено.

В этом фрагменте кода приведен пример того, как проверить наличие разрешения камеры в действии: 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Рекомендуется информировать пользователя о том, почему для приложения требуется разрешение, чтобы было информированное решение предоставить разрешение. Примером этого может быть приложение, которое принимает фотографии и географические теги. Пользователю понятно, что требуется разрешение камеры, но может быть неясно, почему приложению также требуется расположение устройства. Обоснование должно отображать сообщение, помогающее пользователю понять, почему разрешение расположения желательно и что требуется разрешение камеры.

`ActivityCompat.ShouldShowRequestPermissionRationale`Метод используется для определения, следует ли показывать логическое значение пользователю. Этот метод возвращает `true` , если обоснование для данного разрешения должно отображаться. На этом снимке экрана показан пример Снаккбар, отображаемого приложением, в котором объясняется, почему приложению необходимо выяснить расположение устройства:

![Рациональное расположение](permissions-images/07-rationale-snackbar.png) 

Если пользователь предоставляет разрешение, `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` метод должен быть вызван. Этот метод требует наличия следующих параметров:

- **действие** &ndash; Это действие, запрашивающее разрешения и информированное от Android результатов.
- **разрешения** &ndash; Список запрашиваемых разрешений.
- **requestcode вызывает нестандартное** &ndash; Целочисленное значение, которое используется для сопоставления результатов запроса разрешения с `RequestPermissions` вызовом. Значение должно быть больше нуля.

Этот фрагмент кода является примером двух обсуждаемых методов. Во-первых, выполняется проверка, чтобы определить, должно ли быть показано логическое разрешение. Если необходимо отобразить обоснование, то Снаккбар отображается с обоснованием. Если пользователь нажмет кнопку **ОК** в снаккбар, приложение запросит разрешения. Если пользователь не принимает обоснование, приложение не должно продолжать отвечать на запросы разрешений. Если обоснование не отображается, действие запрашивает разрешение:

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission`может вызываться, даже если пользователь уже предоставил разрешение. Последующие вызовы не требуются, но они предоставляют пользователю возможность подтвердить (или отозвать) разрешение. Когда `RequestPermission` вызывается метод, управление передается операционной системе, что приведет к отображению пользовательского интерфейса для принятия разрешений:  

![Диалоговое окно пермссион](permissions-images/08-location-permission-dialog.png)

После завершения работы пользователя Android возвратит результаты в действие с помощью метода обратного вызова `OnRequestPermissionResult` . Этот метод является частью интерфейса, `ActivityCompat.IOnRequestPermissionsResultCallback` который должен быть реализован действием. Этот интерфейс содержит один метод, `OnRequestPermissionsResult` который будет вызываться Android для информирования о действиях, выбранных пользователем. Если пользователь предоставил разрешение, приложение может использовать защищенный ресурс. Ниже приведен пример реализации `OnRequestPermissionResult` . 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permission_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  

## <a name="summary"></a>Сводка

В этом руководство было описано, как добавлять и проверять разрешения на устройстве Android. Различия в том, как разрешения работают между старыми приложениями Android (уровень API < 23) и новыми приложениями Android (уровень API > 22). Было рассмотрено, как выполнять проверки разрешений во время выполнения в Android 6,0.

## <a name="related-links"></a>Связанные ссылки

- [Список обычных разрешений](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Пример приложения "разрешения среды выполнения"](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Обработка разрешений в Xamarin. Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
