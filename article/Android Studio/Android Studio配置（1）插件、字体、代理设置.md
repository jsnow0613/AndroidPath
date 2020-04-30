## Android Studio配置（1）插件、字体、代理设置

### 一、 插件

#### 1.json格式化工具：

将json格式的代码转为实体类

* GsonFormat(java)
* JsonToKotlin(Kotlin)



#### 2.Lifecycle Sorter

将Activity或者Fragment的生命周期进行排序

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gebuus85hvj312p0u0n2x.jpg)

#### 3.代码补全

* Android Postfix Completion(only java)
* Custom Postfix Templates(support kotlin)

#### 4.Translation

翻译插件





### 二、 设置字体
    1.代码字体：
        字体：JetBrains Mono
        字号：16
        行距：1.2
    
    2. 侧边栏字体大小：
        在appearance内，设置 14
        
    3. logcat设置颜色
        在AndroidStudio设置内搜索logcat，然后设置颜色

### 三、 代理
    由于gradle代理需要http，所以需要将ss转为http（ssr自带功能），然后在Android Studio中设置为http代理