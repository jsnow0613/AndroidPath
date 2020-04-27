# Activity的知识总结

## 前言

Activity是我们最常见到，也接触最多的一个组件，也是与用户交互最直接的功能，这篇文章总结了Activity的相关知识点

结构目录：

- 生命周期
- 启动模式
- Activity之间的跳转



## 一、生命周期

### 1.1	Activity状态

#### 1.1.1	运行状态

Activity处于栈顶

#### 1.1.2	暂停状态

Activity不在栈顶，但处于可见状态，且不可操作

#### 1.1.3	停止状态

Activity不在栈顶，且完全不可见

#### 1.1.4	销毁状态

Activity从返回栈中移除

### 1.2	Activity生命周期函数

![activity生命周期](https://tva1.sinaimg.cn/large/007S8ZIlgy1ge3uy5or3rj30e90ifwfj.jpg)

如上图，这是Google官方文档上给出的生命周期图，下面我们来一一讲解

#### 1.2.1	onCreate()

每次启动Activity的时候会调用该方法，一般会在此初始化视图

#### 1.2.2	onStart()

1. 执行**onCreate()**方法后，会接着调用此方法。
2. 执行**onRestart()**方法后，会接着调用此方法。如：从其他Activity返回到该Activity，或者从后台重新进入前台

#### 1.2.3	onResume()

执行**onStart()**方法后，会接着调用此方法。此方法执行完后，Activity进入运行状态，可与用户交互

#### 1.2.4	onPause()

当该方法执行后，界面**处于可见不可交互**状态，如：进入新的透明Activity  
**onRestart()和onStart()**方法

#### 1.2.5	onStop()

当Activity不可见时，进入此状态。如：

- 进入新的非透明Activity
- 进入后台

#### 1.2.6	onRestart()

App从后台切换到前台时

#### 1.2.7	onDestroy()

#### **tips：**

​	当Activity A，启动了透明Activity B后，它们的回调为：

​	A:	onPause()

​	B:	onCreate() -->	onStart()	-->	onResume()

​	若此时按home键将App置于后台，则回调为：

​	A:	onStop()

​	B:	onPause() -->	onStop()

​	再将App切换到前台时：

​	A:	onRestart()	-->	onStart()

​	B:	onRestart()	-->	onStart()	-->onResume()



### 1.3	Task

> 任务是用户在执行某项工作时与之互动的一系列 Activity 的集合。

这是官方文档对task的解释，task使用栈的形式来管理activity，一个App可以不止一个task

### 1.4	启动模式

当Activity A启动Activity B时，新的Activity B会进入栈顶并获得焦点，Activity A仍会保留在栈中，当B销毁时，A会再次处于栈顶状态，Activity的栈遵从**“先进后出”**的运作方式，参考官方图：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ge43ljj48lj30h505f0t2.jpg)



#### 1.4.1	standard（默认）

默认值。系统在启动该 Activity 的任务中创建 Activity 的新实例，并将 intent 传送给该实例。Activity 可以多次实例化，每个实例可以属于不同的任务，一个任务可以拥有多个实例。

#### 1.4.2	singleTop

如果当前任务的顶部已存在 Activity 的实例，则系统会通过调用其 `onNewIntent()` 方法来将 intent 转送给该实例，而不是创建 Activity 的新实例。Activity 可以多次实例化，每个实例可以属于不同的任务，一个任务可以拥有多个实例（但前提是返回堆栈顶部的 Activity 不是该 Activity 的现有实例）。

#### 1.4.3	singleTask

1. 判断是否设置了独立的taskAffinity属性值，若没有，它会在已有的任务中查看是否已经存在相应的Activity实例，如果存在，就会把位于这个Activity实例上面的Activity全部结束掉，即最终这个Activity实例会位于任务的堆栈顶端中。
2. 若设置了独立的taskAffinity属性值，就会在新任务中启动。因此，如果我们想要设置了"singleTask"启动模式的Activity在新的任务中启动，就要为它设置一个独立的taskAffinity属性值。
3. 如果您启动了指定 `singleTask` 启动模式的 Activity，而后台任务中已存在该 Activity 的实例，则系统会将该后台任务整个转到前台运行。此时，返回堆栈包含了转到前台的任务中的所有 Activity，这些 Activity 都位于堆栈的顶部

![backstack_singletask_multiactivity](https://tva1.sinaimg.cn/large/007S8ZIlgy1ge84jg9v18j30fa08l0tc.jpg)

#### 1.4.4	singleInstance

与 "singleTask" 相似，唯一不同的是系统不会将任何其他 Activity 启动到包含该实例的任务中。该 Activity 始终是其任务唯一的成员；由该 Activity 启动的任何 Activity 都会在其他的任务中打开。



#### 参考

1. https://developer.android.com/guide/components/activities/intro-activities



