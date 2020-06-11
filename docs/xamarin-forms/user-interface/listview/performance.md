---
Title: "производительность ListView" Описание: "ListView является мощным представлением для отображения данных, но имеет некоторые ограничения. В этой статье объясняется, как обеспечить высокую производительность с помощью Xamarin.Forms ListView в приложении. "
MS. произв. Xamarin MS. AssetID: 1B085639-652C-4862-86EB-5D55D32B9395 MS. Technology: Xamarin-Forms author: давидбритч MS. author: дабритч МС. Дата: 12/11/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-performance"></a>Производительность ListView

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

При написании мобильных приложений имеет значение производительность. Пользователям приходится ждать плавной прокрутки и быстрой загрузки. Если не удовлетворить ожидания пользователей, вы сможете получить оценку в магазине приложений или в случае бизнес-приложения, изменяя время и деньги Организации.

Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Является мощным представлением для отображения данных, но имеет некоторые ограничения. Производительность прокрутки может снизиться при использовании пользовательских ячеек, особенно если они содержат глубоко вложенные иерархии представлений или используют определенные макеты, требующие сложного измерения. К счастью, существуют приемы, которые можно использовать во избежание низкой производительности.

## <a name="caching-strategy"></a>Стратегия кэширования

ListView часто используется для отображения гораздо большего объема данных, чем на экране. Например, музыкальное приложение может иметь библиотеку песен с тысячами записей. Создание элемента для каждой записи приведет к лишнему потреблению ценной памяти и будет работать плохо. Для постоянного создания и уничтожения строк приложение должно постоянно создавать и очищать объекты, что также будет работать плохо.

Для экономии памяти собственные [`ListView`](xref:Xamarin.Forms.ListView) эквиваленты для каждой платформы имеют встроенные функции для повторного использования строк. Только ячейки, видимые на экране, загружаются в память, а **содержимое** загружается в существующие ячейки. Этот шаблон запрещает приложению создавать экземпляры тысяч объектов, экономя время и память.

Xamarin.Formsразрешает [`ListView`](xref:Xamarin.Forms.ListView) повторное использование ячеек через [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) перечисление, которое имеет следующие значения:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Универсальная платформа Windows (UWP) игнорирует [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) стратегию кэширования, так как она всегда использует кэширование для повышения производительности. Таким образом, по умолчанию он ведет себя так, как если бы [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) стратегия кэширования была применена.

### <a name="retainelement"></a>ретаинелемент

