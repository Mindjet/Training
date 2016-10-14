# Data-binding

### 1. 介绍  
`data-binding-library`是Google公司出品的第三方开源库，个人认为最大的一个特点是在布局文件`xml`中引入变量和`Java`类，这些变量可以用`Java`定义，使布局十分灵活和方便。


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

 - **例子2：`ListView`与`Data-binding`**
