
# SlideBack
无需继承的Activity侧滑返回库 类全面屏返回手势效果 仿“即刻”侧滑返回
[![](https://jitpack.io/v/ParfoisMeng/SlideBack.svg)](https://jitpack.io/#ParfoisMeng/SlideBack)

---

### 前情
最近一直在研究侧滑返回效果的实现，目前比较多的方案如下：

 1. 背景透明主题。问题是性能与神坑"Only fullscreen activities can request
    orientation"。
 2. 将上页ContentView绘制到当前页，侧滑时动画推入推出。（也许挺不错？）
 3. 类全面屏返回手势。[即刻App](https://www.ruguoapp.com/)的效果（下图）。

本库这里选择了方案3。

### 预览
![即刻App](https://github.com/ParfoisMeng/SlideBack/blob/master/screenshot/jike.gif)
![本库](https://github.com/ParfoisMeng/SlideBack/blob/master/screenshot/mine.gif)
[Demo下载](https://github.com/ParfoisMeng/SlideBack/blob/master/demo/demo.apk)

### 使用
 - 引用类库 *请将last-version替换为最新版本号 [![](https://jitpack.io/v/ParfoisMeng/SlideBack.svg)](https://jitpack.io/#ParfoisMeng/SlideBack)
```
	// 1.添加jitpack仓库
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
	// 2.添加项目依赖（last-version替换为最新版本号）
	dependencies {
		implementation 'com.github.parfoismeng:slideback:last-version'
	}
```
- 代码使用
```
// Kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // 在需要滑动返回的Activity中注册
        SlideBack.register(this) {
            Toast.makeText(this, "SlideBack", Toast.LENGTH_SHORT).show()
        }
    }

    override fun onDestroy() {
        super.onDestroy()

        // onDestroy时记得解绑
        // 内部使用WeakHashMap，理论上不解绑也行，但最好还是手动解绑一下
        SlideBack.unregister(this)
    }
}

// Java
public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 在需要滑动返回的Activity中注册
        SlideBack.register(this, new SlideBackCallBack() {
            @Override
            public void onSlideBack() {
                Toast.makeText(SecondActivity.this, "SlideBack", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();

        // onDestroy时记得解绑
        // 内部使用WeakHashMap，理论上不解绑也行，但最好还是手动解绑一下
        SlideBack.unregister(this);
    }
}
```
OJBK！So easy！

### 性能
附一张性能截图。可以看出来中间进行了很多次 onCreate & onDestory，最后内存和开始时一致：
![
MEMORY](https://github.com/ParfoisMeng/SlideBack/blob/master/screenshot/memory.png)

### 感谢
感谢 [ChenTianSaber](https://github.com/ChenTianSaber)  的开源库 [SlideBack](https://github.com/ChenTianSaber/SlideBack) （[掘金](https://juejin.im/post/5b7a837cf265da432f653617)）提供的思路与源码

### 更新
1. 删除无用依赖，添加Java引用示例 - 1.0.2
2. 检查警告，修改类名，更新README.md - 1.0.1
3. 初版发布 - 1.0.0

### 计划
1. 目前还是依赖了v7包，作用仅为@ColorInt和@Nullable约束，要不要保留呢？
2. 提交个Kotlin版本（其实AS直接转换就行...）
3. 写个源码分析（一共也就不到200行代码，看看就差不多了吧...）
4. 看情况吧......

### 支持
劳烦各位大佬给个Star让我出去好装B行嘛！

<img src="https://github.com/ParfoisMeng/SlideBack/blob/master/screenshot/alipay.jpg" width="320px"/>  <img src="https://github.com/ParfoisMeng/SlideBack/blob/master/screenshot/wechat.png" width="320px"/>
