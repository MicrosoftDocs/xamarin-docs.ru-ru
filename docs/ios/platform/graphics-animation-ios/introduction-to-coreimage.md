---
title: Основные образы в Xamarin. iOS
description: Основной образ — это новая платформа, появившаяся в iOS 5 для обеспечения обработки изображений и функций динамического воспроизведения видео. В этой статье представлены эти функции с примерами Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 9e0363f941784ecc488861c0d2f089a30c275a10
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937401"
---
# <a name="core-image-in-xamarinios"></a>Основные образы в Xamarin. iOS

_Основной образ — это новая платформа, появившаяся в iOS 5 для обеспечения обработки изображений и функций динамического воспроизведения видео. В этой статье представлены эти функции с примерами Xamarin. iOS._

Основной образ — это новая платформа, появившаяся в iOS 5, которая предоставляет ряд встроенных фильтров и эффектов, применяемых к изображениям и видео, включая обнаружение лиц.

Этот документ содержит простые примеры:

- Обнаружение лиц.
- Применение фильтров к изображению
- Список доступных фильтров.

Эти примеры помогут вам приступить к внедрению основных функций образа в приложения Xamarin. iOS.

## <a name="requirements"></a>Requirements (Требования)

Необходимо использовать последнюю версию Xcode.

## <a name="face-detection"></a>Обнаружение лиц

Основная функция обнаружения изображений основного образа делает то, что она говорит — пытается определить лиц в фотографии и возвращает координаты всех распознаваемых лиц. Эти сведения можно использовать для подсчета количества людей в изображении, отображения индикаторов на изображении (например, для людей, помечающих теги в фотографии), или что-нибудь еще, вы можете подумать.

Этот код из Кореимаже\самплекоде.КС демонстрирует создание и использование распознавания лиц на внедренном изображении:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Массив Features будет заполнен `CIFaceFeature` объектами (если обнаружены какие-либо лица). `CIFaceFeature`Для каждой грани. `CIFaceFeature` имеет следующие свойства:

- Хасмауспоситион — был ли обнаружен рот для этого лица.
- Хаслефтэйепоситион — указывает, был ли обнаружен левый глаз для этой грани.
- Хасригхтэйепоситион — указывает, был ли обнаружен верный глаз для этого лица. 
- Мауспоситион — координаты рот для этого лица.
- Лефтэйепоситион — координаты левого глаза для этого лица.
- Ригхтэйепоситион — координаты правильного глаза для этого лица.

Координаты всех этих свойств имеют свое происхождение в левом нижнем углу, в отличие от UIKit, где в качестве источника используется левый верхний. При использовании координат убедитесь, `CIFaceFeature` что они переворачиваются. Это очень простое представление пользовательского изображения в Кореимаже\кореимажевиевконтроллер.КС демонстрирует, как рисовать на изображении треугольники "индикатор лица" (Обратите внимание на `FlipForBottomOrigin` метод):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Затем в файле SampleCode.cs образ и компоненты присваиваются до перерисовки изображения:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

На снимке экрана показан пример выходных данных: расположения обнаруженных функций-лиц отображаются в Уитекствиев и нарисованы на исходном изображении с помощью Кореграфикс.

Из-за того, как распознавание лиц работает, оно иногда обнаруживает вещи, кроме лиц, таких как Toy монкэйс!.

## <a name="filters"></a>Фильтры

Существует более 50 различных встроенных фильтров, и платформа является расширяемой, чтобы можно было реализовать новые фильтры.

## <a name="using-filters"></a>Применение фильтров

Применение фильтра к изображению состоит из четырех отдельных шагов: Загрузка изображения, создание фильтра, применение фильтра и сохранение (или отображение) результата.

Сначала загрузите изображение в `CIImage` объект.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Во вторых, создайте класс Filter и задайте его свойства.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

В третьих, обратитесь к `OutputImage` свойству и вызовите `CreateCGImage` метод, чтобы отобразить окончательный результат.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Наконец, назначьте изображение для представления, чтобы увидеть результат. В реальных приложениях полученный образ может быть сохранен в файловой системе, фотоальбоме, твите или электронной почте.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

На этих снимках экрана показан результат `CISepia` `CIHueAdjust` фильтров и, показанных в CoreImage.zip образце кода.

Пример фильтра см. в разделе [Настройка контракта и яркости рецепта изображения](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) `CIColorControls` .

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Список фильтров и их свойств

Этот код из Кореимаже\самплекоде.КС выводит полный список встроенных фильтров и их параметров.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

В [справочнике по классу Цифилтер](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) описаны встроенные фильтры 50 и их свойства. С помощью приведенного выше кода можно запрашивать классы фильтров, в том числе значения по умолчанию для параметров и максимальное и минимальное допустимые значения (которые можно использовать для проверки входных данных перед применением фильтра).

Выходные данные категорий списка выглядят в симуляторе — прокрутите список, чтобы увидеть все фильтры и их параметры.

 [![Выходные данные категорий списка выглядят следующим образом в симуляторе](introduction-to-coreimage-images/coreimage05.png)](introduction-to-coreimage-images/coreimage05.png#lightbox)

Каждый указанный фильтр представлен в виде класса в Xamarin. iOS, поэтому можно также изучить API Xamarin. iOS. Кореимаже в браузере сборок или использовать автозаполнение в Visual Studio для Mac или Visual Studio. 

## <a name="summary"></a>Итоги

В этой статье показано, как использовать некоторые новые функции платформы образов iOS 5 Core, такие как обнаружение лиц и применение фильтров к изображению. В платформе есть десятки различных фильтров изображений, которые можно использовать.

## <a name="related-links"></a>Связанные ссылки

- [Основное изображение (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/coreimage)
- [Настройка контракта и яркости для рецепта изображения](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [Использование основных фильтров образов](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Справочник по классам Цифилтер](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
