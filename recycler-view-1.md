# RecyclerView-1

## 简介

>  
>The RecyclerView is a more advanced and more flexible version of the ListView.  
>  


　　在`Android Lollipop`推出时，`Google`也推出了这个新控件。`ListView`是平时开发很常用的控件，但是控件比较原始，很多东西都需要自己配置。`RecyclerView`作为`ListView`的替代品，并没有继承于后者，而直接继承于`ViewGroup`。  

　　在使用方面，`RecyclerView`采用插拔式的设计，解耦性很强，而且扩展性十分惊人，可以实现的效果也很多。  

　　`RecyclerView`使用`LayoutManager`来控制布局，通过`ItemAnimation`来设定插入/删除的动画。

---

## 导入
在项目`module`的`build.gradle`下加入：

```
compile 'com.android.support:recyclerview-v7:24.2.1'
```
注意版本号，我的`compileSDK`是`24`，所以我用的`24`开头的版本，这需要你根据你的`compileSDK`进行修改。

---


## 使用
### 1. 设置布局样式

```
mRecyclerView.setLayoutManager(new LinearLayoutManager(this));
```
当然还有其他样式：

```
mRecyclerView.setLayoutManager(new GridLayoutManager(this, 2));  //2，代表一行有两个项
mRecyclerView.setLayoutManager(new StaggeredGridLayoutManager(2, OrientationHelper.VERTICAL));  //其中第二个参数代表滑动的方向，2代表与滑动方向垂直的方向的项目数
```  

### 2. 绑定适配器

```
mRecyclerView.setAdapter(new NormalTextViewAdapter(this));
```

### 3. 编写适配器

编写适配器包括以下几个过程：  

#### 确定泛型 

```
public class NormalRecyclerViewAdapter extends RecyclerView.Adapter<NormalRecyclerViewAdapter.NormalTextViewHolder>
```

　　由于我们这里只有一种类型的列表项，即文字列表项。我们只需要编写一种`ViewHolder`，这里定义为`NormalTextViewHolder`，作为一个静态内部类。

#### 编写ViewHolder

```
public static class NormalTextViewHolder extends RecyclerView.ViewHolder{

    TextView mTextView;

    public NormalTextViewHolder(View itemView) {
        super(itemView);

        mTextView = (TextView) itemView.findViewById(R.id.id_text);
        itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d("NormalTextViewHolder","onClick--->position="+getPosition());
            }
        });

    }
}
```

所有的`ViewHolder`都要继承于`RecyclerView.ViewHolder`，在`ViewHolder`实现**控件的实例化**和**监听器的绑定**等操作。

#### 重写onCreateViewHolder方法

```
@Override
public NormalTextViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    return new NormalTextViewHolder(mLayoutInflater.inflate(R.layout.text_item,parent,false));
}
```

　　我们只有一种类型的列表项，故不必判断`viewType`的数值，直接返回一个`NormalTextViewHolder`对象即可。创建对象的构造方法中需要传入`View`类型的参数，这里使用`LayoutInflater.inflate`方法将列表项的`xml`文件加载进来。

#### 编写onBindViewHolder方法

```
@Override
public void onBindViewHolder(NormalTextViewHolder holder, int position) {
    holder.mTextView.setText(Titles[position]);
}
```
在该方法中主要处理的就是控件数据的绑定，即给`TextView`设置文字，给`ImageView`设置图片等。

#### 完整代码

```
package com.mindjet.recyclerviewdemo;

import android.content.Context;
import android.support.v7.widget.RecyclerView;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by mindjet on 19/10/2016.
 */
public class NormalRecyclerViewAdapter extends RecyclerView.Adapter<NormalRecyclerViewAdapter.NormalTextViewHolder> {

    private Context mContext;
    private LayoutInflater mLayoutInflater;
    private String[] Titles;

    public NormalRecyclerViewAdapter(Context context) {

        this.mContext = context;
        this.mLayoutInflater = LayoutInflater.from(context);

    }

    public void setTitles(String[] titles) {
        Titles = titles;
    }

    @Override
    public NormalTextViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        return new NormalTextViewHolder(mLayoutInflater.inflate(R.layout.text_item,parent,false));
    }

    @Override
    public void onBindViewHolder(NormalTextViewHolder holder, int position) {

        holder.mTextView.setText(Titles[position]);

    }

    @Override
    public int getItemCount() {
        return Titles == null ? 0 : Titles.length;
    }

    public static class NormalTextViewHolder extends RecyclerView.ViewHolder{

        TextView mTextView;

        public NormalTextViewHolder(View itemView) {
            super(itemView);

            mTextView = (TextView) itemView.findViewById(R.id.id_text);
            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Log.d("NormalTextViewHolder","onClick--->position="+getPosition());
                }
            });

        }
    }

}
```

### 4. 效果
- `LinearLayoutManager`:  
<img src="screenshots/recycler-view-screen-1.png" width="250"/>  


- `GridLayoutManager`:  
<img src="screenshots/recycler-view-screen-2.png" width="250"/>


---

## 总结
`RecyclerView`果然好用，是时候抛弃`ListView`了。

---

*更多关于`RecyclerView`，请看[RecyclerView-2](recycler-view-2.md)和[RecyclerView-3](recycler-view-3.md)。*





