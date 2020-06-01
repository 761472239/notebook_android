[TOC]

参考[Android AndroidX的迁移 - 简书](https://www.jianshu.com/p/7dc111353328)

# 1、AndroidX变化

##  1.1、常用依赖库对比

| Old build artifact                                         | AndroidX build artifact                              |
| ---------------------------------------------------------- | ---------------------------------------------------- |
| android.arch.core:common                                   | androidx.arch.core:core-common                       |
| android.arch.core:core                                     | androidx.arch.core:core                              |
| android.arch.core:core-testing                             | androidx.arch.core:core-testing                      |
| android.arch.core:runtime                                  | androidx.arch.core:core-runtime                      |
| android.arch.lifecycle:common                              | androidx.lifecycle:lifecycle-common                  |
| android.arch.lifecycle:common-java8                        | androidx.lifecycle:lifecycle-common-java8            |
| android.arch.lifecycle:compiler                            | androidx.lifecycle:lifecycle-compiler                |
| android.arch.lifecycle:extensions                          | androidx.lifecycle:lifecycle-extensions              |
| android.arch.lifecycle:livedata                            | androidx.lifecycle:lifecycle-livedata                |
| android.arch.lifecycle:livedata-core                       | androidx.lifecycle:lifecycle-livedata-core           |
| android.arch.lifecycle:reactivestreams                     | androidx.lifecycle:lifecycle-reactivestreams         |
| android.arch.lifecycle:runtime                             | androidx.lifecycle:lifecycle-runtime                 |
| android.arch.lifecycle:viewmodel                           | androidx.lifecycle:lifecycle-viewmodel               |
| android.arch.paging:common                                 | androidx.paging:paging-common                        |
| android.arch.paging:runtime                                | androidx.paging:paging-runtime                       |
| android.arch.paging:rxjava2                                | androidx.paging:paging-rxjava2                       |
| android.arch.persistence.room:common                       | androidx.room:room-common                            |
| android.arch.persistence.room:compiler                     | androidx.room:room-compiler                          |
| android.arch.persistence.room:guava                        | androidx.room:room-guava                             |
| android.arch.persistence.room:migration                    | androidx.room:room-migration                         |
| android.arch.persistence.room:runtime                      | androidx.room:room-runtime                           |
| android.arch.persistence.room:rxjava2                      | androidx.room:room-rxjava2                           |
| android.arch.persistence.room:testing                      | androidx.room:room-testing                           |
| android.arch.persistence:db                                | androidx.sqlite:sqlite                               |
| android.arch.persistence:db-framework                      | androidx.sqlite:sqlite-framework                     |
| com.android.support.constraint:constraint-layout           | androidx.constraintlayout:constraintlayout           |
| com.android.support.constraint:constraint-layout-solver    | androidx.constraintlayout:constraintlayout-solver    |
| com.android.support.test.espresso.idling:idling-concurrent | androidx.test.espresso.idling:idling-concurrent      |
| com.android.support.test.espresso.idling:idling-net        | androidx.test.espresso.idling:idling-net             |
| com.android.support.test.espresso:espresso-accessibility   | androidx.test.espresso:espresso-accessibility        |
| com.android.support.test.espresso:espresso-contrib         | androidx.test.espresso:espresso-contrib              |
| com.android.support.test.espresso:espresso-core            | androidx.test.espresso:espresso-core                 |
| com.android.support.test.espresso:espresso-idling-resource | androidx.test.espresso:espresso-idling-resource      |
| com.android.support.test.espresso:espresso-intents         | androidx.test.espresso:espresso-intents              |
| com.android.support.test.espresso:espresso-remote          | androidx.test.espresso:espresso-remote               |
| com.android.support.test.espresso:espresso-web             | androidx.test.espresso:espresso-web                  |
| com.android.support.test.janktesthelper:janktesthelper     | androidx.test.jank:janktesthelper                    |
| com.android.support.test.services:test-services            | androidx.test:test-services                          |
| com.android.support.test.uiautomator:uiautomator           | androidx.test.uiautomator:uiautomator                |
| com.android.support.test:monitor                           | androidx.test:monitor                                |
| com.android.support.test:orchestrator                      | androidx.test:orchestrator                           |
| com.android.support.test:rules                             | androidx.test:rules                                  |
| com.android.support.test:runner                            | androidx.test:runner                                 |
| com.android.support:animated-vector-drawable               | androidx.vectordrawable:vectordrawable-animated      |
| com.android.support:appcompat-v7                           | androidx.appcompat:appcompat                         |
| com.android.support:asynclayoutinflater                    | androidx.asynclayoutinflater:asynclayoutinflater     |
| com.android.support:car                                    | androidx.car:car                                     |
| com.android.support:cardview-v7                            | androidx.cardview:cardview                           |
| com.android.support:collections                            | androidx.collection:collection                       |
| com.android.support:coordinatorlayout                      | androidx.coordinatorlayout:coordinatorlayout         |
| com.android.support:cursoradapter                          | androidx.cursoradapter:cursoradapter                 |
| com.android.support:customtabs                             | androidx.browser:browser                             |
| com.android.support:customview                             | androidx.customview:customview                       |
| com.android.support:design                                 | com.google.android.material:material                 |
| com.android.support:documentfile                           | androidx.documentfile:documentfile                   |
| com.android.support:drawerlayout                           | androidx.drawerlayout:drawerlayout                   |
| com.android.support:exifinterface                          | androidx.exifinterface:exifinterface                 |
| com.android.support:gridlayout-v7                          | androidx.gridlayout:gridlayout                       |
| com.android.support:heifwriter                             | androidx.heifwriter:heifwriter                       |
| com.android.support:interpolator                           | androidx.interpolator:interpolator                   |
| com.android.support:leanback-v17                           | androidx.leanback:leanback                           |
| com.android.support:loader                                 | androidx.loader:loader                               |
| com.android.support:localbroadcastmanager                  | androidx.localbroadcastmanager:localbroadcastmanager |
| com.android.support:media2                                 | androidx.media2:media2                               |
| com.android.support:media2-exoplayer                       | androidx.media2:media2-exoplayer                     |
| com.android.support:mediarouter-v7                         | androidx.mediarouter:mediarouter                     |
| com.android.support:multidex                               | androidx.multidex:multidex                           |
| com.android.support:multidex-instrumentation               | androidx.multidex:multidex-instrumentation           |
| com.android.support:palette-v7                             | androidx.palette:palette                             |
| com.android.support:percent                                | androidx.percentlayout:percentlayout                 |
| com.android.support:preference-leanback-v17                | androidx.leanback:leanback-preference                |
| com.android.support:preference-v14                         | androidx.legacy:legacy-preference-v14                |
| com.android.support:preference-v7                          | androidx.preference:preference                       |
| com.android.support:print                                  | androidx.print:print                                 |
| com.android.support:recommendation                         | androidx.recommendation:recommendation               |
| com.android.support:recyclerview-selection                 | androidx.recyclerview:recyclerview-selection         |
| com.android.support:recyclerview-v7                        | androidx.recyclerview:recyclerview                   |
| com.android.support:slices-builders                        | androidx.slice:slice-builders                        |
| com.android.support:slices-core                            | androidx.slice:slice-core                            |
| com.android.support:slices-view                            | androidx.slice:slice-view                            |
| com.android.support:slidingpanelayout                      | androidx.slidingpanelayout:slidingpanelayout         |
| com.android.support:support-annotations                    | androidx.annotation:annotation                       |
| com.android.support:support-compat                         | androidx.core:core                                   |
| com.android.support:support-content                        | androidx.contentpager:contentpager                   |
| com.android.support:support-core-ui                        | androidx.legacy:legacy-support-core-ui               |
| com.android.support:support-core-utils                     | androidx.legacy:legacy-support-core-utils            |
| com.android.support:support-dynamic-animation              | androidx.dynamicanimation:dynamicanimation           |
| com.android.support:support-emoji                          | androidx.emoji:emoji                                 |
| com.android.support:support-emoji-appcompat                | androidx.emoji:emoji-appcompat                       |
| com.android.support:support-emoji-bundled                  | androidx.emoji:emoji-bundled                         |
| com.android.support:support-fragment                       | androidx.fragment:fragment                           |
| com.android.support:support-media-compat                   | androidx.media:media                                 |
| com.android.support:support-tv-provider                    | androidx.tvprovider:tvprovider                       |
| com.android.support:support-v13                            | androidx.legacy:legacy-support-v13                   |
| com.android.support:support-v4                             | androidx.legacy:legacy-support-v4                    |
| com.android.support:support-vector-drawable                | androidx.vectordrawable:vectordrawable               |
| com.android.support:swiperefreshlayout                     | androidx.swiperefreshlayout:swiperefreshlayout       |
| com.android.support:textclassifier                         | androidx.textclassifier:textclassifier               |
| com.android.support:transition                             | androidx.transition:transition                       |
| com.android.support:versionedparcelable                    | androidx.versionedparcelable:versionedparcelable     |
| com.android.support:viewpager                              | androidx.viewpager:viewpager                         |
| com.android.support:wear                                   | androidx.wear:wear                                   |
| com.android.support:webkit                                 | androidx.webkit:webkit                               |

 更多详细变化内容，可以[下载CSV格式](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.android.google.cn%2Ftopic%2Flibraries%2Fsupport-library%2Fdownloads%2Fandroidx-artifact-mapping.csv)映射文件； 

##  1.2、常用支持库类对比

| Support Library class                      | AndroidX class                              |
| ------------------------------------------ | ------------------------------------------- |
| `android.support.v4.app.Fragment`          | `androidx.fragment.app.Fragment`            |
| `android.support.v4.app.FragmentActivity`  | `androidx.fragment.app.FragmentActivity`    |
| `android.support.v7.app.AppCompatActivity` | `androidx.appcompat.app.AppCompatActivity`  |
| `android.support.v7.app.ActionBar`         | `androidx.appcompat.app.ActionBar`          |
| `android.support.v7.widget.RecyclerView`   | `androidx.recyclerview.widget.RecyclerView` |

 更多详细变化内容，可以[下载CSV格式](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.android.google.cn%2Ftopic%2Flibraries%2Fsupport-library%2Fdownloads%2Fandroidx-class-mapping.csv)映射文件。 

# 2. AndroidX配置

**1）更新升级插件**

- 将AS更新至 **AS 3.2**及以上；
- Gradle 插件版本改为 **4.6**及以上；
   项目下 `gradle/wrapper/gradle-wrapper.propertie` 文件中的`distributionUrl`改为：



```ruby
distributionUrl=https\://services.gradle.org/distributions/gradle-4.6-all.zip
```

- compileSdkVersion 版本升级到 **28**及以上；
- buildToolsVersion 版本改为 **28.0.2**及以上。

![img](https:////upload-images.jianshu.io/upload_images/4625401-92ed6de990f27533.png?imageMogr2/auto-orient/strip|imageView2/2/w/546/format/webp)

插件更新提示

**2）开启迁移AndroidX**
 在项目的`gradle.properties`文件里添加如下配置：



```bash
android.useAndroidX=true
android.enableJetifier=true
```

 表示项目启用 AndroidX 并迁移到 AndroidX。

**3）替换依赖库**
 修改项目app目录下的`build.gradle`依赖库：



```bash
implementation 'com.android.support:appcompat-v7:28.0.2' → implementation 'androidx.appcompat:appcompat:1.0.0'
implementation 'com.android.support:design:28.0.2'  → implementation 'com.google.android.material:material:1.0.0'
implementation 'com.android.support.constraint:constraint-layout:1.1.2' → implementation 'androidx.constraintlayout:constraintlayout:1.1.2'
...
```

**4）修改支持库类**
 将原来`import`的`android.**`包删除，重新`import`新的`androidx.**`包；



```java
import android.support.v7.app.AppCompatActivity; → import androidx.appcompat.app.AppCompatActivity;
```

**5）一键迁移AndroidX库**
 AS 3.2 及以上版本提供了更加方便快捷的方法一键迁移到 AndroidX。选择菜单上的**ReFactor —— Migrate to AndroidX...** 即可。（如果迁移失败，就需要重复上面1，2，3，4步手动去修改迁移）

![img](https:////upload-images.jianshu.io/upload_images/4625401-b9524e8fa789d620.png?imageMogr2/auto-orient/strip|imageView2/2/w/316/format/webp)

AndroidX 迁移

**注意：**如果你的项目compileSdkVersion 低于28，点击Refactor to AndroidX...会提示：



```cpp
You need to have at least have compileSdk 28 set in your module build.gradle to refactor to androidx
```

提示让你使用不低于28的sdk，升级最新到SDK，然后点击 **Migrate to AndroidX...**，AS就会自动将项目重构并使用AndroidX库。

# 3. AndroidX迁移问题

[《Android Support库和AndroidX冲突问题》](https://www.jianshu.com/p/7507c25fd986)

# 4. AndroidX影响

  虽然说目前对我们没有多大影响，我们可以不使用仍然使用旧版本的支持库，毕竟没有强制，但长远来看还是有好处的。AndroidX重新设计了包结构，旨在鼓励库的小型化，支持库和架构组件包的名字也都简化了；而且也是减轻Android生态系统碎片化的有效方式。

# 参考

[AndroidX 官方文档，开启传送门](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.android.google.cn%2Fjetpack%2Fandroidx%2Fmigrate)