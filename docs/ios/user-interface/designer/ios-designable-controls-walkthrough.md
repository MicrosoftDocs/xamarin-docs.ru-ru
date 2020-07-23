---
title: Использование пользовательских элементов управления в конструкторе iOS
description: В этом документе описывается, как создать пользовательский элемент управления и использовать его с Xamarin Designer для iOS. В нем показано, как сделать элемент управления доступным в панели элементов конструктора iOS, реализовать элемент управления таким образом, чтобы он правильно отображался и был доступен во время разработки, и многое другое.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4652497aa6a7819afe7224617a429b2852566255
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934697"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Использование пользовательских элементов управления в конструкторе iOS

## <a name="requirements"></a>Requirements (Требования)

Xamarin Designer для iOS доступен в Visual Studio для Mac и Visual Studio 2017 и более поздних версий в Windows.

В этом руководстве предполагается, что вы знакомы с содержанием, изложенным в [руководствах по начало работы](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Пошаговое руководство

> [!IMPORTANT]
> Начиная с Xamarin. Studio 5,5, способ создания пользовательских элементов управления немного отличается от предыдущих версий. Для создания пользовательского элемента управления `IComponent` необходим либо интерфейс (со связанными методами реализации), либо класс, с которым можно добавить заметки `[DesignTimeVisible(true)]` . Последний метод используется в следующем примере пошагового руководства.

1. Создайте новое решение из приложения **> iOS > приложение с одним представлением > шаблон C#** , присвойте ему имя `ScratchTicket` и продолжайте работу с мастером создания проекта:

    [![Создание нового решения](ios-designable-controls-walkthrough-images/01new.png)](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Создайте новый пустой файл класса с именем `ScratchTicketView` :

    [![Создание нового класса Скратчтиккетвиев](ios-designable-controls-walkthrough-images/02new.png)](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Добавьте следующий код для `ScratchTicketView` класса:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;

    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;

            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }

            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }

            public ScratchTicketView()
            {
                Initialize();
            }

            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }

            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }

            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }

            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }

            public override void Draw(CGRect rect)
            {
                base.Draw(rect);

                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);

                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();

                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }

                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```

1. Добавьте `FillTexture.png` `FillTexture2.png` `Monkey.png` файлы и (доступны [из GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) в папку **Resources** .

1. Дважды щелкните файл, `Main.storyboard` чтобы открыть его в конструкторе:

    [![Конструктор iOS](ios-designable-controls-walkthrough-images/03new.png)](ios-designable-controls-walkthrough-images/03new.png#lightbox)

1. Перетащите **представление изображения** из **панели элементов** в представление в раскадровке.

    [![Представление изображения, добавленное в макет](ios-designable-controls-walkthrough-images/04new.png)](ios-designable-controls-walkthrough-images/04new.png#lightbox)

1. Выберите **представление изображения** и измените его свойство **Image** на `Monkey.png` .

    [![Задание для свойства Image представления изображения значения Monkey.png](ios-designable-controls-walkthrough-images/05new.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

1. По мере использования классов размера необходимо ограничить это представление изображений. Дважды щелкните изображение, чтобы перевести его в режим ограничения. Добавим его в центр, щелкнув маркер крепления в центре и выровняйте его по вертикали и по горизонтали:

    [![Центрирование изображения](ios-designable-controls-walkthrough-images/06new.png)](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Чтобы ограничить высоту и ширину, щелкните дескрипторы закрепления размера (обрабатываются в форме "кость") и выберите соответственно ширину и высоту.

    [![Добавление ограничений](ios-designable-controls-walkthrough-images/07new.png)](ios-designable-controls-walkthrough-images/07new.png#lightbox)

1. Обновите фрейм на основе ограничений, нажав кнопку Обновить на панели инструментов:

    [![Панель инструментов "ограничения"](ios-designable-controls-walkthrough-images/08new.png)](ios-designable-controls-walkthrough-images/08new.png#lightbox)

1. Затем постройте проект, чтобы **представление временных билетов** отображалось в разделе **пользовательские компоненты** на панели элементов:

    [![Панель элементов пользовательских компонентов](ios-designable-controls-walkthrough-images/09new.png)](ios-designable-controls-walkthrough-images/09new.png#lightbox)

1. Перетащите **представление "создание временных билетов"** , чтобы оно появлялось над изображением обезьяны. Измените маркеры перетаскивания, чтобы представление временных билетов полностью обрабатывало эту обезьяну, как показано ниже:

    [![Представление временных билетов над представлением изображений](ios-designable-controls-walkthrough-images/10new.png)](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Ограничьте представление "Рабочая схема" представлением изображения, нарисовав ограничивающий прямоугольник для выбора обоих представлений. Выберите параметры, чтобы ограничить его шириной, высотой, центром и средним и обновлением кадров на основе ограничений, как показано ниже:

    [![Центрирование и Добавление ограничений](ios-designable-controls-walkthrough-images/11new.png)](ios-designable-controls-walkthrough-images/11new.png#lightbox)

1. Запустите приложение и "выключите" изображение, чтобы увидеть обезьяну.

    [![Запуск примера приложения](ios-designable-controls-walkthrough-images/10-app.png)](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Добавление свойств времени разработки

Конструктор также включает поддержку времени разработки для пользовательских элементов управления числовыми типами, перечислениями, строками, bool, Кгсизе, Уиколор и Уиимаже. Чтобы продемонстрировать, давайте добавим свойство в, `ScratchTicketView` чтобы задать изображение, которое будет "выключено".

Добавьте следующий код в `ScratchTicketView` класс для свойства:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image
{
    get { return image; }
    set {
            image = value;
              SetNeedsDisplay ();
        }
}
```

Также может потребоваться добавить проверку значения NULL в `Draw` метод, например так:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);

        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Включение `ExportAttribute` и `BrowsableAttribute` с аргументом, для которого задано значение, `true` приводит к отображению свойства на панели **свойств** конструктора. Изменение свойства на другой образ, входящий в состав проекта, например `FillTexture2.png` , приводит к обновлению элемента управления во время разработки, как показано ниже:

 [![Изменение свойств времени разработки](ios-designable-controls-walkthrough-images/11-customproperty.png)](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Итоги

В этой статье мы рассмотрели, как создать пользовательский элемент управления, а также как использовать его в приложении iOS с помощью конструктора iOS. Мы увидели, как создать и построить элемент управления, чтобы сделать его доступным для приложения на **панели элементов**конструктора. Кроме того, мы рассматривали, как реализовать элемент управления таким образом, чтобы он правильно отображался как во время разработки, так и в среде выполнения, а также как предоставить настраиваемые свойства элемента управления в конструкторе.

## <a name="related-links"></a>Связанные ссылки

- [Скратчтиккет (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/scratchticket)
- [требуемые образы (пример)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
