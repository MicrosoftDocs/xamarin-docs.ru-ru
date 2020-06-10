---
title: Дополнительные изменения платформы macOS Sierra
description: В этом документе описываются незначительные изменения и улучшения существующих платформ, появившихся в macOS Sierra. Он изучает изменения в платформе ускорения, AppKit, Авфаундатион, основных данных, основном образе, фундаменте и т. д.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 03840e127ba53ee63623252585e51e51e6890eb5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571601"
---
# <a name="additional-macos-sierra-framework-changes"></a>Дополнительные изменения платформы macOS Sierra

<a name="Accelerate-Framework-Enhancements"></a>

## <a name="accelerate-framework-enhancements"></a>Усовершенствования платформы ускорения

В платформу ускорения для macOS Sierra были внесены следующие улучшения.

- Добавлен Куадратуре (целочисленный математического анализа за).
- Добавлены основные функции для создания нейронных сетей.
- Добавлены функции геометрического предиката для проверки на такие вещи, как пересечение двух геометрических объектов.

<a name="AppKit-Framework-Enhancements"></a>

## <a name="appkit-framework-enhancements"></a>Усовершенствования платформы AppKit

В платформу AppKit для macOS Sierra были внесены следующие улучшения.

- Несколько усовершенствований, `NSCollectionView` например:
  - **Сворачиваемые разделы** . позволяет пользователю сворачивать раздел представления коллекции в одну горизонтальную строку.
  - **Плавающие заголовки** — верхние и нижние колонтитулы теперь можно перемещать (в макете потока), используя тот же API, что и [уиколлектионвиев](https://developer.apple.com/reference/uikit/uicollectionview) в iOS.
  - **Прокручиваемые фоновые представления** — фон представлений коллекции теперь можно настроить для прокрутки и содержимого.
- Этап макета отложенного представления оптимизирован и расширен.
- API-интерфейс перетаскивания теперь включает новые `NSFilePromiseProvider` `NSFilePromiseReceiver` классы и для поддержки перетаскивания флоккинг.
- В существующие элементы управления добавлены несколько удобных конструкторов:
  - `NSButton`содержит новые конструкторы для создания кнопок, флажков и переключателей.
  - `NSTextField`содержит новые конструкторы для создания неупакованных и необтекаемых меток, меток с атрибутами и редактируемых текстовых полей.
  - `NSSegmentedControl`содержит новые конструкторы для создания сегментированных элементов управления из группы меток или изображений.
  - `NSSlider`содержит новые конструкторы для создания горизонтальных линейных ползунков.
  - `NSImageView`содержит новые конструкторы для создания неизменяемых представлений изображений из заданного `NSImage` .
- Новый элемент был `NSGridView` добавлен для автоматической разметки коллекции вложенных представлений в сетку с переменным размером строк и столбцов, которые могут динамически скрываться или отображаться.

<a name="AVFoundation-Framework-Enhancements"></a>

## <a name="avfoundation-framework-enhancements"></a>Усовершенствования платформы Авфаундатион

В платформу Авфаундатион для macOS Sierra были внесены следующие улучшения.

- В macOS приложение больше не должно реализовывать различные поведения [авплайеритем](https://developer.apple.com/reference/avfoundation/avplayeritem) на основе типа содержимого. Просто задайте `Rate` свойство, и авфаундатион определит, когда будет доступно достаточное содержимое для воспроизведения без ожидания.
- Новый `AVPlayerLooper` класс упрощает циклический перебор заданной части мультимедиа во время воспроизведения.
- `AVAssetDownloadURLSession`Класс позволяет загружать и более поздние воспроизводить Fairplay зашифрованные HLS потоки.

<a name="Core-Data-Framework-Enhancements"></a>

## <a name="core-data-framework-enhancements"></a>Усовершенствования основных платформ данных

В платформу Core Data Framework были внесены следующие улучшения для macOS Sierra:

- Корневые объекты [нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) поддерживают одновременную ошибку и выборку без сериализации.
- Класс [нсперсистентсторекурдинатор](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) поддерживает пул хранилищ данных SQLite.
- [Нсманажедобжектконтекст](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объекты с хранилищами данных SQLite в режиме журнала Wal поддерживают новую функцию создания запросов, где контексты управляемых объектов (MOC) могут быть закреплены в конкретных версиях базы данных для последующей выборки и сбоя транзакций.
- Использование высокого уровня `NSPersistenceContainer` для ссылки на `NSPersistentStoreCoordinator` [Нсманажедобжектмодел](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и другие основные ресурсы по настройке данных.
- Было добавлено несколько новых удобных методов для `NSManagedObject` упрощения выборки и создания подклассов.

Дополнительные сведения см. в [справочнике по основной платформе данных](https://developer.apple.com/reference/coredata)Apple.

<a name="Core-Image-Framework-Enhancements"></a>

## <a name="core-image-framework-enhancements"></a>Усовершенствования основной платформы образов

В основную платформу образов для macOS Sierra были внесены следующие улучшения.

- `ImageWithExtent`Метод класса [Цифилтер](https://developer.apple.com/reference/coreimage/cifilter) можно использовать для вставки пользовательской обработки в операцию фильтрации. Основное изображение будет вызывать заданный обратный вызов между фильтрами при обработке изображения для вывода или показа.
- Теперь приложение может обрабатывать изображения в цветовом пространстве за пределами рабочего пространства контекста образа путем преобразования в цветовое пространство и из него до и после обработки.
- Ядро образа ядра теперь может запрашивать конкретный выходной формат пикселей.
- Добавлены следующие новые фильтры изображений: `CINinePartTitled` , `CINinePartStretched` , `CIHueSaturationValueGradient` `CIEdgePreserveUpsampleFilter` и `CIClamp` .

<a name="Foundation-Framework-Enhancements"></a>

## <a name="foundation-framework-enhancements"></a>Усовершенствования в Foundation Framework

В платформу Foundation для macOS Sierra были внесены следующие улучшения.

- Используйте API [нсдиментионс](https://developer.apple.com/reference/foundation/nsdimension) для представления, преобразования и отображения многих наиболее распространенных физических устройств, таких как масса, длина, скорость, продолжительность и температура.
- Используйте класс [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) для синтаксического анализа и создания дат в формате ISO 8601.
- Новый класс [нсдатеинтервал](https://developer.apple.com/reference/foundation/nsdateinterval) используется для вычисления интервалов даты и времени, таких как длительность, для сравнения интервалов и тестирования для пересечения интервалов.
- Используйте класс [нсперсоннамекомпонентсформаттер](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) для анализа элементов имени человека из строки.
- Используйте новый класс [нсурлсессионтаскметрикс](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) для получения метрик для сеанса с сетевыми URL-адресами.

Дополнительные сведения см. в [статье заметки о выпуске Apple для OS X v 10.12 и iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements"></a>

## <a name="gamekit-framework-enhancements"></a>Усовершенствования платформы GameKit

В платформу GameKit для macOS Sierra были внесены следующие улучшения.

- **Game Centerное приложение** устарело и удалено из macOS. Если приложение использует GameKit, оно _должно_ предоставлять собственный интерфейс для отображения таких gamekit функций, как списки лидеров и т. д.
- Новый тип учетной записи только iCloud реализован классом [гкклаудплайер](https://developer.apple.com/reference/gamekit/gkcloudplayer) .
- Новый класс [гкгамесессион](https://developer.apple.com/reference/gamekit/gkgamesession) предоставляет обобщенное решение для управления хранилищем постоянных данных на Game Center. `GKGameSession`ведет список игроков, и приложение отвечает за реализацию того, как и когда дата участника сохраняется, извлекается или обменивается между игроками. Во многих случаях игровые сеансы могут заменять существующие соответствия, основанные на включении, совпадения в режиме реального времени или постоянные способы сохранения игр.

<a name="GamePlayKit-Framework-Enhancements"></a>

## <a name="gameplaykit-framework-enhancements"></a>Усовершенствования платформы Гамеплайкит

В платформу Гамеплайкит для macOS Sierra были внесены следующие улучшения.

- Было добавлено описание процедурного создания шума, которое можно использовать для улучшения реальных текстур в естественном виде, добавления факта к перемещению камеры и помощи в создании полнофункциональных игр.
- Используйте пространственное секционирование, чтобы секционировать реальные данные игры для эффективного поиска.
- Добавлен новый Монте-Карло специалист по стратегиям ([гкмонтекарлостратегист](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) для исчерпывающего возможного вычисления перемещения.
- Добавлен новый API дерева принятия решений ([гкдеЦисионтри](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [гкдеЦисионноде](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения искусственного проектирования.
- поддержка трехмерных объектов добавлена к существующим агентам и поведениям поиска пути с помощью новых классов [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) .
- Используйте новый класс [гкмешграф](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) для обеспечения высокопроизводительных и естественных путей.
- Новые классы [гксцене](https://developer.apple.com/reference/gameplaykit/gkscene) и [Гкскнодекомпонент](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) делают объединение гамеплайкит и SpriteKit проще, чем когда-либо.

<a name="Metal-Framework-Enhancements"></a>

## <a name="metal-framework-enhancements"></a>Усовершенствования в среде металла

В платформу металла были внесены следующие улучшения для macOS Sierra:

- Трехмерные приложения и игры теперь могут использовать _тесселяцию_ для эффективного отображения сложных сцен и геометрии с помощью GPU.
- Используйте специализацию функции, чтобы создать высокооптимизированный набор функций для комбинирования материалов и освещения для сцены.
- Обеспечение точного контроля за выделением ресурсов для оптимизации производительности металлических приложений с помощью куч ресурсов и целевых объектов визуализации, поддерживающих память.

Дополнительные сведения см. в статье [по программированию для металлического программирования](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)Apple.

<a name="ModelIO-Framework-Enhancements"></a>

## <a name="model-io-framework-enhancements"></a>Усовершенствования платформы ввода-вывода модели

В платформу ввода-вывода модели были внесены следующие улучшения для macOS Sierra:

- Теперь поддерживается формат файла USD.
- Используйте новый `MDLMaterialPropertyGraph` класс, чтобы легко поддерживать изменения среды выполнения в моделях.
- В класс [мдлвокселаррай](https://developer.apple.com/reference/modelio/mdlvoxelarray) добавлена поддержка поля со знакомого расстояния.
- Используйте новый `MDLLightProbeIrradianceDataSource` класс, чтобы упростить размещение зонда.

<a name="Photos-Framework-Enhancements"></a>

## <a name="photos-framework-enhancements"></a>Усовершенствования для платформы фотографий

В платформу Photos для macOS Sierra были внесены следующие улучшения.

- Редактирование фотографий в реальном времени теперь доступно для приложений, поддерживающих платформу фото, и для расширений редактирования фотографий (для использования в фотографиях и в фотоаппаратных приложениях).
- Используйте новый класс [фливефотоедитингконтекст](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) , чтобы применить изменения как к видео, так и по прежнему в содержимом живых фотографий.
- Используйте классы [Циимажепроцессоринпут](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) и [Циимажепроцессораутпут](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) , чтобы воспользоваться преимуществами новой функции процессора основных образов для внесения изменений.
- Для поддержки динамических фотографий классы [фливефото](https://developer.apple.com/reference/photos/phlivephoto) и [фливефотовиев](https://developer.apple.com/reference/photosui/phlivephotoview) были перенесены из iOS в macOS.

<a name="SceneKit-Framework-Enhancements"></a>

## <a name="scenekit-framework-enhancements"></a>Усовершенствования платформы SceneKit

В платформу SceneKit для macOS Sierra были внесены следующие улучшения.

- Теперь включает новую систему с физическим рендерингом (PBR) для более реалистичных результатов с более простым созданием ресурсов.
- Используйте новую модель заливки [скнлигхтингмоделфисикаллибасед](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) для произведения широкого спектра реалистичных эффектов заливки, при этом требуются только три фундаментальных свойства ( `Diffuse` `Metalness` и `Roughness` ).
- Поскольку заливка PBR лучше всего работает с освещением на основе среды, используйте `LightingEnvironment` свойство, чтобы назначить освещение на основе изображения для всей сцены Tan.
- Используйте `IESProfileURL` свойство для импорта реальных осветительных источников, определяющих основу освещения в реальных значениях, таких как интенсивность (в люменах) и цветовая температура (в градусах Кельвина).
- Класс [скнкамера](https://developer.apple.com/reference/scenekit/scncamera) обеспечивает более высокую реальную работу с помощью функций и эффектов HDR. Используйте адаптивную раскрытие, чтобы создать автоматические эффекты или использовать вигнеттинг, регулировку цвета и цветовую раскраску для добавления филматик эффектов в игру.
- Функции PBR и HDR-камеры предоставляют лучшие результаты по сравнению с традиционными методами отрисовки, и в результате SceneKit теперь выполняет все цветовые вычисления в линейном цветовом пространстве (при отображении цветовой гаммы P3 на устройствах с широким цветом).
- SceneKit теперь цвет соответствует всем цветам, считывая сведения о цветовых профилях.
- SceneKit интерпретирует значения цветовых компонентов в линейном цветовом пространстве RGB для всех типов шейдера.
- Так как SceneKit считывает и корректирует сведения о цветовых профилях в образах текстур, используйте каталоги активов для всех изображений, чтобы обеспечить их использование.
- Как линейное, так и расширенное отображение цветового пространства можно отключить, `SCNDisableLinearSpaceRendering` указав `SCNDisableWideGamut` ключи и в приложении `Info.plist` .
- Создайте произвольный многоугольник приматов (загружается из файлов или создается программно), чтобы указать геометрию с новым классом [скнжеометрипримитиветипеполигон](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon?language=objc) .

<a name="Security-Framework-Enhancements"></a>

## <a name="security-framework-enhancements"></a>Усовершенствования платформы безопасности

В платформу безопасности для macOS Sierra были внесены следующие улучшения.

- `SecKey`Интерфейс был современным и единым для всех платформ (iOS, tvOS, watchOS и macOS).

<a name="SpriteKit-Framework-Enhancements"></a>

## <a name="spritekit-framework-enhancements"></a>Усовершенствования платформы SpriteKit

В платформу SpriteKit для macOS Sierra были внесены следующие улучшения.

- Тилемапс теперь поддерживает фигуры мозаичных, шестиугольников и изометрических плиток для двумерных, 2,5 и прокрутых игр с `SKTileMapMode` помощью `SKTileGroup` `SKTileGroupRule` классов, и `SKTileSet` .
- Используйте новый `SKWarpGeometry` класс для растяжения или искажения отрисовки [Скспритеноде](https://developer.apple.com/reference/spritekit/skspritenode) или [скеффектноде](https://developer.apple.com/reference/spritekit/skeffectnode) . Новый класс [скактион](https://developer.apple.com/reference/spritekit/skaction) можно использовать для анимации переходов между эффектами деформации.
- Пользовательские шейдеры могут предоставлять атрибуты ( `SKAttribute` ), которые можно настроить отдельно для каждого узла, использующего шейдер, путем предоставления значения атрибута ( `SKAttributeValue` ).
- Класс [сквиев](https://developer.apple.com/reference/spritekit/skview) предоставляет несколько новых методов, позволяющих точно контролировать время и способ отрисовки сцены.

<a name="New-Frameworks"></a>

## <a name="new-frameworks"></a>Новые платформы

В macOS Sierra добавлены следующие платформы:

- **Платформа целей** . Эта платформа позволяет приложению проверять взаимодействие (например, расположение или действия пользователя) и предпринимать действия на основе этих сведений.
- **Платформа сафарисервицес** . Эта платформа позволяет приложению разрабатывать расширения приложений для Safari (например, блокирование содержимого) как для macOS, так и для iOS.

## <a name="related-links"></a>Связанные ссылки

- [Примеры Mac](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [Новые возможности в OS X 10,12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
