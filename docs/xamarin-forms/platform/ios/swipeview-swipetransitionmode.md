---
Title: "Свипевиевный режим перехода на iOS" Описание: "особенности платформы позволяют использовать функции, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая управляет переходом, используемым при открытии Свипевиев.
MS. произв. Xamarin MS. AssetID: C667F24C-BAD8-47E0-9285-D3546BEF703B MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 12/11/2019 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="swipeview-swipe-transition-mode-on-ios"></a>Режим перехода Свипевиевного считывания в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот элемент управления, относящийся к платформе iOS, управляет переходом, который используется при открытии `SwipeView` . Он используется в XAML путем задания `SwipeView.SwipeTransitionMode` свойству BIND значения `SwipeTransitionMode` перечисления.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SwipeView ios:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `SwipeView.SetSwipeTransitionMode`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления переходом, который используется при открытии `SwipeView` . `SwipeTransitionMode`Перечисление предоставляет два возможных значения:

- `Reveal`Указывает, что прокрутка элементов будет выдаваться по мере `SwipeView` прокрутки содержимого, и является значением свойства по умолчанию `SwipeView.SwipeTransitionMode` .
- `Drag`Указывает, что прокрутка элементов будет перемещена в представление при `SwipeView` прокрутке содержимого.

Кроме того, `SwipeView.GetSwipeTransitionMode` метод можно использовать для возврата объекта `SwipeTransitionMode` , который применяется к `SwipeView` .

В результате заданное `SwipeTransitionMode` значение применяется к элементу `SwipeView` , который управляет переходом, используемым при открытии `SwipeView` :

[![Снимок экрана Свипевиев Свипетранситионмодес в iOS](swipeview-swipetransitionmode-images/swipetransitionmode.png "Свипетранситионмодес в iOS")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Свипетранситионмодес в iOS")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
