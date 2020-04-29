# Fragment

## 前言

`Fragment` 表示 `FragmentActivity` 中的行为或界面的一部分。您可以在一个 Activity 中组合多个片段，从而构建多窗格界面，并在多个 Activity 中重复使用某个片段。您可以将片段视为 Activity 的模块化组成部分，它具有自己的生命周期，能接收自己的输入事件，并且您可以在 Activity 运行时添加或移除片段（这有点像可以在不同 Activity 中重复使用的“子 Activity”）。



## 一、Fragment的生命周期

![fragment_lifecycle](https://tva1.sinaimg.cn/large/007S8ZIlgy1geafs43y80j308t0nj3zh.jpg)







## 二、Fragment的使用

### 2.1 静态使用

在Activity布局中声明：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment android:name="com.example.news.ArticleListFragment"
            android:id="@+id/list"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
    <fragment android:name="com.example.news.ArticleReaderFragment"
            android:id="@+id/viewer"
            android:layout_weight="2"
            android:layout_width="0dp"
            android:layout_height="match_parent" />
</LinearLayout>
```

`<fragment>`中的`android:name` 属性指定要在布局中进行实例化的 `Fragment` 类。

### 2.2 动态添加

在 Activity 运行期间，您可以随时将片段添加到 Activity 布局中。您只需指定要将片段放入哪个 `ViewGroup`。

如要在您的 Activity 中执行片段事务（如添加、移除或替换片段），则必须使用 `FragmentTransaction` 中的 API。如下所示，您可以从 `FragmentActivity` 获取一个 `FragmentTransaction` 实例：

```kotlin
val fragmentManager = supportFragmentManager
val fragmentTransaction = fragmentManager.beginTransaction()
```

然后，您可以使用 `add()` 方法添加一个片段，指定要添加的片段以及将其插入哪个视图。例如：

```kotlin
val fragment = ExampleFragment()
fragmentTransaction.add(R.id.fragment_container, fragment)
fragmentTransaction.commit()
```

传递到 `add()` 的第一个参数是 `ViewGroup`，即应放置片段的位置，由资源 ID 指定，第二个参数是要添加的片段。

一旦您通过 `FragmentTransaction` 做出了更改，就必须调用 `commit()` 以使更改生效。



## 三、与Activity通信

### 3.1 Activity中获取Fragment数据

使用 `findFragmentById()` 或 `findFragmentByTag()`，通过从 `FragmentManager` 获取对 `Fragment` 的引用来调用片段中的方法。例如：https://developer.android.com/guide/components/fragments#java)

```kotlin
val fragment = supportFragmentManager.findFragmentById(R.id.example_fragment) as ExampleFragment
```

### 3.2 Fragment中获取Activity的数据

可通过 `getActivity()` 访问 `FragmentActivity` 实例，并轻松执行在 Activity 布局中查找视图等任务：

```kotlin
val listView: View? = activity?.findViewById(R.id.list)
```

### 3.3 创建回调

在Fragment中创建回调接口，并在Activity中实现

```kotlin
public class FragmentA : ListFragment() {

    var listener: OnArticleSelectedListener? = null
    ...
    override fun onAttach(context: Context) {
        super.onAttach(context)
        listener = context as? OnArticleSelectedListener
        if (listener == null) {
            throw ClassCastException("$context must implement 		OnArticleSelectedListener")
        }

    }
    ...
}
```

### 3.4 使用ViewModel

```kotlin
    class SharedViewModel : ViewModel() {
        val selected = MutableLiveData<Item>()

        fun select(item: Item) {
            selected.value = item
        }
    }

    class MasterFragment : Fragment() {

        private lateinit var itemSelector: Selector

        private lateinit var model: SharedViewModel

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            model = activity?.run {
                ViewModelProviders.of(this)[SharedViewModel::class.java]
            } ?: throw Exception("Invalid Activity")
            itemSelector.setOnClickListener { item ->
                // Update the UI
            }
        }
    }

    class DetailFragment : Fragment() {

        private lateinit var model: SharedViewModel

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            model = activity?.run {
                ViewModelProviders.of(this)[SharedViewModel::class.java]
            } ?: throw Exception("Invalid Activity")
            model.selected.observe(this, Observer<Item> { item ->
                // Update the UI
            })
        }
    }
    
```