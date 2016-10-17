# Data-binding

### 1. 介绍  
`data-binding-library`是Google公司出品的第三方开源库.  
  
简而言之，最大的一个特点是在布局文件`xml`中引入变量和`Java`类，这些变量可以用`Java`定义，使布局十分灵活和方便。  
  
若深究一些，则可以发现`data-binding`机制是使用了`MVVM`设计模式。`MVVM`即`Model-View-ViewModel`，`ViewModel`的数据变化，直接会在`View`上面体现出来。


### 2. 导入
基于`Andoird Studio`开发，则在`module`所属的`build.gradle`文件中加入：  
```
android{
			xxx
			...
			dataBinding{
					enable = true
			}
}
```
另外，需要在项目的`build.gradle`中加入：
```
dependencies{
		...
		classpath 'com.android.databinding:dataBinder:1.0-rc0'
}
```

### 3. 使用

 - **例子1：简单的显示`JavaBean`内容**

假设我们有实体类`User`，其中有两个字符串成员变量`name`和`age`。使用`data-binding`，需要为实体类设置`getter`。
  
```
public class User {

    private String name;
    private String age;

    public User(String age, String name) {

        this.age = age;
        this.name = name;
    }

    public String getName() {

        return name;
    }

    public String getAge() {

        return age;
    }
}
```

--  

  对于`layout`文件，最外层的节点必须为`<layout></layout>`，在其中通过`<data>`标签引入`variable`，或者使用`import`导入系统自带的类。  
  而在控件中，利用`@{bean.xxx}`则可以直接引入数据。
  
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">

    <data>
    
        <variable
            name="user"
            type="com.mindjet.data_binding_sample.User"/>

    </data>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="horizontal">

        <TextView
            android:id="@+id/tv_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.name}"/>

    </LinearLayout>

</layout>

```

--  

对于使用了`<layout>`节点的`xml`布局文件，`databinding`会自动为其生成`Binding`类，如此处的`activity_main.xml`则生成`ActivityMainBinding.java`，利用该类可以实现数据的绑定。

```
ActivityMainBinding mBinding = DataBindingUtil.setContentView(R.layout.activity_main);
User user = new User("21","Mindjet");
mBinding.setUser(user);

```

---


 - **例子2：`ListView`与`Data-binding`**

