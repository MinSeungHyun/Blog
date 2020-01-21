---
title: "[Android] Data Binding(데이터 바인딩) 반복되는 코드 줄이기(BaseActivity)"
categories: [Android]
tags: [Android, DataBinding, Boilerplate, BaseActivity]
---
---
[**Data Binding**](https://developer.android.com/topic/libraries/data-binding)은 코드를 간결하고 구조 있게 만들어줍니다.  
데이터 바인딩을 사용하기 위해 우리는 항상 아래와 같은 코드를 작성합니다.

```kotlin
//MainActivity.kt
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
        binding.apply {
            //Do something
        }
    }
}
```
얼마 안되는 길이의 코드이긴 하지만 이런 Boilerplate 코드가 생기면 좋지 않습니다.  
## 그러면 어떻게 해결하나요?
이미 아시는 분들은 많이 쓰고 있는 방법인데요,  
**BaseActivity를 만들고 이를 상속해서 쓰는 것입니다.**
```kotlin
//BaseActivity.kt
abstract class BaseActivity<T : ViewDataBinding>(@LayoutRes private val layoutResId: Int) : AppCompatActivity() {
    protected lateinit var binding: T

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, layoutResId)
    }
}
```
이런 식으로 만들어두면 데이터 바인딩을 사용하는 Activity에서 아래와 같이 사용할 수 있게 됩니다.
```kotlin
//MainActivity.kt
class MainActivity : BaseActivity<ActivityMainBinding>(R.layout.activity_main) {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding.apply {
            //Do something
        }
    }
}
```
## 장점
- 데이터 바인딩뿐만 아니라 Activity들이 공통적으로 수행해야 하는 코드가 있다면 BaseActivity를 이용할 수 있다.
- 따라서 Bolierplate 코드가 줄어든다.

## 단점
[이 링크](https://www.androidpub.com/index.php?mid=devfree&document_srl=2431162)에 BaseActivity를 사용하는 것에 대해 사람들이 의견을 남겨두었더라고요.  
간단하게 요약하자면
- AppCompatActivity가 아니라 특정한 Activity를 상속해야만 하는 경우가 있다.  
=> 어쩔 수 없이 BaseActivity를 쓰지 못한다.
- BaseActivity가 변경되면 이를 상속한 모든 Activity들이 변경되는 것이므로 부담이 크다. (Side Effect)
- 공동작업을 할 때 다른 사람이 코드를 이해하기 어렵다.

## 결론
어떻게 보면 단점이 많아 보이지만 적절하게 사용하면 그만큼의 장점이 있으니 잘 판단해서 사용하시면 되겠습니다. 😄