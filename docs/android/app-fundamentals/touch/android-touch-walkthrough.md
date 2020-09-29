---
title: Пошаговое руководство. Использование сенсорного ввода в Android
ms.prod: xamarin
ms.assetid: E281F89B-4142-4BD8-8882-FB65508BF69E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
ms.openlocfilehash: b7db20c2d51e96de8fcadaf5d50627132946177c
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455291"
---
# <a name="walkthrough---using-touch-in-android"></a>Пошаговое руководство. Использование сенсорного ввода в Android

Давайте посмотрим, как использовать концепции из предыдущего раздела в рабочем приложении. Мы создадим приложение с четырьмя действиями. Первое действие — это меню или кнопочная форма, которая запустит другие действия для демонстрации различных интерфейсов API. На следующем снимке экрана показано основное действие:

[![Пример снимка экрана с кнопкой "сенсорный ввод"](android-touch-walkthrough-images/image14.png)](android-touch-walkthrough-images/image14.png#lightbox)

Первое действие, пример касания, покажет, как использовать обработчики событий для прикосновения к представлениям. Действие распознавателя жестов демонстрирует, как создать подкласс `Android.View.Views` и обработку событий, а также показать, как работать с жестами сжатия. Третье и завершающее действие, **Настраиваемый жест**, покажет, как использовать пользовательские жесты. Чтобы упростить дальнейшие действия и придерживаться, мы удалим это пошаговое руководство в разделы, где каждый раздел посвящен одному из действий.

## <a name="touch-sample-activity"></a>Пример действия Touch

- Откройте проект **таучвалксраугх \_ Start**. **MainActivity** все настроено для того, чтобы &ndash; мы смогли реализовать поведение касания в действии. Если запустить приложение и щелкнуть **образец касания**, должно начаться следующее действие:

  [![Отображается снимок экрана действий с касанием](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

- Теперь, когда мы подтвердили, что действие запускается, откройте файл **TouchActivity.CS** и добавьте обработчик для `Touch` события `ImageView` :

  ```csharp
  _touchMeImageView.Touch += TouchMeImageViewOnTouch;
  ```

- Затем добавьте следующий метод в **TouchActivity.CS**:

  ```csharp
  private void TouchMeImageViewOnTouch(object sender, View.TouchEventArgs touchEventArgs)
  {
      string message;
      switch (touchEventArgs.Event.Action & MotionEventActions.Mask)
      {
          case MotionEventActions.Down:
          case MotionEventActions.Move:
          message = "Touch Begins";
          break;

          case MotionEventActions.Up:
          message = "Touch Ends";
          break;

          default:
          message = string.Empty;
          break;
      }

      _touchInfoTextView.Text = message;
  }
  ```

Обратите внимание, что в приведенном выше коде `Move` действие и обрабатывается `Down` как одно и то же. Это связано с тем, что несмотря на то, что пользователь может не допустить палец пальца `ImageView` , он может перемещаться или задавление, которое может изменить пользователь. Эти типы изменений приводят к созданию `Move` действия.

Каждый раз, когда пользователь обращается к `ImageView` , `Touch` будет вызвано событие, и наш обработчик отобразит на экране **касание** сообщения, как показано на следующем снимке экрана:

[![Снимок экрана действий с касанием начинается](android-touch-walkthrough-images/image15.png)](android-touch-walkthrough-images/image15.png#lightbox)

При условии, что пользователь касается `ImageView` , **касание начнет** отображаться в `TextView` . Когда пользователь больше не касается `ImageView` , на экране будут отображаться **концы** сообщений `TextView` , как показано на следующем снимке экрана:

[![Снимок экрана действий с сенсорными элементами](android-touch-walkthrough-images/image16.png)](android-touch-walkthrough-images/image16.png#lightbox)

## <a name="gesture-recognizer-activity"></a>Действие распознавателя жестов

Теперь позволяет реализовать действие распознавателя жестов. Это действие демонстрирует, как перетащить представление по экрану и проиллюстрировать один из способов реализации сжатия.

- Добавьте новое действие в приложение с именем `GestureRecognizer` .
  Измените код для этого действия, чтобы он соответствовал следующему коду:

  ```csharp
  public class GestureRecognizerActivity : Activity
  {
      protected override void OnCreate(Bundle bundle)
      {
          base.OnCreate(bundle);
          View v = new GestureRecognizerView(this);
          SetContentView(v);
      }
  }
  ```

- Добавьте в проект новое представление Android и присвойте ему имя `GestureRecognizerView` . Добавьте в этот класс следующие переменные:

  ```csharp
  private static readonly int InvalidPointerId = -1;

  private readonly Drawable _icon;
  private readonly ScaleGestureDetector _scaleDetector;

  private int _activePointerId = InvalidPointerId;
  private float _lastTouchX;
  private float _lastTouchY;
  private float _posX;
  private float _posY;
  private float _scaleFactor = 1.0f;
  ```

- Добавьте следующий конструктор в `GestureRecognizerView` . Этот конструктор добавит `ImageView` в действие. На этом этапе код по-прежнему не будет компилироваться &ndash; . нам нужно создать класс `MyScaleListener` , который поможет изменить размер, `ImageView` когда пользователь его сжимает:

  ```csharp
  public GestureRecognizerView(Context context): base(context, null, 0)
  {
      _icon = context.Resources.GetDrawable(Resource.Drawable.Icon);
      _icon.SetBounds(0, 0, _icon.IntrinsicWidth, _icon.IntrinsicHeight);
      _scaleDetector = new ScaleGestureDetector(context, new MyScaleListener(this));
  }
  ```

- Чтобы нарисовать изображение на нашем действии, необходимо переопределить `OnDraw` метод класса представления, как показано в следующем фрагменте кода. Этот код переместит в положение, указанное в, `ImageView` `_posX` а `_posY` также изменяет размер изображения в соответствии с коэффициентом масштабирования:

  ```csharp
  protected override void OnDraw(Canvas canvas)
  {
      base.OnDraw(canvas);
      canvas.Save();
      canvas.Translate(_posX, _posY);
      canvas.Scale(_scaleFactor, _scaleFactor);
      _icon.Draw(canvas);
      canvas.Restore();
  }
  ```

- Далее нам нужно обновить переменную экземпляра, `_scaleFactor` так как пользователь сжимает `ImageView` . Мы добавим класс с именем `MyScaleListener` . Этот класс будет прослушивать события масштабирования, которые будут вызываться Android, когда пользователь сжимает `ImageView` .
  Добавьте следующий внутренний класс в `GestureRecognizerView` . Этот класс является `ScaleGesture.SimpleOnScaleGestureListener` . Этот класс является удобным классом, который прослушиватели могут подклассировать, когда вы заинтересованы в подмножестве жестов:

  ```csharp
  private class MyScaleListener : ScaleGestureDetector.SimpleOnScaleGestureListener
  {
      private readonly GestureRecognizerView _view;

      public MyScaleListener(GestureRecognizerView view)
      {
          _view = view;
      }

      public override bool OnScale(ScaleGestureDetector detector)
      {
          _view._scaleFactor *= detector.ScaleFactor;

          // put a limit on how small or big the image can get.
          if (_view._scaleFactor > 5.0f)
          {
              _view._scaleFactor = 5.0f;
          }
          if (_view._scaleFactor < 0.1f)
          {
              _view._scaleFactor = 0.1f;
          }

          _view.Invalidate();
          return true;
      }
  }
  ```

- Следующий метод, который нужно переопределить в `GestureRecognizerView` , — это `OnTouchEvent` . В следующем коде показана полная реализация этого метода. Здесь есть большой объем кода, поэтому он займет пару минут и посмотрим, что происходит здесь. Первое, что делает этот метод, — это масштабирование значка, если &ndash; это необходимо, путем вызова метода `_scaleDetector.OnTouchEvent` . Далее мы пытаемся выяснить, какое действие вызвало этот метод:

  - Если пользователь затронул экран с помощью, запишите позиции X и Y и идентификатор первого указателя, который затронул экран.

  - Если пользователь переместил касание на экране, мы вычислим, насколько далеко Пользователь переместил указатель.

  - Если пользователь направит палец с экрана, он перестанет отслеживать жесты.

  ```csharp
  public override bool OnTouchEvent(MotionEvent ev)
  {
      _scaleDetector.OnTouchEvent(ev);

      MotionEventActions action = ev.Action & MotionEventActions.Mask;
      int pointerIndex;

      switch (action)
      {
          case MotionEventActions.Down:
          _lastTouchX = ev.GetX();
          _lastTouchY = ev.GetY();
          _activePointerId = ev.GetPointerId(0);
          break;

          case MotionEventActions.Move:
          pointerIndex = ev.FindPointerIndex(_activePointerId);
          float x = ev.GetX(pointerIndex);
          float y = ev.GetY(pointerIndex);
          if (!_scaleDetector.IsInProgress)
          {
              // Only move the ScaleGestureDetector isn't already processing a gesture.
              float deltaX = x - _lastTouchX;
              float deltaY = y - _lastTouchY;
              _posX += deltaX;
              _posY += deltaY;
              Invalidate();
          }

          _lastTouchX = x;
          _lastTouchY = y;
          break;

          case MotionEventActions.Up:
          case MotionEventActions.Cancel:
          // We no longer need to keep track of the active pointer.
          _activePointerId = InvalidPointerId;
          break;

          case MotionEventActions.PointerUp:
          // check to make sure that the pointer that went up is for the gesture we're tracking.
          pointerIndex = (int) (ev.Action & MotionEventActions.PointerIndexMask) >> (int) MotionEventActions.PointerIndexShift;
          int pointerId = ev.GetPointerId(pointerIndex);
          if (pointerId == _activePointerId)
          {
              // This was our active pointer going up. Choose a new
              // action pointer and adjust accordingly
              int newPointerIndex = pointerIndex == 0 ? 1 : 0;
              _lastTouchX = ev.GetX(newPointerIndex);
              _lastTouchY = ev.GetY(newPointerIndex);
              _activePointerId = ev.GetPointerId(newPointerIndex);
          }
          break;

      }
      return true;
  }
  ```

- Теперь запустите приложение и запустите действие распознавателя жестов.
  При запуске экран должен выглядеть примерно так, как показано на снимке экрана ниже:

  [![Экран запуска распознавателя жестов с помощью значка Android](android-touch-walkthrough-images/image17.png)](android-touch-walkthrough-images/image17.png#lightbox)

- Теперь коснитесь значка и перетащите его вокруг экрана. Попробуйте использовать жест сжатия для масштабирования. В некоторый момент экран может выглядеть примерно так, как на следующем снимке экрана:

  [![Значок перемещения по экрану](android-touch-walkthrough-images/image18.png)](android-touch-walkthrough-images/image18.png#lightbox)

На этом этапе вы должны дать себе точку зрения: вы только что реализовали сжатие в приложении Android! Выполните быстрый перерыв и перейдите к третьему и завершающему действию в этом пошаговом руководстве &ndash; с помощью пользовательских жестов.

## <a name="custom-gesture-activity"></a>Действие настраиваемого жеста

На последнем экране в этом пошаговом руководстве будут использоваться пользовательские жесты.

В рамках этого пошагового руководства библиотека жестов уже создана с помощью средства жеста и добавляется в проект в файл **ресурсов/необработанные/жесты**. Благодаря этому завершению работы в этом пошаговом руководстве можно приступить к выполнению окончательного действия.

- Добавьте файл макета с именем **Custom \_ жест \_ Layout. axml** в проект со следующим содержимым. В проекте уже есть все образы в папке **ресурсов** :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <ImageView
          android:src="@drawable/check_me"
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="3"
          android:id="@+id/imageView1"
          android:layout_gravity="center_vertical" />
      <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
  </LinearLayout>
  ```

- Затем добавьте новое действие в проект и присвойте ему имя `CustomGestureRecognizerActivity.cs` . Добавьте в класс две переменные экземпляра, как показано в следующих двух строках кода:

  ```csharp
  private GestureLibrary _gestureLibrary;
  private ImageView _imageView;
  ```

- Измените `OnCreate` метод этого действия, чтобы он соответствовал следующему коду. Потратьте пару минут, чтобы объяснить, что происходит в этом коде. В первую очередь мы создаем экземпляр `GestureOverlayView` класса и устанавливаем его в качестве корневого представления действия.
  Мы также присваиваем обработчик событий для `GesturePerformed` события `GestureOverlayView` . Далее мы изменим файл макета, который был создан ранее, и добавим его в качестве дочернего представления `GestureOverlayView` . Последним шагом является инициализация переменной `_gestureLibrary` и загрузка файла жестов из ресурсов приложения. Если не удается загрузить файл жестов по какой-либо причине, это действие не может быть завершено, поэтому оно будет выключено.

  ```csharp
  protected override void OnCreate(Bundle bundle)
  {
      base.OnCreate(bundle);

      GestureOverlayView gestureOverlayView = new GestureOverlayView(this);
      SetContentView(gestureOverlayView);
      gestureOverlayView.GesturePerformed += GestureOverlayViewOnGesturePerformed;

      View view = LayoutInflater.Inflate(Resource.Layout.custom_gesture_layout, null);
      _imageView = view.FindViewById<ImageView>(Resource.Id.imageView1);
      gestureOverlayView.AddView(view);

      _gestureLibrary = GestureLibraries.FromRawResource(this, Resource.Raw.gestures);
      if (!_gestureLibrary.Load())
      {
          Log.Wtf(GetType().FullName, "There was a problem loading the gesture library.");
          Finish();
      }
  }
  ```

- Наконец, нам нужно реализовать метод, `GestureOverlayViewOnGesturePerformed` как показано в следующем фрагменте кода. Когда `GestureOverlayView` обнаруживает жест, он выполняет обратный вызов к этому методу. Первое, что мы пытаемся получить `IList<Prediction>` объекты, соответствующие жесту, вызвав `_gestureLibrary.Recognize()` . Мы используем немного LINQ для получения объекта `Prediction` , имеющего наибольшую оценку для жеста.

  Если подходящий жест с достаточно высоким показателем нет, обработчик событий завершает работу без выполнения каких-либо действий. В противном случае мы проверяем имя прогноза и изменим отображаемое изображение на основе имени жеста:

  ```csharp
  private void GestureOverlayViewOnGesturePerformed(object sender, GestureOverlayView.GesturePerformedEventArgs gesturePerformedEventArgs)
  {
      IEnumerable<Prediction> predictions = from p in _gestureLibrary.Recognize(gesturePerformedEventArgs.Gesture)
      orderby p.Score descending
      where p.Score > 1.0
      select p;
      Prediction prediction = predictions.FirstOrDefault();

      if (prediction == null)
      {
          Log.Debug(GetType().FullName, "Nothing seemed to match the user's gesture, so don't do anything.");
          return;
      }

      Log.Debug(GetType().FullName, "Using the prediction named {0} with a score of {1}.", prediction.Name, prediction.Score);

      if (prediction.Name.StartsWith("checkmark"))
      {
          _imageView.SetImageResource(Resource.Drawable.checked_me);
      }
      else if (prediction.Name.StartsWith("erase", StringComparison.OrdinalIgnoreCase))
      {
          // Match one of our "erase" gestures
          _imageView.SetImageResource(Resource.Drawable.check_me);
      }
  }
  ```

- Запустите приложение и запустите действие распознавателя пользовательских жестов. Он должен выглядеть примерно так, как на следующем снимке экрана:

  [![Снимок экрана с изображением для проверки](android-touch-walkthrough-images/image19.png)](android-touch-walkthrough-images/image19.png#lightbox)

  Теперь нарисуйте флажок на экране, и отображаемое растровое изображение должно выглядеть примерно так, как показано на следующих снимках экрана:

  [![Отображается галочка, флажок распознан](android-touch-walkthrough-images/image20.png)](android-touch-walkthrough-images/image20.png#lightbox)

  Наконец, нарисуйте каракулю на экране. Флажок должен вернуться к исходному изображению, как показано на снимках экрана:

  [![Рисованная кривая на экране, отображается исходное изображение](android-touch-walkthrough-images/image21.png)](android-touch-walkthrough-images/image21.png#lightbox)

Теперь вы понимаете, как интегрировать сенсоры и жесты в приложении Android с помощью Xamarin. Android.

## <a name="related-links"></a>Связанные ссылки

- [Запуск Android Touch (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Окончательное касание Android (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)