[`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)Стратегия кэширования указывает, что [`ListView`](xref:Xamarin.Forms.ListView) компонент будет формировать ячейку для каждого элемента в списке и является поведением по умолчанию `ListView` . Его следует использовать в следующих случаях:

- Каждая ячейка имеет большое количество привязок (20 – 30 +).
- Шаблон ячейки часто меняется.
- Тестирование показывает, что `RecycleElement` стратегия кэширования приводит к снижению скорости выполнения.

Важно понимать последствия [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) стратегии кэширования при работе с пользовательскими ячейками. Любой код инициализации ячейки должен выполняться для каждого создания ячейки, что может быть несколько раз в секунду. В этом случае приемы макета, которые хорошо были на странице, например использование нескольких вложенных [`StackLayout`](xref:Xamarin.Forms.StackLayout) экземпляров, становятся узким местом производительности, когда они настраиваются и уничтожаются в режиме реального времени при прокрутке пользователем.

### <a name="recycleelement"></a>рециклилемент

[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)Стратегия кэширования указывает, что пытается [`ListView`](xref:Xamarin.Forms.ListView) сократить объем памяти и скорость выполнения, перезапуская ячейки списка. Этот режим не всегда обеспечивает повышение производительности, поэтому для определения каких-либо улучшений необходимо выполнить тестирование. Однако это предпочтительный вариант, и его следует использовать в следующих случаях.

- Каждая ячейка имеет небольшое и умеренное количество привязок.
- Каждая ячейка [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) определяет все данные ячейки.
- Каждая ячейка почти похожа, при этом шаблон ячейки не изменяется.

Во время виртуализации для ячейки будет обновлен контекст привязки, поэтому если приложение использует этот режим, оно должно обеспечить правильную обработку обновлений контекста привязки. Все данные о ячейке должны поступать из контекста привязки или ошибок согласованности. Эту проблему можно избежать с помощью привязки данных для вывода данных ячейки. Кроме того, данные ячейки должны быть заданы в `OnBindingContextChanged` переопределении, а не в конструкторе пользовательской ячейки, как показано в следующем примере кода:

```csharp
public class CustomCell : ViewCell
{
    Image image = null;
    
    public CustomCell ()
    {
        image = new Image();
        View = image;
    }
    
    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();
        
        var item = BindingContext as ImageItem;
        if (item != null) {
            image.Source = item.ImageUrl;
        }
    }
}
```

Дополнительные сведения см. в разделе [изменение контекста привязки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

В iOS и Android, если ячейки используют пользовательские модули подготовки отчетов, они должны обеспечить правильную реализацию уведомлений об изменении свойств. Когда ячейки повторно используются, их значения свойств изменятся при обновлении контекста привязки до значения доступной ячейки с `PropertyChanged` вызываемыми событиями. Дополнительные сведения см. [в разделе Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### <a name="recycleelement-with-a-datatemplateselector"></a>Рециклилемент с DataTemplateSelector

Если [`ListView`](xref:Xamarin.Forms.ListView) компонент использует [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , то [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) стратегия кэширования не кэширует `DataTemplate` s. Вместо этого `DataTemplate` для каждого элемента данных в списке выбирается.

> [!NOTE]
> [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)Стратегия кэширования имеет предварительные требования, представленные в Xamarin.Forms 2,4, что при [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) запросе на выбор [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , каждый из которых `DataTemplate` должен возвращать один и тот же [`ViewCell`](xref:Xamarin.Forms.ViewCell) тип. Например, при наличии объекта [`ListView`](xref:Xamarin.Forms.ListView) с типом `DataTemplateSelector` , который может возвращать либо `MyDataTemplateA` (где `MyDataTemplateA` возвращает `ViewCell` тип `MyViewCellA` ), либо `MyDataTemplateB` (где `MyDataTemplateB` возвращает `ViewCell` тип `MyViewCellB` ), когда возвращается значение, `MyDataTemplateA` оно должно возвращать значение, `MyViewCellA` иначе возникнет исключение.

### <a name="recycleelementanddatatemplate"></a>рециклилементанддататемплате

[`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate)Стратегия кэширования основана на [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) стратегии кэширования, а также гарантирует, что при использовании [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) для выбора [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , `DataTemplate` s кэшируется типом элемента в списке. Таким образом, `DataTemplate` Выбранные элементы выбираются один раз для каждого типа элемента, а не один раз для каждого экземпляра элемента.

> [!NOTE]
> [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate)Стратегия кэширования имеет предварительные требования, которые `DataTemplate` возвраты [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) должны использовать [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) конструктор, который принимает `Type` .

### <a name="set-the-caching-strategy"></a>Настройка стратегии кэширования

[`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)Значение перечисления указывается с помощью [`ListView`](xref:Xamarin.Forms.ListView) перегрузки конструктора, как показано в следующем примере кода:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

В XAML задайте атрибут, `CachingStrategy` как показано в XAML ниже:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Этот метод оказывает тот же результат, что и установка аргумента стратегии кэширования в конструкторе в C#.

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>Настройка стратегии кэширования в ListView с подклассом

Установка `CachingStrategy` атрибута из XAML в подклассе [`ListView`](xref:Xamarin.Forms.ListView) не приведет к желаемому поведению, так как отсутствует `CachingStrategy` свойство в `ListView` . Кроме того, если [XAMLC](~/xamarin-forms/xaml/xamlc.md) включен, будет выдаваться следующее сообщение об ошибке: **для "качингстратеги" не найдено свойство, связываемое свойство или событие** .

Решением этой проблемы является указание конструктора в подклассе [`ListView`](xref:Xamarin.Forms.ListView) , который принимает [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) параметр и передает его в базовый класс:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Затем [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) значение перечисления можно указать из XAML с помощью `x:Arguments` синтаксиса:

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>Предложения по повышению производительности ListView

Существует множество методов повышения производительности `ListView` . Следующие рекомендации могут повысить производительность ListView

- Привяжите `ItemsSource` свойство к `IList<T>` коллекции, а не к `IEnumerable<T>` коллекции, так как `IEnumerable<T>` коллекции не поддерживают произвольный доступ.
- Используйте встроенные ячейки (например `TextCell`  /  `SwitchCell` ,), а не `ViewCell` всякий раз, когда это возможно.
- Используйте меньше элементов. Например, можно использовать одну метку, `FormattedString` а не несколько меток.
- Замените на `ListView` `TableView` при отображении неоднородных данных, то есть данных различных типов.
- Ограничьте использование [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) метода. Если он используется, это приведет к снижению производительности.
- В Android не следует задавать `ListView` видимость разделителя строк или цвет после создания экземпляра, так как это приводит к значительному снижению производительности.
- Избегайте изменения макета ячейки на основе [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) . Изменение макета влечет за собой большие затраты на измерение и инициализацию.
- Избегайте глубоко вложенных иерархий макетов. Использование `AbsoluteLayout` или `Grid` для сокращения вложенности.
- Старайтесь не использовать `LayoutOptions` `Fill` не только ( `Fill` самый дешевый для вычислений).
- Старайтесь не размещать `ListView` внутри a `ScrollView` по следующим причинам:
  - В `ListView` реализуется собственная прокрутка.
  - `ListView`Не будет получать никакие жесты, так как они будут обрабатываться родительским объектом `ScrollView` .
  - `ListView`Может представлять настраиваемый верхний и нижний колонтитулы, которые прокручивается с элементами списка, потенциально предлагая функциональные возможности, которые `ScrollView` использовались для. Дополнительные сведения см. в разделе [верхние и нижние колонтитулы](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Рассмотрите пользовательский модуль подготовки отчетов, если вам нужна конкретная, сложная структура, представленная в ячейках.

`AbsoluteLayout`имеет возможность выполнять макеты без единичного вызова меры, делая ее очень производительной. Если `AbsoluteLayout` не может использоваться, рассмотрите возможность [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . При использовании `RelativeLayout` Передача ограничений напрямую будет значительно быстрее, чем с помощью API Expression. Этот метод выполняется быстрее, поскольку API выражений использует JIT, а в iOS дерево необходимо интерпретировать, что является медленным. API выражений подходит для макетов страниц, где он необходим только для первоначального макета и вращения, но в `ListView` , где он постоянно запускается во время прокрутки, он вредит производительность.

Создание пользовательского модуля подготовки отчетов для [`ListView`](xref:Xamarin.Forms.ListView) или его ячеек является одним из подходов к уменьшению влияния вычисления макета на производительность при прокрутке. Дополнительные сведения см. в разделе [Настройка ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) и [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## <a name="related-links"></a>Связанные ссылки

- [Настраиваемое представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Пользовательский модуль подготовки к просмотру ViewCell (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [листвиевкачингстратеги](xref:Xamarin.Forms.ListViewCachingStrategy)
